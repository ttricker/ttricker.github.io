---
layout: post
title:  "Build System Design"
date:  2013-08-08
categories: [Coding Testing]
---
# Build System Programming #


One of the things that amazes me is how the bad build systems are in many of the 
projects, I've had to work on. 


- Build System Targets
  - Configure - software configuration. The best example of this is kconfig
  - Test Build Environment - testing the machine environment
  - GenerateDocumentation  - any structured documents or automated documentation
  - Compile - building the actual software
  - Test Software - Unit tests, Suite Tests..
  - Package - Package the software for release 
  - Deploy  - Install the packaged software onto a device/machine
  - Clean   - Clean up the filesystem to remove any of the output from the prevous targets

- Build Profiles
  - Directed - User present to direct the process
  - Full     - Execute all targets
  - Slim     - Just build the minimal subset of the project

