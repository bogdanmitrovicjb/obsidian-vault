ad descriptions for comments for tests



testtree
- check icon for test folder is testRoot.svg
- gutter has rerun.svg or run.svg
- from model there is action GotoTest to go to test
- from test there is action GotoTest to go back to model
- showPassed.svg


TESTS MINITEST
gutter icons
from class
from test
from tab
from editor BAR AT TOP
from file right click
when you have results
SHOW HIDE GREEN TESTS
right click on test correct info is there
make some tests fail on purpose
running failed tests


The current configuration is here (https://jetbrains.team/p/ij/repositories/ultimate-teamcity-config/files/master/.teamcity/src/rubymine/tests/uiTests/Ruby_Tests_UiTests.kt), the docker images I'm trying to use are here (https://jetbrains.team/p/rubymine/repositories/rubymine-test-images/files/master/docker) and ruby integration tests are configured here (https://jetbrains.team/p/ij/repositories/ultimate-teamcity-config/files/master/.teamcity/src/rubymine/tests/integrationTests). I was going to try something like this 

code

The changes are:
- linuxAWS instead of linuxAWSUI
- dockerImage
- dockerArgs
- additionalParameters (including REMOTE_DEV_RUN because my tests should be running in split mode)
- buildType block similar to this file (https://jetbrains.team/p/ij/repositories/ultimate-teamcity-config/files/master/.teamcity/src/rubymine/tests/integrationTests/buildTypes/RubyUltimateTestWithDocker.kt)