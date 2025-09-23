- IrbConsoleTest - failing due to old Ruby versions
- TrustVcsProjectTest - failing due to same project name already being used - ask automation or rubymine internal? USE DIFFERENT PROJECTS FOR DIFFERENT TESTS solved by deleting the RubymineProjects directory before
- RubyDebuggerTest - failing indicators??? is maybe 1 minute not enough?
- CreateRubyProjectTest - success
- OpenRailsProjectVcsTest - success (note: this probably caused TrustVcsProjectTest failure)
- CreateGemProjectTest - failing due to "Action org.jetbrains.plugins.ruby.gem.bundler.actions.RunBundlerInitAction was rejected with error: action is disabled (early check)"
- CreateRailsApiProjectTest - failing due to old Ruby versions
- OpenRubyProjectTest - disabled until "scanning for ruby interpreters" issue is solved
- Other console tests also require newer ruby interpreters
- Rails navigation test also requires newer ruby interpreters




PRESENTATION 19.09.
short intro of the project
the result as explicitly described as possible
which tests are done
what do they cover
what is still run manually and what is covered by my tests IN A TABLE
how does it work, technical details
what framework
how do they run in split mode
common structure of tests
like a thesis presentation but shorter
TRAINING 15.09.
for Karina, the most important part is test coverage, it's all about saving time and reducing human factor
also stability and similar



!!!!!!!!!!!! https://jetbrains.team/p/iuia/repositories/ui-tests/files/master/src/test/kotlin/ruby/projectCreation/RMCreateGemWithMinitestTest.kt


!!!!!!!!!!!! 
```coffeescript
bundle config build.nio4r --with-cflags="-Wno-incompatible-pointer-types"
```
