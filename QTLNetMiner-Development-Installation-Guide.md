## Installing Java
* Install the standard JDK from Oracle. http://www.oracle.com/technetwork/java/javase/downloads/index.html 

## Installing Eclipse

(at time of writing Eclipse-Kepler)

* Download “Eclipse IDE for Java Developers” (http://www.eclipse.org/downloads) 
* Unpack the zip file (creating D:\eclipse).
* Open D:\eclipse\eclipse.ini
* Before the –vmargs line, add the following
-vm 
PATH_TO_JDK\bin\javaw.exe 

* Save and Start D:\eclipse\eclipse.exe
* Set your workspace, e.g. D:\WorkspaceGit

## Checking out QTLNetMiner as Maven Project

* Start Eclipse.
* Go to Workbench.
* Open Perspective on the top right bar.
* Select Git Repository Exploring.
* Clone a Git repository.
  * Add https://github.com/KeywanHP/QTLNetMiner.git as the URI
  * Enter Git User name and password
  * Set your Local Destination Directory
  * Finish

* File -> Import -> Maven Project -> Existing maven Project
* In Advanced, Name template select [artifactId]
* Finish
