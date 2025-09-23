


```kotlin
package com.intellij.driver.tests.rubymine.consoles  
  
import com.intellij.driver.client.Driver  
import com.intellij.driver.sdk.invokeAction  
import com.intellij.driver.sdk.ui.components.UiComponent.Companion.waitFound  
import com.intellij.driver.sdk.ui.components.common.JEditorUiComponent  
import com.intellij.driver.sdk.ui.components.common.dialogs.terminal  
import com.intellij.driver.sdk.ui.components.common.editor  
import com.intellij.driver.sdk.ui.components.common.ideFrame  
import com.intellij.driver.sdk.ui.components.common.welcomeScreen  
import com.intellij.driver.sdk.ui.components.elements.button  
import com.intellij.driver.sdk.ui.components.elements.comboBox  
import com.intellij.driver.sdk.ui.components.elements.dialog  
import com.intellij.driver.sdk.ui.components.elements.jBlist  
import com.intellij.driver.sdk.ui.present  
import com.intellij.driver.sdk.ui.should  
import com.intellij.driver.sdk.ui.shouldBe  
import com.intellij.driver.sdk.ui.ui  
import com.intellij.driver.sdk.ui.xQuery  
import com.intellij.driver.sdk.waitForIndicators  
import com.intellij.driver.tests.rubymine.steps.rubyProjectSteps  
import com.intellij.driver.tests.rubymine.utilities.extractVersionNumber  
import com.intellij.ide.starter.driver.engine.BackgroundRun  
import com.intellij.ide.starter.driver.engine.runIdeWithDriver  
import com.intellij.ide.starter.extended.allure.Layers  
import com.intellij.ide.starter.extended.allure.Subsystems  
import com.intellij.ide.starter.extended.data.cases.RubyMineCases  
import com.intellij.ide.starter.ide.IDETestContext  
import com.intellij.ide.starter.ide.IdeProductProvider  
import com.intellij.ide.starter.junit5.newContext  
import com.intellij.ide.starter.report.AllureHelper.step  
import com.intellij.ide.starter.runner.Starter  
import org.junit.jupiter.api.AfterAll  
import org.junit.jupiter.api.Assertions.assertTrue  
import org.junit.jupiter.api.BeforeAll  
import org.junit.jupiter.api.Test  
import kotlin.time.Duration.Companion.minutes  
import kotlin.time.Duration.Companion.seconds  
  
@Layers.UI  
@Subsystems.Subsystem("Ruby consoles")  
class IrbConsoleTest {  
  
  @Test  
  fun testIrbConsole() {  
    // action id za rails console je org.jetbrains.plugins.ruby.rails.console.RunRailsConsoleAction  
    // action id za irb console je org.jetbrains.plugins.ruby.console.RunIRBConsoleAction    // action id za pry console je org.jetbrains.plugins.ruby.console.RunPryConsoleAction    driver.withContext {  
      step("Initialize environment")  
      {  
        rubyProjectSteps.waitForRubyInterpretersScanning()  
      }  
      step("Create a new Ruby project") {  
        welcomeScreen {  
          shouldBe("No welcome screen is showing", present, timeout = 1.minutes)  
          createNewProjectButton.click()  
          ui.dialog(xQuery { byTitle("New Project") }) {  
  
            val rubyComboBox = comboBox(xQuery { byClass("RubySdkComboBox") })  
            rubyComboBox.click()  
            assertTrue(rubyComboBox.listValues().isNotEmpty(), "Ruby Interpreters not showing")  
            val highestVersion = rubyComboBox.listValues().sortedByDescending { extractVersionNumber(it) }[0]  
            assertTrue(extractVersionNumber(highestVersion) >= 30306, "Ruby interpreter version 3.3.6 or higher not found")  
            rubyComboBox.selectItem(highestVersion)  
            should(message = "Ruby interpreter is not selected") { rubyComboBox.getSelectedItem().contains("ruby") }  
  
            shouldBe("New Project dialog is not showing", present, timeout = 30.seconds)  
            jBlist(xQuery { byClass("JBList") }).clickItem("Empty Project", fullMatch = false)  
            button("Create").click()  
          }  
        }        waitForIndicators(2.minutes)  
      }  
      step("Check if the project was created successfully") {  
        ideFrame {  
          invokeAction("org.jetbrains.plugins.ruby.console.RunIRBConsoleAction")  
          val irbConsole = ui.x("//div[@class='IrbRubyLanguageConsoleView']")  
          irbConsole.waitFound(timeout = 10.seconds)  
          val idk = xx(JEditorUiComponent::class.java) { byClass("EditorComponentImpl") }.list().last()  
          idk.waitFound(timeout = 10.seconds)  
          print("FOUND!!!!!")  
          idk.setFocus()  
          keyboard {  
            typeText("put")  
          }  
          val suggestions = x("//div[@class='LookupList']")  
          suggestions.waitFound(timeout = 10.seconds)  
          var suggestionsTexts = suggestions.getAllTexts()  
          print(suggestionsTexts)  
          print(suggestionsTexts.toString())  
          print("count: ${suggestionsTexts.size}\n")  
          keyboard {  
            down()  
          }  
          suggestionsTexts = suggestions.getAllTexts()  
          print(suggestionsTexts)  
          print(suggestionsTexts.toString())  
          print("count: ${suggestionsTexts.size}\n")  
          print("FOUND!!!!!")  
          val interpretersPopup = driver.ui.x { contains(byVisibleText("Scanning for Ruby Interpreters")) }  
          interpretersPopup.waitFound(10.minutes)  
        }  
      }    }  }  
  
  private companion object {  
    private lateinit var context: IDETestContext  
    private lateinit var bgRun: BackgroundRun  
    private lateinit var driver: Driver  
  
    @BeforeAll  
    @JvmStatic    fun startIde() {  
      context = Starter.newContext(testName = "testIrbConsole", ideInfo = IdeProductProvider.RM)  
      bgRun = context.runIdeWithDriver(runTimeout = 5.minutes)  
      driver = bgRun.driver  
    }  
  
    @AfterAll  
    @JvmStatic    fun waitUntilClosed() {  
      bgRun.closeIdeAndWait()  
    }  
  }  
}
```