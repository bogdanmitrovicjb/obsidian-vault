


```kotlin
package com.intellij.driver.tests.rubymine.debugger  
  
import com.intellij.driver.client.Driver  
import com.intellij.driver.sdk.openFile  
import com.intellij.driver.sdk.ui.components.UiComponent.Companion.waitFound  
import com.intellij.driver.sdk.ui.components.common.ideFrame  
import com.intellij.driver.sdk.ui.components.common.mainToolbar  
import com.intellij.driver.sdk.ui.components.elements.button  
import com.intellij.driver.sdk.ui.components.elements.checkBoxTree  
import com.intellij.driver.sdk.ui.components.settings.settingsDialog  
import com.intellij.driver.sdk.ui.ui  
import com.intellij.driver.sdk.waitForIndicators  
import com.intellij.driver.tests.platform.debugger.debugger  
import com.intellij.driver.tests.rubymine.utilities.extractVersionNumber  
import com.intellij.driver.tests.rubymine.utilities.ifElementPresent  
import com.intellij.ide.starter.driver.engine.BackgroundRun  
import com.intellij.ide.starter.driver.engine.runIdeWithDriver  
import com.intellij.ide.starter.extended.allure.AllureHelperExtended.step  
import com.intellij.ide.starter.extended.data.TestCases  
import com.intellij.ide.starter.extended.remdev.RemoteDevRun  
import com.intellij.ide.starter.ide.IDETestContext  
import com.intellij.ide.starter.runner.Starter  
import org.junit.jupiter.api.AfterAll  
import org.junit.jupiter.api.Assertions.assertTrue  
import org.junit.jupiter.api.BeforeAll  
import org.junit.jupiter.api.Test  
import org.junit.jupiter.api.extension.ExtendWith  
import kotlin.time.Duration.Companion.minutes  
  
@ExtendWith(RemoteDevRun::class)  
class RubyDebuggerShortcutsTest {  
  @Test  
  fun testRubyDebuggerShortcuts() {  
    bgRun.useDriverAndCloseIde {  
      ui.ideFrame {  
        waitForIndicators(1.minutes)  
  
        step("Choose the right Ruby interpreter") {  
          settingsDialog {  
            openSettingsDialog()  
            openTreeSettingsSection("Languages & Frameworks", "Ruby Interpreters", fullMatch = false)  
            val tree = checkBoxTree()  
            val sortedPaths = tree.collectExpandedPaths().flatMap { it.path }.sortedByDescending { extractVersionNumber(it) }  
            assertTrue(sortedPaths.isNotEmpty() && extractVersionNumber(sortedPaths[0]) >= 30000, "Ruby interpreter version 3.0.0 or higher not found")  
            tree.clickPath(sortedPaths[0], fullMatch = false)  
            keyboard { space() }  
            button("OK").click()  
          }  
        }  
        openFile("basic_ruby/controls_statements.rb", waitForCodeAnalysis = false)  
        waitForIndicators(1.minutes)  
        debugger.removeAllBreakpoints()  
        debugger.setBreakpointAtLineByShortcut(6)  
        debugger.setBreakpointAtLineByShortcut(15)  
        debugger.setBreakpointAtLineByShortcut(21)  
        debugger.setBreakpointAtLineByShortcut(31)  
        debugger.setBreakpointAtLineByShortcut(42)  
        step("Start debug session") {  
          debugger.startDebugFromMainToolbar()  
          ifElementPresent(x { byTitle("RubyMine Debugger") }, timeout = 1.minutes) {  
            it.button("Install").click()  
            mainToolbar.stopButton.waitFound()  
            mainToolbar.stopButton.click()  
            mainToolbar.debugButton.waitFound()  
          }  
        }        //step into step out resume step over run to cursor tolko zasad?  
        //step("Go through debug session") {        // TODO add back steps but with keyboard shortcuts only  
        //}  
  
        //step("Stop session") {        //  debugger.stopDebugFromMainToolbar()        //}//  
        //step("Check main toolbar state") {        //  mainToolbar.stopButton.waitNotFound()        //  mainToolbar.debugButton.waitFound()        //}      }  
  
    }  }  
  
  
  
  private companion object {  
    private lateinit var context: IDETestContext  
    private lateinit var bgRun: BackgroundRun  
    private lateinit var driver: Driver  
  
    @BeforeAll  
    @JvmStatic    fun startIde() {  
      context = Starter.newContext(testCase = TestCases.RM.DebuggerProject, testName = "testRubyDebuggerShortcuts")  
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