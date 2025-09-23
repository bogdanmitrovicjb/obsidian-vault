```kotlin
package com.intellij.driver.tests.rubymine.project  
  
import com.intellij.driver.client.Driver  
import com.intellij.driver.sdk.invokeAction  
import com.intellij.driver.sdk.ui.components.common.editorTabs  
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
import com.intellij.driver.tests.rubymine.utilities.extractVersionNumber  
import com.intellij.driver.tests.rubymine.utilities.ifElementPresent  
import com.intellij.ide.starter.driver.engine.BackgroundRun  
import com.intellij.ide.starter.driver.engine.runIdeWithDriver  
import com.intellij.ide.starter.extended.allure.Layers  
import com.intellij.ide.starter.extended.allure.Subsystems  
import com.intellij.ide.starter.extended.remdev.RemoteDevRun  
import com.intellij.ide.starter.ide.IDETestContext  
import com.intellij.ide.starter.ide.IdeProductProvider  
import com.intellij.ide.starter.junit5.newContext  
import com.intellij.ide.starter.report.AllureHelper.step  
import com.intellij.ide.starter.runner.Starter  
import org.junit.jupiter.api.AfterAll  
import org.junit.jupiter.api.Assertions.assertTrue  
import org.junit.jupiter.api.BeforeAll  
import org.junit.jupiter.api.Test  
import org.junit.jupiter.api.extension.ExtendWith  
import kotlin.time.Duration.Companion.minutes  
import kotlin.time.Duration.Companion.seconds  
  
@Layers.UI  
@Subsystems.Subsystem("Ruby projects")  
@ExtendWith(RemoteDevRun::class)  
class CreateGemProjectTest {  
  @Test  
  fun testCreateGemProject() {  
    driver.withContext {  
      step("Create a new Gem project") {  
        // TODO add a check if the popup "Waiting for Ruby Interpreters" hangs  
        welcomeScreen {  
          shouldBe("No welcome screen is showing", present, timeout = 1.minutes)  
          createNewProjectButton.click()  
          //waitForIndicators(1.minutes)  
          var rubyInterpreterSpecified = true  
  
          step("Close No Ruby Interpreter Specified dialog") {  
            ifElementPresent(  
              element = ui.dialog(xQuery { byTitle("No Ruby Interpreter Specified") }),  
              timeout = 1.seconds  
            ) { rubyDialog ->  
              rubyInterpreterSpecified = false  
              rubyDialog.button("OK").click()  
            }  
          }  
          ui.dialog(xQuery { byTitle("New Project") }) {  
            shouldBe("New Project dialog is not showing", present, timeout = 30.seconds)  
            jBlist(xQuery { byClass("JBList") }).clickItem("Gem", fullMatch = false)  
  
  
            step("Close No Ruby Interpreter Specified dialog") {  
              ifElementPresent(  
                element = ui.dialog(xQuery { byTitle("No Ruby Interpreter Specified") }),  
                timeout = 1.seconds  
              ) { rubyDialog ->  
                rubyInterpreterSpecified = false  
                rubyDialog.button("OK").click()  
              }  
            }  
            // TODO check if this should only be called if rubyInterpreterSpecified is false or always like it is now  
            val rubyComboBox = comboBox(xQuery { byClass("RubySdkComboBox") })  
            rubyComboBox.click()  
            val rubyList = jBlist(xQuery { contains(byVisibleText("ruby")) })  
            rubyList.shouldBe("Ruby interpreters not showing", present)  
            val highestVersion = rubyList.items.sortedByDescending { extractVersionNumber(it) }[0]  
            assertTrue(extractVersionNumber(highestVersion) >= 30100, "Ruby interpreter version 3.1.0 or higher not found")  
            rubyList.clickItem(highestVersion, fullMatch = false)  
            should(message = "Ruby interpreter is not selected") { rubyComboBox.getSelectedItem().contains("ruby") }  
  
            button("Create").click()  
          }  
        }  
        step("Downloading Bundler gems list")  
        {  
          val gemDialog = ui.dialog(title = "Downloading Bundler gems list")  
          // gemDialog.shouldBe("Downloading Bundler gems list should show", present, 10.seconds)  
          gemDialog.waitNotFound(30.seconds)  
        }  
  
        step("Install bundler Gem if needed") {  
          ifElementPresent(  
            element = ui.dialog(xQuery { byTitle("Install bundler Gem") }),  
            timeout = 1.minutes  
          ) { installBundlerDialog ->  
            installBundlerDialog.button("Install").click()  
          }  
        }  
        step("Try bundler init to check if bundler is working") {  
          waitForIndicators(1.minutes)  
          ideFrame {  
            invokeAction("org.jetbrains.plugins.ruby.gem.bundler.actions.RunBundlerInitAction")  
            waitForIndicators(1.minutes)  
            val currentTab = ui.editorTabs().getTabs().first()  
            assertTrue(currentTab.text.contains("Gemfile"), "Gemfile is not opened")  
          }  
        }      }    }  }  
  
  private companion object {  
    private lateinit var context: IDETestContext  
    private lateinit var bgRun: BackgroundRun  
    private lateinit var driver: Driver  
  
    @BeforeAll  
    @JvmStatic    fun startIde() {  
      context = Starter.newContext(testName = "testCreateRailsApiProject", ideInfo = IdeProductProvider.RM)  
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