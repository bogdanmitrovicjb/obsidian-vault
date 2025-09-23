## Initial idea
### Should be covered
- New project & open project
	- Trusted project (remove the trusted project after testing), easily reproduced with VCS
- Rails console, Rails server
	- port forwarding check
	- checks for completion
	- command history
- Files and directories, repetitive tasks but very easy to automate
	- new -> ruby script known bug
	- rename, move (files + dir)
	- if test dir is marked as such
- Test tree, different types of tests (minitest, rspec, cucumber)
	- tree can be broken in remdev, also RM bugs
	- start with checking if the tree looks good with minitest
- IRB console
	- checks for completion
	- command history
- Pry console
	- checks for completion
	- command history
### Good to have
- bundler
- Ruby version detection may be hard to achieve in auto UI tests due to no way of checking whether all version managers and versions are detected, all the info we have on these is from the IDE itself and we cannot assess if all are present (is that right? are there ways to check all installed version managers on a device from the test itself? does the build server even have any other ruby managers? when do auto UI tests run?)
### Less important or hard to cover
- docker integration, may introduce flakiness due to docker errors or misconfigured stuff
	- port forwarding check
- typing tests, not sure auto UI tests are a good choice since the delays are probably hard to track because of many keyboard inputs and lots of things to track (actual appearing of letters, suggestions, auto completion, highlighting etc.)
	- ruby extension for split mode/frontend, provides type assist, potentially add a check for this plugin to see whether it's listed
- check for correct interpreter, should be covered by unit tests

### Postponed
- Structure view (mundane task, can be easily covered by auto UI tests I think)
- Debugger
## Questions for Karina

- what should the granularity be for these tests? Should they cover many features or be 1 test 1 feature
- what would be the optimal number of tests?
- one test file with multiple tests inside and @BeforeAll @AfterAll for running and closing the IDE || multiple files with shorter content and small number of tests (maybe even 1 per file)
	- later ask guys from automation team or Anastasia
## Notes
- see other team members' issues and commits
- smaller commits