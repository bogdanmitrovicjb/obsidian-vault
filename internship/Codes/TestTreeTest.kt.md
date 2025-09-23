
```kotlin
package com.intellij.driver.tests.rubymine.testTree  
  
import com.intellij.driver.client.Driver  
import com.intellij.driver.sdk.invokeAction  
import com.intellij.driver.sdk.ui.UiText.Companion.asString  
import com.intellij.driver.sdk.ui.components.UiComponent.Companion.waitFound  
import com.intellij.driver.sdk.ui.components.common.ideFrame  
import com.intellij.driver.sdk.ui.components.common.navigationBar  
import com.intellij.driver.sdk.ui.components.common.toolwindows.projectView  
import com.intellij.driver.sdk.ui.components.elements.popup  
import com.intellij.driver.sdk.ui.components.elements.tree  
import com.intellij.driver.sdk.ui.ui  
import com.intellij.driver.sdk.wait  
import com.intellij.driver.sdk.waitForIndicators  
import com.intellij.driver.tests.rubymine.actions.rubyActions  
import com.intellij.driver.tests.rubymine.actions.rubyVersionActions  
import com.intellij.driver.tests.utils.openFile  
import com.intellij.ide.starter.driver.engine.BackgroundRun  
import com.intellij.ide.starter.driver.engine.runIdeWithDriver  
import com.intellij.ide.starter.extended.allure.AllureHelperExtended.step  
import com.intellij.ide.starter.extended.allure.Layers  
import com.intellij.ide.starter.extended.allure.Subsystems  
import com.intellij.ide.starter.extended.data.cases.RubyMineCases  
import com.intellij.ide.starter.ide.IDETestContext  
import com.intellij.ide.starter.runner.Starter  
import io.qameta.allure.AllureId  
import org.junit.jupiter.api.AfterAll  
import org.junit.jupiter.api.Assertions.assertTrue  
import org.junit.jupiter.api.BeforeAll  
import org.junit.jupiter.api.Test  
import kotlin.time.Duration.Companion.minutes  
import kotlin.time.Duration.Companion.seconds  
  
@Layers.UI  
@Subsystems.Subsystem("Test Tree")  
class TestTreeTest {  
  @Test  
  @AllureId("286928")  
  fun testTestTree() {  
    driver.withContext {  
      ideFrame {  
        waitForIndicators(1.minutes)  
        rubyVersionActions.selectRubyFromSettings("3.3.6")  
        waitForIndicators(1.minutes)  
        openFile("app/models/user.rb")  
        invokeAction("GotoTest")  
        wait(2.seconds)  
        invokeAction("GotoTest")  
        wait(2.seconds)  
        invokeAction("GotoTest")  
        rubyActions.clickGutterIcon(3, "run")  
        popup().waitOneText("Run 'Minitest: UserTest'").click()  
        waitForIndicators(1.minutes)  
  
        // test/models/user_test.rb  
  
  
        step("Run tests and check result") {  
          //leftToolWindowToolbar.projectButton.open()  
          //projectView().waitFound().waitAnyTextsContains("test_car_unittest.py").first().rightClick()          //fileContextMenu.waitFound().waitAnyTextsContains("Run 'Python tests in test").first().click()          showPassedTests.click()  
          wait(2.seconds)  
          showPassedTests.click()  
          println("TREE STRINGS!!! :${driver.ui.x("//div[@class='SMTRunnerTestTreeView']").waitFound().getAllTexts().asString()}")  
          val result = testsOutput.getAllTexts().asString()  
          println("RESULTS!!!!!!: $result")  
          //assertTrue(unitTestExpectedOutput in result, "'$unitTestExpectedOutput' not found in '$result'")  
        }  
      }  
    }  }  
  
  private companion object {  
    private lateinit var context: IDETestContext  
    private lateinit var bgRun: BackgroundRun  
    private lateinit var driver: Driver  
    private val showPassedTests  
      get() = driver.ui.x("//div[@accessiblename='Show Passed']")  
    private val testsOutput  
      get() = driver.ui.x("//div[@accessiblename='Editor']")  
    private val fileContextMenu  
      get() = driver.ui.x("//div[@class='MyMenu']")  
  
    @BeforeAll  
    @JvmStatic    fun startIde() {  
      context = Starter.newContext(testCase = RubyMineCases.RailsSampleProject, testName = "testTestTree")  
      context.applyVMOptionsPatch {  
        addLine("-Dexpose.ui.hierarchy.url=true")  
      }  
      bgRun = context.runIdeWithDriver(runTimeout = 5.minutes)  
      driver = bgRun.driver  
      driver.rubyActions.deleteRubyProjects()  
    }  
  
    @AfterAll  
    @JvmStatic    fun waitUntilClosed() {  
      bgRun.closeIdeAndWait()  
    }  
  }  
}
```