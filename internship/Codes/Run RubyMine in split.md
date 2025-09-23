
```kotlin
package com.jetbrains.rdct.remoteDriverTests.rubymine  
  
import com.intellij.ide.starter.driver.engine.runIdeWithDriver  
import com.intellij.ide.starter.extended.remdev.RemoteDevRun  
import com.intellij.ide.starter.ide.IdeProductProvider  
import com.intellij.ide.starter.junit5.newContext  
import com.intellij.ide.starter.runner.Starter  
import com.jetbrains.rdct.remoteDriverTests.runConfigurations.RunConfigurationsProjects  
import com.jetbrains.rdct.remoteDriverTests.runConfigurations.RunConfigurationsTest  
import org.junit.jupiter.api.Test  
import org.junit.jupiter.api.extension.ExtendWith  
  
@ExtendWith(RemoteDevRun::class)  
class RunConfigurationsRubyMineTest : RunConfigurationsTest() {  
  
  override fun filePath() = "main.rb"  
  
  @Test  
  fun `rubymine run from configurations toolbar`() {  
    val context = Starter.newContext(IdeProductProvider.RM) {  
      project = RunConfigurationsProjects.RubyMineSimpleProject  
    }  
    context.runIdeWithDriver().useDriverAndCloseIde {  
      runFileWithRunConfigurations(this)  
    }  
  }  
  
}
```