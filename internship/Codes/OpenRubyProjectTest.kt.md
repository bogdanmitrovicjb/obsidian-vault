```kotlin
package com.intellij.driver.tests.rubymine.project  
  
import com.intellij.driver.client.Driver  
import com.intellij.driver.sdk.invokeAction  
import com.intellij.driver.sdk.ui.components.common.ideFrame  
import com.intellij.driver.sdk.ui.components.common.welcomeScreen  
import com.intellij.driver.sdk.ui.components.elements.button  
import com.intellij.driver.sdk.ui.components.elements.dialog  
import com.intellij.driver.sdk.ui.components.elements.jBlist  
import com.intellij.driver.sdk.ui.components.elements.textField  
import com.intellij.driver.sdk.ui.present  
import com.intellij.driver.sdk.ui.shouldBe  
import com.intellij.driver.sdk.ui.ui  
import com.intellij.driver.sdk.ui.xQuery  
import com.intellij.driver.sdk.waitForIndicators  
import com.intellij.driver.tests.clion.tests.openProjectTests.OpenCompilationDatabaseTest  
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
class OpenRubyProjectTest {  
  
  @Test  
  fun testOpenRubyProject() {  
    driver.withContext {  
      step("Create a new Ruby project") {  
        welcomeScreen {  
          shouldBe("No welcome screen is showing", present, timeout = 1.minutes)  
          createNewProjectButton.click()  
          ui.dialog(xQuery { byTitle("New Project") }) {  
            shouldBe("New Project dialog is not showing", present, timeout = 30.seconds)  
            jBlist(xQuery { byClass("JBList") }).clickItem("Empty Project", fullMatch = false)  
            jBlist(xQuery { byClass("JBList") }).clickItem("Gem", fullMatch = false)  
            step("Close No Ruby Interpreter Specified dialog") {  
              ui.dialog(xQuery { byTitle("No Ruby Interpreter Specified") }) {  
                ui.button("OK").click()  
              }  
            }            jBlist(xQuery { byClass("JBList") }).clickItem("Empty Project", fullMatch = false)  
            //textField(xQuery { byClass("JBTextField") }).text = "RubyTestProject" TODO should remain untitled, remove everywhere  
            button("Create").click()  
          }  
        }        waitForIndicators(2.minutes)  
  
        step("Wait for project to be created") {  
          // TODO remove as this is unnecessary since it's after waitForIndicators  
          ideFrame {  
            shouldBe("IDE frame is not showing", present, timeout = 1.minutes)  
  
            projectName = project!!.getName()  
            runCatching {  
                closeProject()  
            }  
            welcomeScreen {  
              shouldBe("No welcome screen is showing", present, timeout = 1.minutes)  
              openProjectButton.click()  
  
            }  
          }        }      }    }  }  
  
  private companion object {  
    private lateinit var context: IDETestContext  
    private lateinit var bgRun: BackgroundRun  
    private lateinit var driver: Driver  
  
    private lateinit var projectPath: String  
    private lateinit var projectName: String  
  
    @BeforeAll  
    @JvmStatic    fun startIde() {  
      context = Starter.newContext(testName = "testOpenRubyProject", ideInfo = IdeProductProvider.RM)  
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