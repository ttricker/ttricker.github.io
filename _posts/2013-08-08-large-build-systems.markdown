---
layout: post
title:  "Build System Design"
date:  2013-08-08
categories: [Coding Testing]
---

 ## Introduction
    While being employed as a software engineer, it's a surprizingly rare opertunity to build projects from scratch (clean room implementation). Instead a code base is interrited with often millions of lines of code with many more managed from third partyibraries. In this line of work you get see some amazingly beautiful code and some really terrible code but one thing that's neglected more than the code itself is the build scripts.
   In 2013 we have some amazong build systems. Ant, Cmake, Msbuild, Scions, and Rake are all fantastic tools for software constriction and yet most of the projects I work on use window batch files to kick off builds. One project even used a windows batch files to launch a cygwin process. Then ran python scripts inside of cygwin, which then setup the environent to run more windows batch files. Besides the dependency bloat no one seemed to think it was a problem, until I requested a change in the build process.
   It's an unmantainable nightmare and it didn't fufill some of the features required for continous delivery.

quick list of important points.
- keep the build scripts independent of version control. Nothing in the build process should have any knowledge of what version control system is managing it.	
- automated means hands free. Only changing the configuration at the beginning of a build should require any user input.
- for every project the build scripts should able to
 - Test the build environment
 - Configure the source code
 - Build the source code
 - Run any automated tests (e.g. unit and integration)
 - Assemble Documentation
 - Package the project
 - Deploy the target to the next stage in the delivery pipeline.


- Build System Targets
  - Configure - software configuration. The best example of this is kconfig
  - Test Build Environment - testing the machine environment
  - GenerateDocumentation  - any structured documents or automated documentation
  - Compile - building the actual software
  - Test Software - Unit tests, Suite Tests..
  - Package - Package the software for release 
  - Deploy  - Install the packaged software onto a device/machine
  - Clean   - Clean up the filesystem to remove any of the output from the prevous targets

Implementation in Msbuild
