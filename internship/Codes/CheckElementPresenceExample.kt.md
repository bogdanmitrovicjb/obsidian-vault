
```kotlin
package com.intellij.driver.tests.examples  
  
import com.intellij.driver.client.Driver  
import com.intellij.driver.sdk.ui.components.UiComponent  
import com.intellij.driver.sdk.ui.components.common.ideFrame  
import com.intellij.driver.sdk.ui.components.elements.dialog  
import com.intellij.driver.sdk.ui.components.elements.button  
import com.intellij.driver.sdk.ui.present  
import com.intellij.driver.sdk.ui.shouldBe  
import com.intellij.driver.sdk.ui.ui  
import com.intellij.driver.sdk.ui.xQuery  
import kotlin.time.Duration.Companion.seconds  
  
/**  
 * This example demonstrates how to check if a UI element is present * and execute different logic based on that. */class CheckElementPresenceExample {  
  
    /**  
     * Example 1: Using the present() method directly     *     * This approach checks if an element is present without waiting or asserting.  
     * It's useful when you want to conditionally execute code based on whether     * an element is present or not.     */    fun checkElementWithPresentMethod(driver: Driver) {  
        driver.withContext {  
            ideFrame {  
                // Try to find a dialog with title "Some Dialog"  
                val dialogMaybePresent = ui.dialog(xQuery { byTitle("Some Dialog") })  
                  
                // Check if the dialog is present without waiting or asserting  
                if (dialogMaybePresent.present()) {  
                    // Dialog is present, do something with it  
                    println("Dialog is present, handling it")  
                    dialogMaybePresent.button("OK").click()  
                } else {  
                    // Dialog is not present, do something else  
                    println("Dialog is not present, continuing with other logic")  
                }  
            }  
        }    }  
  
    /**  
     * Example 2: Using a safe approach with a helper function     *     * This approach uses a helper function to safely check if an element is present  
     * and execute code only if it is. It avoids exceptions if the element is not found.     */    fun checkElementWithSafeApproach(driver: Driver) {  
        driver.withContext {  
            ideFrame {  
                // Try to safely interact with a dialog that may or may not be present  
                ifElementPresent(  
                    element = ui.dialog(xQuery { byTitle("Some Dialog") }),  
                    timeout = 5.seconds  
                ) { dialog ->  
                    // This code only executes if the dialog is present  
                    println("Dialog is present, handling it")  
                    dialog.button("OK").click()  
                }  
                // Continue with the rest of the test logic  
                println("Continuing with other logic regardless of dialog presence")  
            }  
        }    }  
      
    /**  
     * Helper function to safely check if an element is present and execute code only if it is.     ** @param element The UI element to check  
     * @param timeout How long to wait for the element (default is 1 second)  
     * @param action The code to execute if the element is present  
     * @return true if the element was present and the action was executed, false otherwise  
     */    private fun <T : UiComponent> ifElementPresent(  
        element: T,  
        timeout: kotlin.time.Duration = 1.seconds,  
        action: (T) -> Unit  
    ): Boolean {  
        return try {  
            // Wait for a short time to see if the element becomes present  
            element.shouldBe("Checking if element is present", present, timeout)  
            // If we get here, the element is present, so execute the action  
            action(element)  
            true  
        } catch (e: Exception) {  
            // Element is not present within the timeout, just continue  
            false  
        }  
    }  
}
```