

- debugger test
- rails navigation test
- console tests
- tests test


### All tasks sorted by priority
- high
	- CHECK THIS - To make the standard test run in split mode from TC, you need to add `param("env.REMOTE_DEV_RUN", "true")` to the build configuration. You may use [platformTests.common.getMonolithAndSplitRunSpecs](https://jetbrains.team/p/ij/repositories/ultimate-teamcity-config/files/d0ed1e329ff821c9b1f90e7d4f2818a60b0c27f4/.teamcity/src/platformTests/common/RunSpec.kt?tab=source&line=47&lines-count=1).
	- CHECK THIS - See why the tests are no longer running on TeamCity
	- find out what did I do wrong with git and what's the correct way to do it DONE
		- basically the flow we did before is the right one so no issues there
	- add the "scanning for ruby interpreters" popup to all other tests which need it DONE
		- although the tests are now in monolith instead of split mode until the issue is fixed
	- ask about allure annotations in the slack channel and finish adding allure annotations to the rest of the tests DONE
		- added them, will have to revisit since there was some talk about changing how it's written, I think subsystem annotations should match subsystems in YouTrack
		- ? ask where allure annotations are used
	- ask about ruby on linux agents issue status NOT DONE
		- Dmitrii Panov who looked at the issue is not available, he suggested #ij-builds but it's stated that it's only for urgent stuff, and the link for non urgent stuff just opens a new YouTrack issue, which I already did, so I suppose I should just wait
	- finish the trust project test DONE
		- it already has checks when you open a project and it's not trusted
		- needs added checks for when you trust the project to see if everything is marked as trusted (file explorer on the left), the project gets indexed and you can do everything you can with a trusted project
	- remove shortcuts debugger test DONE
		- as stated in some guide, shortcuts can be changed and shouldn't be used in UI test because they introduce flakiness, if needed, a separate shortcuts test can be added, which I have started and done some work on, but it's low priority for now
	- add endpoints to rails navigation test REDUCE FLAKINESS
	- finish the "tests" test
		- running tests 
	- convert "project" tests to monolith mode until the issue with Gem stuff is taken care of DONE
		- all the tests in the "project" directory are now in monolith
	- read [remote development quality gates](https://youtrack.jetbrains.com/articles/RUBY-A-220365219) again (when I finish, the tests will be included as an extra step!) DONE
		- basically a file stating what a build must go through to be valid
- medium
	- check which subsystems does RubyMine have
	- check if I can update the "Empty project" RubyMine testcase
	- see when to use driver and when to use IdeaFrameUI in steps files DONE
	- read the driver guides again (now I have a better understanding on how driver stuff works, detailed guide on best practices should be more useful now) DONE
		- everything makes much more sense now
	- read [aggregator](https://youtrack.jetbrains.com/articles/IJPL-A-156) stuff again
	- finish the gutter test
		- split into different tests, not one test running all checks
		- add the rest of the gutter icons
	- create a shortcuts test (only shortcuts which interact with ruby stuff and debugger)
	- go through the checklists again to see what else is missing
	- go through the Lux and BeControl 
	- extract common steps like mentioned in the guide
	- finish tests maintenance guide
- low
	- find out why new project always starts the gem thing AND find out what happens with ruby interpreters in split mode
		- investigated a bit, partially alone, partially with Egor, it doesn't actually start creating a Gem project (!!!!!), only the message is misleading, it's an error message which appears when the interpreters are not loaded before any project wizard starts and, for reasons I don't understand, the error message says the Gem project can’t run without an interpreter, even though the others can't either
		- there seems to be a race condition since the ruby interpreter scanning popup is a modal dialog (modal window?) and there are several warnings about using them, they are also marked as deprecated
		- I had different issues with modal windows when I need to run invokeAction which opens them, if I don't specify that they need to run async, the test will freeze and ultimately fail
	- find how to check if the color of text in consoles is correct (no clue how to do this)
	- finish the shortcuts test

- open rails console through "Run anything"
- tech details, realization, how it works, HOW IT LOOKS FROM QA PERSPECTIVE

https://sandbox.mc.edu/~bennet/ruby/code/


Endgame

- all tests in allure
- maintenance guide
- rails console test
- refactor files test
- refactor ruby test
- testtreetest
- allure id commit and push
- teamcity config commit and push
- rvm.sh commit and push
- feedback
- move stuff to other laptop
- check when can I return the laptop
- log in to other laptop
- lokija na friday pets

sta mora danas
- maintenance guide
- rails console test DONE
- refactor files test
- refactor ruby test
- testtreetest
- allure id commit and push CEKAM REVIEW
- teamcity config commit and push CEKAM REVIEW
- rvm.sh commit and push DONE
- move stuff to other laptop DONE
- check when can I return the laptop DONE
- log in to other laptop DONE


sta moze sutra
- all tests in allure
- feedback
- lokija na friday pets

