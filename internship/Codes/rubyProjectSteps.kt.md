```kotlin
package com.intellij.driver.tests.rubymine.steps  
  
import com.intellij.driver.client.Driver  
import com.intellij.driver.sdk.ui.components.common.welcomeScreen  
import com.intellij.driver.sdk.ui.components.elements.*  
import com.intellij.driver.sdk.ui.present  
import com.intellij.driver.sdk.ui.shouldBe  
import com.intellij.driver.sdk.ui.ui  
import com.intellij.driver.sdk.ui.xQuery  
import com.intellij.driver.sdk.waitForIndicators  
import com.intellij.ide.starter.extended.allure.AllureHelperExtended.step  
import kotlin.time.Duration.Companion.minutes  
import kotlin.time.Duration.Companion.seconds  
  
val Driver.rubyProjectSteps get() = RubyProjectOpener(this)  
  
class RubyProjectOpener(private val driver: Driver) {  
  fun createNewProjectFromWelcomeScreen() {  
    driver.run {  
      step("Create a new Ruby project") {  
        welcomeScreen {  
          shouldBe("No welcome screen is showing", present, timeout = 1.minutes)  
          createNewProjectButton.click()  
        }  
  
        step("Configure new project") {  
          ui.dialog(xQuery { byTitle("New Project") }) {  
            shouldBe("New Project dialog is not showing", present, timeout = 30.seconds)  
            jBlist(xQuery { byClass("JBList") }).clickItem("Empty Project", fullMatch = false)  
  
            // Set project name  
            textField(xQuery { byClass("JBTextField") }).text = "RubyTestProject"  
  
            // Click Create button  
            button("Create").click()  
          }  
        }  
        step("Wait for project to be created") {  
          waitForIndicators(2.minutes)  
        }  
      }    }  }  
}
```