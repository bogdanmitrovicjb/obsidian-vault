
```kotlin
package com.intellij.ruby.performanceTests.splitMode  
  
import com.intellij.ide.starter.driver.engine.remoteDev.transportDelay  
import com.intellij.ide.starter.driver.engine.runIdeWithDriver  
import com.intellij.ide.starter.driver.execute  
import com.intellij.ide.starter.extended.remdev.RemoteDevRun  
import com.intellij.ide.starter.extended.setupRubySdk  
import com.intellij.ide.starter.runner.Starter  
import com.intellij.ruby.performanceTests.RubyMineTestCases  
import com.intellij.tools.ide.metrics.collector.starter.publishing.MetricsPublisher  
import com.intellij.tools.ide.metrics.collector.starter.publishing.publishOnlySpans  
import com.intellij.tools.ide.metrics.collector.telemetry.SpanFilter  
import com.intellij.tools.ide.performanceTesting.commands.*  
import org.junit.jupiter.api.Test  
import org.junit.jupiter.api.extension.ExtendWith  
  
@ExtendWith(RemoteDevRun::class)  
class BasicSplitModeTest {  
  
  @Test  
  fun testOpenFile() {  
    val startResult = Starter.newContext("splitMode/local/ruby-26170/openFile", RubyMineTestCases.Ruby26170, true).runIdeWithDriver().useDriverAndCloseIde {  
      execute(CommandChain().startProfile("openFile").openFile("swagger_helper.rb").goto(116, 11).stopProfile())  
    }  
  
    MetricsPublisher.newInstance.publishOnlySpans(startResult, SpanFilter.nameEquals("openFile"))  
  }  
  
  @Test  
  fun testSearchEverywhere() {  
    val startResult = Starter.newContext("splitMode/local/ruby-26170/searchEverywhere", RubyMineTestCases.Ruby26170, true).runIdeWithDriver().useDriverAndCloseIde {  
      execute(CommandChain().transportDelay(100).openFile("swagger_helper.rb").startProfile("searcheverywhere").searchEverywhere(textToType = "swagger").stopProfile())  
    }  
    MetricsPublisher.newInstance.publishOnlySpans(startResult, SpanFilter.nameEquals("searchEverywhere"))  
  }  
  
  @Test  
  fun testCompletion() {  
    val startResult = Starter.newContext("splitMode/local/ruby-26170/completion", RubyMineTestCases.Ruby26170, true).setupRubySdk().runIdeWithDriver().useDriverAndCloseIde {  
      execute(CommandChain().openFile("swagger_helper.rb").startProfile("completion").goto(116, 11).doComplete(5).stopProfile())  
    }  
    MetricsPublisher.newInstance.publishOnlySpans(startResult, SpanFilter.nameEquals("completion#firstItemShown"))  
  }  
}
```