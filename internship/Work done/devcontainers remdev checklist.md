**New app creation - ok
- Create a new Ruby application via the Create Project wizard - ok
    - able to specify name - ok
    - able to specify location - ok
    - able to choose an interpreter - ok
- Create a new Rails application - ok
    - able to specify name - ok
    - able to specify location - ok
    - able to choose an interpreter - ok
    - able to choose Rails version - ok
    - able to install new Rails version - ok (it's installed upon creation)
    - able to specify DB - ok
    - able to specify JS framework (+ Bun) - I can't see this option
    - able to add extra options - ok

**Existing app opening - ok
- Open existing Ruby / Rails app - ok but recents are not shown unless you click Recent Projects -> Manage Projects
    - new tab - ok
    - same tab - ok
    - attach - ok

**Ruby Interpreters and Version Managers** - ok
- interaction with Ruby SDK and Gems settings - ok
    - interpreters and gems can be loaded and updated - ok
- possible to use ruby via RVM, rbenv, chruby(or asdf), mise  - ok checked with rbenv
- possible to add system ruby interpreter - TODO install another ruby without ruby manager to check
- [possible to install ruby interpreter via notification](https://youtrack.jetbrains.com/issue/RUBY-31760/Add-an-ability-to-install-Ruby-from-the-SDK-not-found-notification) - ok
- [possible to choose ruby interpreter via notification](https://youtrack.jetbrains.com/issue/RUBY-31827/Add-support-for-switching-the-project-SDK-on-project-opening-for-all-version-managers) - ok

**Typing experience**
The main issue is fixed: Issue: [RUBY-32527](https://youtrack.jetbrains.com/issue/RUBY-32527/Improve-editing-experience-in-Remote-Development-for-Ruby)
Other typing related issues: [RUBY-33614](https://youtrack.jetbrains.com/issue/RUBY-33614/Migrate-Ruby-secondary-languages-logic-to-Split-Mode)
Checks:
- type the code (should be not delay with typing, no extra or unneeded completion, syntax highlighting, indents are corresponding with settings)  - ok
- copy/paste code, reformat code - ok

**Refactorings** - [no issues with #JBC-251.23774.275]
1. copy/move/rename file - ok
2. extract: - ok
    - variable (Ctrl + Alt + V) - ok
    - constant (Ctrl + Alt + C) - ok
    - method (Ctrl + Alt + M) - ok
    - field (Ctrl + Alt + F) - ok
**Files and directories** - [no issues with #JBC-251.23774.275] - ok
- create ruby file/class/module - ok
- create scratch file (Ruby) - ok
- possible to rename File - ok
- copy/paste file in the project tree - ok
- copy/paste multiple files and folder with files in the project tree - TODO check

**Run and debug** - [no issues except minors with #JBC-251.23774.306] - ok
- run Ruby script via popup on the tabname, from Editor, from project tree - Issue [minor]: [IJPL-169880](https://youtrack.jetbrains.com/issue/IJPL-169880/Current-run-configuration-is-infinitely-loading) - ok (note: Ruby is great for scripting it seems)
- debug Ruby script via popup on the tabname, from Editor, from project tree - ok
- check that files other than Ruby don’t have this option (e.g. MD) - ok
- Edit Run/Debug configuration - ok
- [Check-list for RubyMine debugger](https://youtrack.jetbrains.com/articles/RUBY-A-220364945/Check-list-for-RubyMine-debugger "RUBY-A-220364945: Check-list for RubyMine debugger") - Issue: [IJPL-175187](https://youtrack.jetbrains.com/issue/IJPL-175187/Smart-step-into-enters-new-line-instead-of-stepping-into-in-RubyMine) - TODO finish this

**Run and debug for Rails app** - [no issues with #JBC-243.21565.128] - ok
- Port forwarding should be available - ~~Issue [major]~~~~:~~ [GTW-9708](https://youtrack.jetbrains.com/issue/GTW-9708/The-run-tool-window-does-not-contain-ports-for-port-forwarding) - ok, has ports
- Server should run without any errors, check that [http://localhost:3000](http://localhost:3000/) (Rails server) is available in browser. -  ok
- Do somethings with app (scaffold generator provides already some logic), we should display Rails logs - app works, ok
- Via `Run/Debug Configuration` on the main toolbar run `Development` configuration. - haven't found this
- Run configuration via `Run Configurations` popup - ok
- Rerun Rails run configuration. Previous run should stop, and server is rerun correctly. - ok
- Debug Rails app - ok

**Rails generators** - ok
- generate scaffolding - ok
- Check popup window - Issue: [RUBY-33348](https://youtrack.jetbrains.com/issue/RUBY-33348/No-type-completion-in-gails-generator-popup-with-RubyMine-in-Remote-Development)- ok
- check generators loading - ok

**Language consoles** - TODO finish

- Run Rails, IRB, Pry console in all available modes - TODO finished reading about IRB and Pry, try it later, tried IRB works ok
- check typing and completion - Issue [fixed in master, reproduces with 2025.1 RC]: [RUBY-34032](https://youtrack.jetbrains.com/issue/RUBY-34032/The-third-input-symbol-in-the-IRB-console-is-duplicated-for-a-moment)- ok, try with 2025.1 RC
- check navigation through completion popup
- check navigation through history - Issue: [GTW-6744](https://youtrack.jetbrains.com/issue/GTW-6744/Navigating-history-in-IRB-Pry-Console-does-not-work)
- check output
- stop the process - there should be no prompt available

**Bundler** - [no issues with #JBC-251.23774.275] - ok
- Bundler init for a new Ruby project - ok
- Install gems with Bundler - ok
- _other - TODO_ - not sure what other are, tested using a gem which is not bundled, then going through popups to install it and it works perfectly

**Rake** - [no issues with #JBC-251.23774.275] - ok
- tasks are loaded and runnable - ok
- use gutters in Rake tasks: specify some tasks, and run them. - TODO what are gutters
- check that custom tasks are listed in the list of available Rake tasks
- run Rails tests using rake task - ok
- run db:migrate in rails project - ok

**RBS**
1. RBS integration is available
2. add `rbs` gem to Gemfile
3. Add manually RBS files to Ruby project [https://www.jetbrains.com/help/ruby/rbs.html#create_rbs_file](https://www.jetbrains.com/help/ruby/rbs.html#create_rbs_file)
4. Generate via `Code | Generate` RBS file for existing Ruby file.
5. Completion in RBS
6. Navigation to RBS - Issue: [IJPL-167940](https://youtrack.jetbrains.com/issue/IJPL-167940/Inlay-hints-for-types-are-not-displayed-in-the-editor)

**Tests** - [no issues with #JBC-251.23774.306]

Perform for Minitests and RSpec. Run:

- using context popup options tabname, from Editor
    
- use run options from project tree: run all in folder/all in file
    
- use gutters: on classname/on specific tests
    
- check the name of all automatic generated Run configurations and gutter text.
    
- test tree should be displayed, tests should be marked as failed or successful.
    
- check switching between failed/passed tests
    
- check navigation in test tree in run tool Windows, use Jump to source
    
- check rerun Failed tests
    
- check scroll to end (should be default option)
    
- Create a new test:
    
    1. Use all frameworks. Use templates from context menu when it's possible
    2. Use `Go to Test` action
    3. Make sure that `New | Ruby Test` is available only on the test source folder, and not available on other.
    

**Coverage** - [no issues with #JBC-251.23774.306]

- Run tests with coverage (required `gem "simplecov"` in Gemfile).
- Check results in tool window
- Check marks in editor
- Check filters
- Generate report
- Import report - [Issue]: Missing option from Run, same as for Profiler: [GTW-2725](https://youtrack.jetbrains.com/issue/GTW-2725/Missing-profiling-options-on-thin-client)

**Profiler** - [no issues with#JBC-243.20847.35]
- run Rails application with profiler (note: it works since Ruby version 2.5.3) 
- run simple script with Profiler
- rerun Profiler
- attach to script with Profiler
- export/import Profiler results - [Issue]: Missing options are described here: [GTW-2725](https://youtrack.jetbrains.com/issue/GTW-2725/Missing-profiling-options-on-thin-client)

**Navigation** **(non-Rails specific cases)** - [no issues with #JBC-243.15521.8]
- Navigate to class, file, symbol - ok
- cmd+B - ok
- Back and Forward - ok
- Recent files - ok
- Recent locations popup - ok

**Navigation** **(Rails specific cases)**
- navigate to `Related Symbol` (see `Related Symbols` action) in Rails project: - Issue [minor]: [RUBY-32869](https://youtrack.jetbrains.com/issue/RUBY-32869/Loading-is-temporarily-displayed-when-Go-To-Related-Symbols-is-called-with-RubyMine-in-Remote-Development)
    - Through gutters and links in editor - ok
        - View - Controller - View - ok
        - Model - Schema - Model - ok
        - controller's action link - routes - Issue: [RUBY-32888](https://youtrack.jetbrains.com/issue/RUBY-32888/No-links-to-routes-for-controllers-actions-with-RubyMine-in-Remote-Development) - ok
    - Through the list of related symbols
        - Controller - ok
        - Model - ok
        - Schema - ok
        - Routes - ok
        - Helper - TODO test
        - Views - Issue: [RUBY-32797](https://youtrack.jetbrains.com/issue/RUBY-32797/Views-and-Layouts-are-not-selected-in-project-tree-for-Go-to-Related-symbols-in-Remote-Development) - ok
        - Layouts - ok
- Check `Endpoints` Tool Window
    - Tool Window's view - ok
    - Navigation to routes:
        - double-click - ok
        - Jump to Source - ok
        - F4 - ok
    - Request generation - ok

**Structure view**
- Available, can be opened - ok
- Gemfile - structure is empty
- Ruby script - ok
- Rails controller - ok
- Rails view - ok
- RBS files [https://www.jetbrains.com/help/ruby/rbs.html](https://www.jetbrains.com/help/ruby/rbs.html) - TODO
- Tests - ok

**Inspections**
- Run All inspections in the small project: check navigation to source from Inspection results - ok
- suppress particularly inspection for statement/method/class - ok
- switch off inspection - ok
- search in `Settings | Inspection` - ok
- copy default profile and configure custom - ok (didn't know this was possible)
- check that inspection settings are saved if they are imported from old version. - export import for same version work, TODO check for older version
- check i18n integration: create properties, navigate to property, check collapsing/expanding
- Problems view - ok
- Inspection widget - ok

**Inspections - Rubocop** - ok
- Rubocop integration is available in a project, results are displayed in the editor - ok
- install Rubocop from notification (if not installed). - ok
- run Inspection by name (Rubocop) - Issue: [IJPL-169461](https://youtrack.jetbrains.com/issue/IJPL-169461/Ruby-icon-is-doubled-in-Run-inspection-by-Name-search-results-in-Remote-Development), Issue [major]: [IJPL-168952](https://youtrack.jetbrains.com/issue/IJPL-168952/Improperly-converting-panel-with-unknown-layout-exception-when-running-Rubocop-inspections-in-Remote-Development) - ok, icon not doubled
- fix Rubocop offences: particularly, all from class, all auto-correctable - ok
- check for any particular inspection(Rubocop) that it's possible to edit settings(severity/colors) - ok

**Inspections - Brakeman** - ok
- see [RUBY-17517](https://youtrack.jetbrains.com/issue/RUBY-17517/Support-Brakeman-code-inspections) for feature details - ok when running from Code -> Analyze code -> Run Inspection By Name -> Brakeman

**Docker integration**
- Add docker remote SDK
- Add docker-compose remote SDK
- check **Run and Debug** with docker* SDK
- check **Working with Rails app** with docker* SDK
- check Port Forwarding - Issue: [IJPL-165697](https://youtrack.jetbrains.com/issue/IJPL-165697/No-port-forwarding-for-containerised-projects)

**Run anything**
- ruby script - ok
- bash command - ok
- Rails console/server - ok

**VCS - under platform quality gates**
- Basic features: clone/add/commit/push - ok

**Database integration** - [no issues with #JBC-243.12818.50]
- Possible to add database from sources - ok added postgres

**Help**
- Help| Help is run - ok
- Help button from some dialogs. - ok
- navigation in Help topics (links are not broken) - ok for all links
- search in Help works and clickable - yes, but is slow and when you move the cursor away from it and place it back, it freezes sometimes
- quick documentation(in popup and tab) - ok

**IDE Feature Training Plugin**
- should be available preinstalled - not preinstalled, once I installed it, restart IDE took long
- all lessons should be available and work as expected - they do once I installed it manually

**IDE features from platform - under platform quality gates**
- change themes - ok
- add to favorites - ok
- appearance options - ok
- recent files/locations/changes/changed files - recent projects don't appear unless you press Recent Projects -> Manage Projects and in nightly they show when only hovering Recent Projects
- Bookmarks

**Deployment**

1. AWS - TODO ask Karina about this
2. DevContainers - Issue: [IJPL-66524](https://youtrack.jetbrains.com/issue/IJPL-66524/DevContainers-when-devcontainer-is-built-with-the-sources-mounting-the-project-settings-save-fails) - unable to run dev containers from a dev container
    - checks for ruby interpreters -  Issue: [RUBY-33280](https://youtrack.jetbrains.com/issue/RUBY-33280/DevContainers-sometimes-SDK-is-not-selected-when-IDE-is-launched)

**Settings sync**
- add special checks for settings in remdev