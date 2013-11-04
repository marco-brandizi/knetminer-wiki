## Installing Java
* Create a temporary directory somewhere on your hard disk. It will be referred from here on in as C:\temp (at time of writing JDK 7 Update 5)
* Obtain the standard JDK from Oracle. http://www.oracle.com/technetwork/java/javase/downloads/index.html 
* Save to C:\temp, and execute. 
* Accept license agreement.
* Click change on the “Install to:”
* Change installation directory to C:\jdk(version)\ 
* Click OK, then next. 
* A dialogue will pop up, asking for where to install the JRE, just click next.
* Click Finish.
* Close the Java SDK registration website.

## Installing Eclipse

(at time of writing Eclipse-Kepler)

* Obtain “Eclipse IDE for Java Developers” (http://www.eclipse.org/downloads) 
Save this to C:\temp 

* Unpack the zip file to C:\ (creating C:\eclipse).
* Open C:\eclipse\eclipse.ini
* Before the –vmargs line, add the following
-vm 
C:\jdk(version)\bin\javaw.exe 

* Save and Start C:\eclipse\eclipse.exe
* Set your workspace, e.g. C:\EclipseWorkspace 