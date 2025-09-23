
### CreateGemProjectTest.kt

```kotlin
Subject: [PATCH] Uncommitted changes before Checkout at 17. 7. 2025., 12:40 [Changes]
---
Index: tests/remote-driver-tests/test/com/intellij/driver/tests/rubymine/project/CreateGemProjectTest.kt
===================================================================
diff --git a/tests/remote-driver-tests/test/com/intellij/driver/tests/rubymine/project/CreateGemProjectTest.kt b/tests/remote-driver-tests/test/com/intellij/driver/tests/rubymine/project/CreateGemProjectTest.kt
--- a/tests/remote-driver-tests/test/com/intellij/driver/tests/rubymine/project/CreateGemProjectTest.kt	(revision 156492d2a9adf4f9686f4b6b1306955f9958ec8b)
+++ b/tests/remote-driver-tests/test/com/intellij/driver/tests/rubymine/project/CreateGemProjectTest.kt	(date 1752586219540)
@@ -2,6 +2,7 @@
 
 import com.intellij.driver.client.Driver
 import com.intellij.driver.sdk.invokeAction
+import com.intellij.driver.sdk.ui.components.UiComponent.Companion.waitFound
 import com.intellij.driver.sdk.ui.components.common.editorTabs
 import com.intellij.driver.sdk.ui.components.common.ideFrame
 import com.intellij.driver.sdk.ui.components.common.welcomeScreen
@@ -16,21 +17,25 @@
 import com.intellij.driver.sdk.ui.ui
 import com.intellij.driver.sdk.ui.xQuery
 import com.intellij.driver.sdk.waitForIndicators
+import com.intellij.driver.tests.rubymine.utilities.extractVersionNumber
 import com.intellij.driver.tests.rubymine.utilities.ifElementPresent
 import com.intellij.ide.starter.driver.engine.BackgroundRun
 import com.intellij.ide.starter.driver.engine.runIdeWithDriver
+import com.intellij.ide.starter.extended.allure.AllureHelperExtended.step
+import com.intellij.ide.starter.extended.remdev.RemoteDevRun
 import com.intellij.ide.starter.ide.IDETestContext
 import com.intellij.ide.starter.ide.IdeProductProvider
 import com.intellij.ide.starter.junit5.newContext
-import com.intellij.ide.starter.report.AllureHelper.step
 import com.intellij.ide.starter.runner.Starter
 import org.junit.jupiter.api.AfterAll
 import org.junit.jupiter.api.Assertions.assertTrue
 import org.junit.jupiter.api.BeforeAll
 import org.junit.jupiter.api.Test
+import org.junit.jupiter.api.extension.ExtendWith
 import kotlin.time.Duration.Companion.minutes
 import kotlin.time.Duration.Companion.seconds
 
+@ExtendWith(RemoteDevRun::class)
 class CreateGemProjectTest {
   @Test
   fun testCreateGemProject() {
@@ -58,9 +63,12 @@
             if (!rubyInterpreterSpecified) {
               val rubyComboBox = comboBox(xQuery { byClass("RubySdkComboBox") })
               rubyComboBox.click()
-              jBlist(xQuery { contains(byVisibleText("ruby")) }).clickItem("ruby", fullMatch = false)
+              jBlist(xQuery { contains(byVisibleText("ruby")) }).waitFound(1.minutes)
+              val sortedValues = rubyComboBox.listValues().sortedByDescending { extractVersionNumber(it) }
+              assertTrue(sortedValues.isNotEmpty(),"No ruby interpreters are showing")
+              assertTrue(extractVersionNumber(sortedValues[0]) >= 30000, "Ruby interpreter version 3.0.0 or higher not found")
+              jBlist(xQuery { contains(byVisibleText("ruby")) }).clickItem(sortedValues[0], fullMatch = false)
               should(message = "Ruby interpreter is not selected") { rubyComboBox.getSelectedItem().contains("ruby") }
-              rubyInterpreterSpecified = true // TODO do I need this here?
             }
             button("Create").click()
           }
@@ -70,7 +78,6 @@
         {
           val gemDialog = ui.dialog()
           gemDialog.shouldBe("Downloading Bundler gems list should show", present, 10.seconds)
-          // gemDialog.shouldBe("Downloading Bundler gems list should disappear", notPresent, 30.seconds) TODO this is flaky and probably just wrong
         }
 
         step("Install bundler Gem if needed") {
@@ -106,7 +113,7 @@
     @BeforeAll
     @JvmStatic
     fun startIde() {
-      context = Starter.newContext(testName = "testCreateRailsApiProject", ideInfo = IdeProductProvider.RM)
+      context = Starter.newContext(testName = "testCreateGemProject", ideInfo = IdeProductProvider.RM)
       bgRun = context.runIdeWithDriver(runTimeout = 5.minutes)
       driver = bgRun.driver
     }

```

### CreateRailsApiProjectTest.kt

```kotlin
Subject: [PATCH] Uncommitted changes before Checkout at 17. 7. 2025., 12:40 [Changes]
---
Index: tests/remote-driver-tests/test/com/intellij/driver/tests/rubymine/project/CreateRailsApiProjectTest.kt
===================================================================
diff --git a/tests/remote-driver-tests/test/com/intellij/driver/tests/rubymine/project/CreateRailsApiProjectTest.kt b/tests/remote-driver-tests/test/com/intellij/driver/tests/rubymine/project/CreateRailsApiProjectTest.kt
--- a/tests/remote-driver-tests/test/com/intellij/driver/tests/rubymine/project/CreateRailsApiProjectTest.kt	(revision 156492d2a9adf4f9686f4b6b1306955f9958ec8b)
+++ b/tests/remote-driver-tests/test/com/intellij/driver/tests/rubymine/project/CreateRailsApiProjectTest.kt	(date 1752227738798)
@@ -116,6 +116,8 @@
               versionPopup.waitFound(30.seconds)
               versionPopup.button("OK").click()
               railsVersionSpecified = true
+              // wait for installing to close, then check if rails actually works (check the checklists to see how to do that exactly)
+
             }
 
             button("Create").click()

```

### CreateRubyProjectTest.kt

```kotlin
Subject: [PATCH] Uncommitted changes before Checkout at 17. 7. 2025., 12:40 [Changes]
---
Index: tests/remote-driver-tests/test/com/intellij/driver/tests/rubymine/project/CreateRubyProjectTest.kt
===================================================================
diff --git a/tests/remote-driver-tests/test/com/intellij/driver/tests/rubymine/project/CreateRubyProjectTest.kt b/tests/remote-driver-tests/test/com/intellij/driver/tests/rubymine/project/CreateRubyProjectTest.kt
--- a/tests/remote-driver-tests/test/com/intellij/driver/tests/rubymine/project/CreateRubyProjectTest.kt	(revision 156492d2a9adf4f9686f4b6b1306955f9958ec8b)
+++ b/tests/remote-driver-tests/test/com/intellij/driver/tests/rubymine/project/CreateRubyProjectTest.kt	(date 1752230398731)
@@ -48,7 +48,6 @@
               }
             }
             jBlist(xQuery { byClass("JBList") }).clickItem("Empty Project", fullMatch = false)
-            //textField(xQuery { byClass("JBTextField") }).text = "RubyTestProject" TODO should remain untitled, remove everywhere
             button("Create").click()
           }
         }

```

### intellij.driver.tests.iml
```xml
Subject: [PATCH] Uncommitted changes before Checkout at 17. 7. 2025., 12:40 [Changes]
---
Index: tests/remote-driver-tests/intellij.driver.tests.iml
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/tests/remote-driver-tests/intellij.driver.tests.iml b/tests/remote-driver-tests/intellij.driver.tests.iml
--- a/tests/remote-driver-tests/intellij.driver.tests.iml	(revision 156492d2a9adf4f9686f4b6b1306955f9958ec8b)
+++ b/tests/remote-driver-tests/intellij.driver.tests.iml	(date 1749197554600)
@@ -44,5 +44,6 @@
     <orderEntry type="library" scope="TEST" name="kotlin-test-assertions-core-jvm" level="project" />
     <orderEntry type="module" module-name="intellij.workspaceModel.performanceTesting" scope="TEST" />
     <orderEntry type="module" module-name="intellij.rustrover.integration.testFramework" scope="TEST" />
+    <orderEntry type="module" module-name="intellij.rdct.testFramework" scope="TEST" />
   </component>
 </module>
\ No newline at end of file

```

### OpenRubyProjectTest.kt

```kotlin
Subject: [PATCH] Uncommitted changes before Checkout at 17. 7. 2025., 12:40 [Changes]
---
Index: tests/remote-driver-tests/test/com/intellij/driver/tests/rubymine/project/OpenRubyProjectTest.kt
===================================================================
diff --git a/tests/remote-driver-tests/test/com/intellij/driver/tests/rubymine/project/OpenRubyProjectTest.kt b/tests/remote-driver-tests/test/com/intellij/driver/tests/rubymine/project/OpenRubyProjectTest.kt
--- a/tests/remote-driver-tests/test/com/intellij/driver/tests/rubymine/project/OpenRubyProjectTest.kt	(revision 156492d2a9adf4f9686f4b6b1306955f9958ec8b)
+++ b/tests/remote-driver-tests/test/com/intellij/driver/tests/rubymine/project/OpenRubyProjectTest.kt	(date 1751891515735)
@@ -34,7 +34,6 @@
 @ExtendWith(RemoteDevRun::class)
 class OpenRubyProjectTest {
 
-  @Disabled
   @Test
   fun testOpenRubyProject() {
     driver.withContext {

```

### RubyDebuggerUiTest.kt

```kotlin
Subject: [PATCH] Uncommitted changes before Checkout at 17. 7. 2025., 12:40 [Changes]
---
Index: tests/remote-driver-tests/test/com/intellij/driver/tests/rubymine/debugger/RubyDebuggerUiTest.kt
===================================================================
diff --git a/tests/remote-driver-tests/test/com/intellij/driver/tests/rubymine/debugger/RubyDebuggerUiTest.kt b/tests/remote-driver-tests/test/com/intellij/driver/tests/rubymine/debugger/RubyDebuggerUiTest.kt
--- a/tests/remote-driver-tests/test/com/intellij/driver/tests/rubymine/debugger/RubyDebuggerUiTest.kt	(revision 156492d2a9adf4f9686f4b6b1306955f9958ec8b)
+++ b/tests/remote-driver-tests/test/com/intellij/driver/tests/rubymine/debugger/RubyDebuggerUiTest.kt	(date 1752224985587)
@@ -5,6 +5,7 @@
 import com.intellij.driver.sdk.ui.components.UiComponent.Companion.waitFound
 import com.intellij.driver.sdk.ui.components.common.ideFrame
 import com.intellij.driver.sdk.ui.components.common.mainToolbar
+import com.intellij.driver.sdk.ui.components.common.resumeButton
 import com.intellij.driver.sdk.ui.components.elements.button
 import com.intellij.driver.sdk.ui.components.elements.checkBoxTree
 import com.intellij.driver.sdk.ui.components.settings.settingsDialog
@@ -28,6 +29,7 @@
 import org.junit.jupiter.api.Test
 import org.junit.jupiter.api.extension.ExtendWith
 import kotlin.time.Duration.Companion.minutes
+import kotlin.time.Duration.Companion.seconds
 
 @ExtendWith(RemoteDevRun::class)
 class RubyDebuggerUiTest {
@@ -60,9 +62,18 @@
         debugger.setBreakpointAtLine(42)
         step("Start debug session") {
           debugger.startDebugFromMainToolbar()
-          ifElementPresent(x { byTitle("RubyMine Debugger") }, timeout = 1.minutes) {
-            it.button("Install").click()
+          ifElementPresent(x { byTitle("RubyMine Debugger") }, timeout = 2.seconds) {
+            step("Install debugger")
+            {
+              it.button("Install").click()
+            }
             mainToolbar.stopButton.waitFound()
+            // check for stopping at a correct line I guess
+            mainToolbar.resumeButton.click()
+            // check again for stopping at a correct line
+            debugger.clickStepButton("Step Over") // Step Into, Step Over, Step Out, Resume Program
+
+
             mainToolbar.stopButton.click()
             mainToolbar.debugButton.waitFound()
           }

```

  

![[Pasted image 20250718095331.png]]
[REMOTE_DRIVER_RUBYMINE_TEST_GROUP]  
com.intellij.driver.tests.rubymine.*