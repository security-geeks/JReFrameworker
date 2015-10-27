---
layout: default
---

This tutorial outlines the steps to create an attack module with JReFrameworker to modify the behavior of the Java runtime's `java.io.File` class to return false if the file name is "secretFile" regardless if the file actually exists or not.

## Creating a JReFrameworker Project

Each "attack module" consists of an Eclipse JReFrameworker project.  An attack module consists of annotated Java source code, which defines how the runtime environment should be modified. A module may also contain test code that uses the runtime APIs that will be modified.  The test code can be used to execute and debug the attack module in the modified as well as original runtime environments.


To create a new attack module, 

*More coming soon. This tutorial is under construction.*