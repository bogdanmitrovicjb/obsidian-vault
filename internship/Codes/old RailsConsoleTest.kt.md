
```kotlin
package com.intellij.driver.tests.rubymine.railsConsole  
  
import com.intellij.driver.client.Driver  
import com.intellij.driver.sdk.invokeAction  
import com.intellij.driver.sdk.ui.components.common.ideFrame  
import com.intellij.driver.sdk.ui.components.common.welcomeScreen  
import com.intellij.driver.sdk.ui.components.elements.button  
import com.intellij.driver.sdk.ui.components.elements.comboBox  
import com.intellij.driver.sdk.ui.components.elements.dialog  
import com.intellij.driver.sdk.ui.components.elements.jBlist  
import com.intellij.driver.sdk.ui.components.elements.popup  
import com.intellij.driver.sdk.ui.components.elements.textField  
import com.intellij.driver.sdk.ui.present  
import com.intellij.driver.sdk.ui.should  
import com.intellij.driver.sdk.ui.shouldBe  
import com.intellij.driver.sdk.ui.ui  
import com.intellij.driver.sdk.ui.xQuery  
import com.intellij.driver.sdk.waitForIndicators  
import com.intellij.driver.tests.rubymine.utilities.ifElementPresent  
import java.awt.event.KeyEvent  
import com.intellij.ide.starter.driver.engine.BackgroundRun  
import com.intellij.ide.starter.driver.engine.runIdeWithDriver  
import com.intellij.ide.starter.extended.remdev.RemoteDevRun  
import com.intellij.ide.starter.ide.IDETestContext  
import com.intellij.ide.starter.ide.IdeProductProvider  
import com.intellij.ide.starter.junit5.newContext  
import com.intellij.ide.starter.report.AllureHelper.step  
import com.intellij.ide.starter.runner.Starter  
import org.junit.jupiter.api.AfterAll  
import org.junit.jupiter.api.BeforeAll  
import org.junit.jupiter.api.Test  
import org.junit.jupiter.api.extension.ExtendWith  
import kotlin.time.Duration.Companion.minutes  
import kotlin.time.Duration.Companion.seconds  
  
@ExtendWith(RemoteDevRun::class)  
class RailsConsoleTest {  
  @Test  
  fun testRailsConsole() {  
    driver.withContext {  
      step("Create a new Rails API project") {  
        welcomeScreen {  
          shouldBe("No welcome screen is showing", present, timeout = 1.minutes)  
          createNewProjectButton.click()  
          ui.dialog(xQuery { byTitle("New Project") }) {  
            shouldBe("New Project dialog is not showing", present, timeout = 30.seconds)  
            jBlist(xQuery { byClass("JBList") }).clickItem("Application", fullMatch = false)  
  
            var rubyInterpreterSpecified = true  
            var railsVersionSpecified = true  
  
            step("Close No Ruby Interpreter Specified dialog") {  
              ifElementPresent(  
                element = ui.dialog(xQuery { byTitle("No Ruby Interpreter Specified") }),  
                timeout = 1.seconds  
              ) { rubyDialog ->  
                rubyInterpreterSpecified = false  
                rubyDialog.button("OK").click()  
              }  
            }  
            step("Close No Rails Version Specified")  
            {  
              ifElementPresent(  
                element = ui.dialog(xQuery { byTitle("No Rails Version Specified") }),  
                timeout = 1.seconds  
              ) { railsDialog ->  
                railsVersionSpecified = false  
                railsDialog.button("OK").click()  
              }  
            }  
            rubyInterpreterSpecified = true  
            railsVersionSpecified = true  
  
            jBlist(xQuery { byClass("JBList") }).clickItem("Rails API", fullMatch = false)  
  
  
            step("Close No Ruby Interpreter Specified dialog 2") {  
              ifElementPresent(  
                element = ui.dialog(xQuery { byTitle("No Ruby Interpreter Specified") }),  
                timeout = 1.seconds  
              ) { rubyDialog ->  
                rubyInterpreterSpecified = false  
                rubyDialog.button("OK").click()  
              }  
            }  
            step("Close No Rails Version Specified 2")  
            {  
              ifElementPresent(  
                element = ui.dialog(xQuery { byTitle("No Rails Version Specified") }),  
                timeout = 1.seconds  
              ) { railsDialog ->  
                railsVersionSpecified = false  
                railsDialog.button("OK").click()  
              }  
            }  
            print("status is: Ruby selected " + rubyInterpreterSpecified + " rails version selected " + railsVersionSpecified)  
  
            if (!rubyInterpreterSpecified) {  
              val rubyComboBox = comboBox(xQuery { byClass("RubySdkComboBox") })  
              rubyComboBox.click()  
              jBlist(xQuery { contains(byVisibleText("ruby")) }).clickItem("ruby", fullMatch = false)  
              should(message = "Ruby interpreter is not selected") { rubyComboBox.getSelectedItem().contains("ruby") }  
              rubyInterpreterSpecified = true  
            }  
  
            if (!railsVersionSpecified) {  
  
              val railsComboBox = comboBox(xQuery { byClass("RailsVersionComboBox") })  
              railsComboBox.click()  
              // TODO this almost never works, add back the PLUS code for adding rails versions when it's fixed  
              jBlist(xQuery { contains(byVisibleText("8.0.2")) }).clickItem("8.0.2", fullMatch = false)  
              should(message = "Rails version is not selected") { railsComboBox.getSelectedItem().contains("8.0.2") }  
              railsVersionSpecified = true  
            }  
  
            button("Create").click()  
          }  
        }  
        step("Wait for project to be created") {  
          waitForIndicators(2.minutes)  
        }  
        step("Open Rails console") {  
  
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