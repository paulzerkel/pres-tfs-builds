Team Foundation Center Builds
=================================

This is a lunch presentation on using Team Foundation Server as a build server. Notes for the presentation are below.

Team Foundation Server
----------------------
* Microsoft’s Application Lifecycle Management solution
* Includes task tracking, source control, bug tracking, build server, deployments, lab management, etc

What is a build server?
-----------------------
* A service often running on a dedicated machine
* Handles running build scripts based on an event
* Often keeps track of build artifacts, retrieves dependencies, logs results, keeps statistics

Examples
--------
* Team Foundation Server
* Jenkins
* Hudson
* Team City
* CruiseControl.net
* Many more

Build Server Pros
-----------------
* Centralized
	* Always building from one place and avoids building off a person’s machine.. Same process every time. Always pulling files out of source control (no “oops! that made it into prod without being checked in)
* Repeatable
	* The process is scripted and can be kicked off with one click. No chance of missing a step in a run. Can be run often without taking up anyone’s time.
* Code Health Monitoring
	* Integrate more tools in other than just the compiler. Sonar, tests, style cop.
* Continuous Integration
	* Build after every commit to make sure all developer’s changes compile together. Constantly getting feedback on the code health monitoring.

Build Server Cons
-----------------
* Requires time to set up early on in the project.
* Another piece of infrastructure to maintain.
* Once you get used to automated builds, doing it by hand is a real pain.

Builds in TFS
-------------
Used for automated builds for code stored within Team Foundation System.

Components include:
* Team Foundation Server
	* Application Tier. The general TFS install is the entry point for recieving build requests.
* Build Server
	* TFBuild service. Handles builds for a team project collection in TFS by acting as a controller, agent, or both.
* Build Controller
	* Performs lightweight tasks such as logging information about the build and distributing CPU intensive tasks to build agents.
* Build Agent
	* Dedicated to a single build controller. Processor and storage intensive. Can be tagged based on capabilities, IIS, x86, x64, etc.

Infrastructure Examples
-----------------------
NA

Build Process Template
----------------------
* TFS 2010 moved from a MSBuild based build process to a Windows Workflow based build process.
	* Workflows can call out to MSBuild scripts
* The workflows are called Build Process Templates
	* Stored in source control in TFS 2012
	* Added into a new TFS project automatically
	* Good to organize in one place and include or branch into your project
* Edit them like a normal workflow
	* Custom activities can be included
	* TFS Extensions on codeplex has a lot of great ones (also supported by hosted build)
* Workflows have arguments that can be customized through the build UI as part of the build definition

Build Definition
----------------
A specific instance of a build which requires:
* Name
* Status
	* Determines if the build will honor build requests
* Trigger
	* Determines how the build is started. Manual, CI, rolling, gated, scheduled
* Source Settings
	* Sets what folders in source control are participating in the build
* Build Defaults
	* Default settings that can be changed when queuing a build such as the controller and output location.
* Process
	* Lets you pick the process template and customize the arguments
	* Pick the sln to build
	* Run unit tests?
	* Perform code analysis?
	* Publish symbols
* Retention Policy
	* Set how long build artifacts are kept around
	* Can be set based on build result 
	* You can set what should be deleted after the retention policy is triggered

Built in Features
-----------------
* Unit testing
	* Can match assemblies based on wildcards
	* Built in it will execute MSTests
	* Adaptors allow 3rd party frameworks like NUnit
	* Results of tests included with build
* Code Analysis
	* Code analysis uses the FxCop engine. Very similar in nature
	* FxCop can be used via an extention
* Deployments
	* Can add MSBuild flags to the compilation step to publish a web app to a location

Bolt on TFS Activities
----------------------
Assemblies must be checked into source control and the build controller must point to the custom assembly directory.

* Build your own
	* Build your own workflow activities to customize the process
*	Open Source (TFS Extensions)
	* FxCop
	* ClickOnce
	* FTP
	* Hyper-V
	* Lab Management
	* Zip
	* etc

Versioning
----------
TFS Extensions has a TFSVersion to help set the assemblyinfo.cs versions when building a project.

* Need to download and customize the workflow to do that
* Can set how the version number is constructed
* Lots of customization available
	* Confusing and a little error prone

StyleCop
--------
TFS Extensions has a StyleCop activity that will run stylecop on the source files and report back as part of the build process.

* Helps enforce consistent code style throughout the project
* Can be customized per project
	* We haven’t but need to. Ignore headers, ignore unit tests, etc
* Violations can be set as errors
	*	This would fail the build which seems a bit drastic
	* We treat them as warnings

