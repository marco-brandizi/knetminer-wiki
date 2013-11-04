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

Start Eclipse.
Go to Workbench.
Open Perspective ( ) on the top right bar.
Select Git Repository Exploring.
Right click under the Git Repositories frame.
Start a new repository location.
Add https://github.com/KeywanHP/QTLNetMiner.git as the Url and click Finish
Right click on "https://svn.code.sf.net/p/ondex/code/trunk"
Checkout... -> Checkout as a project in the workspace -> Project Nane: ondex -> Finish
Swith to the Java View ( )
Right click over ondex [trunk] -> Import... -> Maven -> Existing Maven Projects
In Advanced > Profiles type either 

rothamsted-internal-nexus (at Rothamsted) 
eclipse-folders(otherwise) 

In Advanced > Name template select [groupId].[artifactId] - Next - Finish
If Next is grey just unchek and check again one item from the Projects list
Finish
If any module is missing accept when Eclipse suggest to install it and wait until all jobs are done on the Progress tab before restarting Eclipse. 