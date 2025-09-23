
```kotlin
package com.intellij.driver.tests.rubymine.project  
  
import com.intellij.driver.client.Driver  
import com.intellij.driver.sdk.ui.components.common.ideFrame  
import com.intellij.driver.sdk.ui.components.common.welcomeScreen  
import com.intellij.driver.sdk.ui.components.elements.button  
import com.intellij.driver.sdk.ui.components.elements.comboBox  
import com.intellij.driver.sdk.ui.components.elements.dialog  
import com.intellij.driver.sdk.ui.components.elements.jBlist  
import com.intellij.driver.sdk.ui.components.elements.textField  
import com.intellij.driver.sdk.ui.present  
import com.intellij.driver.sdk.ui.shouldBe  
import com.intellij.driver.sdk.ui.ui  
import com.intellij.driver.sdk.ui.xQuery  
import com.intellij.driver.sdk.waitForIndicators  
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
class CreateRailsApiProjectTest {  
  @Test  
  fun testCreateRubyProject() {  
    driver.withContext {  
      step("Create a new Rails API project") {  
        welcomeScreen {  
          shouldBe("No welcome screen is showing", present, timeout = 1.minutes)  
          createNewProjectButton.click()  
          ui.dialog(xQuery { byTitle("New Project") }) {  
            shouldBe("New Project dialog is not showing", present, timeout = 30.seconds)  
            jBlist(xQuery { byClass("JBList") }).clickItem("Rails API", fullMatch = false)  
  
            var rubyInterpreterSpecified = true  
            var railsVersionSpecified = true  
  
            step("Close No Ruby Interpreter Specified dialog") {  
              ui.dialog(xQuery { byTitle("No Ruby Interpreter Specified") }) {  
                rubyInterpreterSpecified = false  
                ui.button("OK").click()  
              }  
            }  
            step("Close No Rails Version Specified")  
            {  
              ui.dialog(xQuery { byTitle("No Rails Version Specified") }) {  
                railsVersionSpecified = false  
                ui.button("OK").click()  
              }  
            }  
  
            // TODO finish this, last one was buggy  
            if (!rubyInterpreterSpecified) {  
              step("Specify Ruby interpreter") {  
                val interpreterComboBox = comboBox(xQuery { and(byClass("ComboBox"),byText("Ruby interpreter:"))})  
                interpreterComboBox.selectItem(interpreterComboBox.listValues().first())  
              }  
            }  
  
  
  
            // textField(xQuery { byClass("JBTextField") }).text = "RailsAPITestProject"  
            button("Create").click()  
          }  
        }  
        step("Wait for project to be created") {  
          waitForIndicators(2.minutes)  
        }  
      }    }  }  
  
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


//if (!rubyInterpreterSpecified) {  
//  step("Specify Ruby interpreter") {  
//    print("ruby interpreter not specified")  
//    val rubyComboBox = comboBox { xQuery { "//div[@class='RubySdkComboBox']" } }  
//    print("VALUES !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!" + rubyComboBox.listValues())  
//    rubyComboBox.selectItem(rubyComboBox.listValues().first())  
//    //val interpreterComboBox = comboBox(xQuery { and(byClass("ComboBox"),byText("Ruby interpreter:"))})  
//    //interpreterComboBox.selectItem(interpreterComboBox.listValues().first())  
//  }  
//}  
//if (!railsVersionSpecified) {  
//  print("rails version not specified")  
//  val railsComboBox = comboBox { xQuery { "//div[@class='RailsVersionComboBox']" } }  
//  if (railsComboBox.listValues().isEmpty()) {  
//    print("no rails versions available")  
//    val addRails = button(xQuery { "//div[@defaulticon='add.svg']" })  
//    addRails.click()  
//  }  
//  else {  
//    railsComboBox.selectItem(railsComboBox.listValues().first())  
//  }  
//}



```