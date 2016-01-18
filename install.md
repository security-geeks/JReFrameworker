---
layout: page
title: Install
permalink: /install/
---

Installing the JReFrameworker Eclipse plugin is easy.  It is recommended to install the plugin from the provided update site, but it is also possible to install from source. The current version of JReFrameworker has been tested on [Eclipse Luna](https://eclipse.org/luna/) and [Eclipse Mars](https://www.eclipse.org/downloads/packages/release/Mars/1).
        
## Installing from update site
Follow the steps below to install the JReFrameworker plugin from the Eclipse update site.

1. Start Eclipse, then select `Help` &gt; `Install New Software`.
2. Click `Add`, in the top-right corner.
3. In the `Add Repository` dialog that appears, enter &quot;JReFrameworker&quot; for the `Name` and &quot;[http://ben-holland.com/JReFrameworker/updates/](http://ben-holland.com/JReFrameworker/updates/)&quot; for the `Location`.
4. In the `Available Software` dialog, select the checkbox next to "WAR Binary Processing" and click `Next` followed by `OK`.
5. In the next window, you'll see a list of the tools to be downloaded. Click `Next`.
6. Read and accept the license agreements, then click `Finish`. If you get a security warning saying that the authenticity or validity of the software can't be established, click `OK`.
7. When the installation completes, restart Eclipse.

## Installing from source
If you want to install from source for bleeding edge changes, first grab a copy of the [source](https://github.com/benjholla/JReFrameworker) repository. In the Eclipse workspace, import the `jreframeworker`, `jreframeworker.engine`, and `org.objectweb.asm` Eclipse projects located in the source repository. Note that you must have the Eclipse Plugin Development Tools installed in Eclipse to compile the plugin projects. Right click on the `jreframeworker` project and select `Export`.  Select `Plug-in Development` &gt; `Deployable plug-ins and fragments`.  Select the `Install into host. Repository:` radio box and click `Finish`.  Press `OK` for the notice about unsigned software.  Once Eclipse restarts the plugin will be installed.

## Changelog

### 1.1.1
- Improved payload dropper with new command line options for specifying non-standard runtime locations and for specifying output options

### 1.1.0
- Support for exporting a basic based payload dropper

### 1.0.2
- Improvements to preferences
- Bug fixes for builder

### 1.0.1
- Bug fix for missing annotations Jar in new projects

### 1.0.0
- Initial Release