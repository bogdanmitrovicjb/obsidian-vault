

**New app [creation](https://youtrack.jetbrains.com/issue/XTEST-2032/Test-Open-Create-project-inside-Remote-Development-project-in-RubyMine)** - [no issues with #JBC-251.23774.275]

* Create a new Ruby application via the Create Project wizard
	* able to specify name - ok
	* able to specify location - ok
	* able to choose an interpreter - ok
	
* Create a new Rails application
	* able to specify name - ok
	* able to specify location - ok
	* able to choose an interpreter - ok
	* able to choose Rails version - ok
	- able to install new Rails version - ok
	* able to specify DB - ok
	* ~~able to specify JS framework (+ Bun) - I don't see this option
	* able to add extra options - ok
	
**Existing app [opening](https://youtrack.jetbrains.com/issue/XTEST-2032/Test-Open-Create-project-inside-Remote-Development-project-in-RubyMine)** - [no issues with #JBC-251.23774.275]

* Open existing Ruby / Rails app
	* new tab - ok
	* same tab - ok
	* attach - ok

**Ruby Interpreters and Version Managers** - [no issues with #JBC-251.23774.275]

* interaction with Ruby SDK and Gems settings - ok
  * interpreters and gems can be loaded and updated - Issue [minor]: RUBY-32437 - ok
* possible to use ruby via RVM, rbenv, chruby(or asdf), mise - ok
* possible to add system ruby interpreter - ok
* [possible to install ruby interpreter via notification](https://youtrack.jetbrains.com/issue/RUBY-31760/Add-an-ability-to-install-Ruby-from-the-SDK-not-found-notification)  - ok
* [possible to choose ruby interpreter via notification](https://youtrack.jetbrains.com/issue/RUBY-31827/Add-support-for-switching-the-project-SDK-on-project-opening-for-all-version-managers) - ok

**Typing experience**

The main issue is fixed:

Issue: RUBY-32527

Other typing related issues:

RUBY-33614

Checks:

* type the code (should be not delay with typing, no extra or unneeded completion, syntax highlighting, indents are corresponding with settings) - ok
* copy/paste code, reformat code - ok

**Refactorings** - [no issues with #JBC-251.23774.275]

1. copy/move/rename file - ok
2. extract:
   * variable (Ctrl + Alt + V) - ok
   * constant (Ctrl + Alt + C) - ok
   * method (Ctrl + Alt + M) - ok
   * field (Ctrl + Alt + F) - ok

**Files and directories** - [no issues with #JBC-251.23774.275]

* create ruby file/class/module - ok
* create scratch file (Ruby) - ok
* possible to rename File - ok
* copy/paste file in the project tree - ok
* copy/paste multiple files and folder with files in the project tree - ok

**Run and debug** - [no issues except minors with #JBC-251.23774.306]
* run Ruby script via popup on the tabname, from Editor, from project tree - Issue [minor]: IJPL-169880 - ok
* debug Ruby script via popup on the tabname, from Editor, from project tree - ok
* check that files other than Ruby don’t have this option (e.g. MD) - ok
* Edit Run/Debug configuration - ok
* RUBY-A-220364945 - Issue: IJPL-175187

**Run and debug for Rails app** - [no issues with #JBC-243.21565.128]
* Port forwarding should be available - ok
* Server should run without any errors, check that http://localhost:3000 (Rails server) is available in browser. - ok
* Do somethings with app (scaffold generator provides already some logic), we should display Rails logs - ok
* Via `Run/Debug Configuration` on the main toolbar run `Development` configuration. - ok
* Run configuration via `Run Configurations` popup - ok
* Rerun Rails run configuration. Previous run should stop, and server is rerun correctly. - ok
* Debug Rails app - ok




**Rails generators**

* generate scaffolding - ok
* Check popup window - Issue: RUBY-33348
* check generators loading - ok




**Language consoles**

* Run Rails, IRB, Pry console in all available modes
* check typing and completion - Issue [fixed in master, reproduces with 2025.1 RC]: RUBY-34032
* check navigation through completion popup
* check navigation through history - Issue: GTW-6744
* check output
* stop the process - there should be no prompt available




**Bundler** - [no issues with #JBC-251.23774.275]
* Bundler init for a new Ruby project
* Install gems with Bundler
* *other - TODO*




**Rake** - [no issues with #JBC-251.23774.275]
* tasks are loaded and runnable
* use gutters in Rake tasks: specify some tasks, and run them.
* check that custom tasks are listed in the list of available Rake tasks
* run Rails tests using rake task
* run db:migrate in rails project




**RBS**
1. RBS integration is available
2. add `rbs` gem to Gemfile
3. Add manually RBS files to Ruby project <https://www.jetbrains.com/help/ruby/rbs.html#create_rbs_file>
4. Generate via `Code | Generate` RBS file for existing Ruby file.
5. Completion in RBS
6. Navigation to RBS - Issue: IJPL-167940




**Tests** - [no issues with #JBC-251.23774.306]

Perform for Minitests and RSpec. Run:

* using context popup options tabname, from Editor
* use run options from project tree: run all in folder/all in file
* use gutters: on classname/on specific tests
* check the name of all automatic generated Run configurations and gutter text.
* test tree should be displayed, tests should be marked as failed or successful.
* check switching between failed/passed tests
* check navigation in test tree in run tool Windows, use Jump to source
* check rerun Failed tests
* check scroll to end (should be default option)
* Create a new test:
  1. Use all frameworks. Use templates from context menu when it's possible
  2. Use `Go to Test` action
  3. Make sure that `New | Ruby Test` is available only on the test source folder, and not available on other.

  
  

**Coverage** - [no issues with #JBC-251.23774.306]

* Run tests with coverage (required `gem "simplecov"` in Gemfile).
* Check results in tool window
* Check marks in editor
* Check filters
* Generate report
* Import report - [Issue]: Missing option from Run, same as for Profiler: GTW-2725




**Profiler** - [no issues with#JBC-243.20847.35]

* run Rails application with profiler (note: it works since Ruby version 2.5.3)
* run simple script with Profiler
* rerun Profiler
* attach to script with Profiler
* export/import Profiler results - [Issue]: Missing options are described here: GTW-2725




**Navigation** **(non-Rails specific cases)** - [no issues with #JBC-243.15521.8]

* Navigate to class, file, symbol
* cmd+B
* Back and Forward
* Recent files
* Recent locations popup




**Navigation** **(Rails specific cases)**

* navigate to `Related Symbol` (see `Related Symbols` action) in Rails project: - Issue [minor]: RUBY-32869
  * Through gutters and links in editor
    * View - Controller - View
    * Model - Schema - Model
    * controller's action link - routes - Issue: RUBY-32888
  * Through the list of related symbols
    * Controller
    * Model
    * Schema
    * Routes
    * Helper
    * Vews - Issue: RUBY-32797
    * Layouts
* Check `Endpoints` Tool Window
  * Tool Window's view
  * Navigation to routes:
    * double-click
    * Jump to Source
    * F4
  * Request generation

**Structure view**

* Available, can be opened
* Gemfile
* Ruby script
* Rails controller
* Rails view
* RBS files <https://www.jetbrains.com/help/ruby/rbs.html>
* Tests




**Inspections**

* Run All inspections in the small project: check navigation to source from Inspection results
* suppress particularly inspection for statement/method/class
* switch off inspection
* search in `Settings | Inspection`
* copy default profile and configure custom
* check that inspection settings are saved if they are imported from old version.
* check i18n integration: create properties, navigate to property, check collapsing/expanding
* Problems view
* Inspection widget

**Inspections - Rubocop**

* Rubocop integration is available in a project, results are displayed in the editor
* install Rubocop from notification (if not installed).
* run Inspection by name (Rubocop) - Issue: IJPL-169461, <span style="background-color:mistyrose;">Issue<span style="background-color:mistyrose;"> [major]: IJPL-168952
* fix Rubocop offences: particularly, all from class, all auto-correctable
* check for any particular inspection(Rubocop) that it's possible to edit settings(severity/colors)

**Inspections - Brakeman**

* see RUBY-17517 for feature details




**Docker integration**

* Add docker remote SDK
* Add docker-compose remote SDK
* check **Run and Debug** with docker\* SDK
* check **Working with Rails app** with docker\* SDK
* check Port Forwarding - Issue: IJPL-165697




**Run anything**

* ruby script
* bash command
* Rails console/server




**VCS - under platform quality gates**

* Basic features: clone/add/commit/push




**Database integration** - [no issues with #JBC-243.12818.50]

* Possible to add database from sources




**Help**

* Help\| Help is run
* Help button from some dialogs.
* navigation in Help topics (links are not broken)
* search in Help works and clickable
* quick documentation(in popup and tab)




**IDE Feature Training Plugin**

* should be available preinstalled
* all lessons should be available and work as expected




**IDE features from platform - under platform quality gates**

* change themes
* add to favorites
* appearance options
* recent files/locations/changes/changed files
* Bookmarks




**Deployment**

1. AWS
2. DevContainers - Issue: IJPL-66524
   * checks for ruby interpreters -  Issue: RUBY-33280

**Settings sync**

* add special checks for settings in remdev




**Not covered by this checklist - performance tests, autotests, cwm**

rm_required in CWM tracker: IJPL-166681

for performance tests: IJPL-170123 - from the [comment](https://youtrack.jetbrains.com/issue/IJPL-170123/Add-a-way-to-set-Advanced-settings-on-the-backend-from-the-frontend#focus=Comments-27-10368596.0-0), not a high priority for us anymore

IJPL-167922

IJPL-168923

IJPL-168236