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

1. Start Eclipse.
1. Go to Workbench.
1. Open Perspective on the top right bar.
1. Select Git Repository Exploring.
1. Clone a Git repository.
  * Add https://github.com/KeywanHP/QTLNetMiner.git as the URI
  * Enter Git User name and password
  * Set your Local Destination Directory
  * Finish
1. File -> Import -> Maven Project -> Existing maven Project
1. Click "Browse" and select the folder QTLNetMiner within the git directory selected in the step number 4
1. In Advanced, Name template select [artifactId]
1. Finish
