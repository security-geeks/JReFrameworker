---
layout: page
title: Install
permalink: /install/
---

Installing the JReFrameworker Eclipse plugin is easy.  It is recommended to install the plugin from the provided update site, but it is also possible to install from source. The current version of JReFrameworker has been tested on [Eclipse Luna](https://eclipse.org/luna/).
        
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
If you want to install from source for bleeding edge changes, first grab a copy of the [source](https://github.com/benjholla/JReFrameworker) repository. In the Eclipse workspace, import the `jreframeworker` and `org.objectweb.asm` Eclipse projects located in the source repository.  Right click on the `jreframeworker` project and select `Export`.  Select `Plug-in Development` &gt; `Deployable plug-ins and fragments`.  Select the `Install into host. Repository:` radio box and click `Finish`.  Press `OK` for the notice about unsigned software.  Once Eclipse restarts the plugin will be installed.