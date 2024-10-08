---
path: "/docs/cli-command-reference/zipped-prerequisites"
title: "CLI prerequisites offline"
---


The following section explains more about the software prerequisites that you need to install so that you are ready to install the zipped distribution for Galasa for running offline.


## Prerequisites


| Software |  Description  |
| :---- | :-------- | 
| Java JDK  | Required. Galasa tests and Managers are written in Java - you need to install a Java version 11 JDK or later to use it. _Note:_ We do not currently support Java 17 or later. After installing, you must set the `JAVA_HOME` environment variable to your Java JDK installation path and check it set successfully by running the command `echo $JAVA_HOME` on Mac or Unix, or `echo %JAVA_HOME%` on Windows (PowerShell). The returned result shows the path to your JDK installation. |
| Gradle  | Required to install the zipped distribution. You can also build Galasa projects using Gradle. (You can build projects using Maven if you prefer). Galasa projects are hierarchical file structures that provide the ability to store and run Galasa tests. All Galasa versions are compatible with Gradle releases 6.9.x.|
| Maven  | You must install either Maven or Gradle in order to build Galasa projects. Galasa projects are hierarchical file structures that provide the ability to store and run Galasa tests. You do not explicitly need to install Maven because the Galasa plugin downloads and installs it silently during its own installation and configuration. |
| Docker  | Required if using the Docker image. If you want to deploy the Docker image that is provided in the zip file, you will need to have Docker installed.  |
| 3270 emulator | Optional. Although you do not need a 3270 emulator to run a Galasa test (even if it tests a 3270 application) you can use one to [run Galasa Simbank offline](../running-simbank-tests/simbank-cli-offline), a simulated version of an application that helps you get acquainted with Galasa before connecting to a real mainframe to run your own tests. There are many such emulators available but IBM's Personal Communications (PCOMM) is frequently used, as is IBM's Host on Demand software, which includes support for Windows, Linux and MacOS.| 



## Next steps

Install the Galasa plug-in using the zipped distribution by following the instructions in [Installing the Galasa CLI offline](installing-offline). 