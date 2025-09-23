```kotlin
fun <T : UiComponent> ifElementPresent(  
  element: T,  
  timeout: kotlin.time.Duration = 1.seconds,  
  orElse: (() -> Unit)? = null,  
  action: (T) -> Unit,  
): Boolean {  
  return try {  
    element.shouldBe("Checking if element is present", present, timeout)  
    action(element)  
    true  
  }  
  catch (_: Throwable) {  
    orElse?.invoke()  
    false  
  }  
}
```
this one has orElse