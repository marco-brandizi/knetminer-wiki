Installation of the QTLNetMiner web server
================================

For each instance of QTLNetMiner, there are two different programs: A **server** and a **client**. The **client**  is deployed on a Tomcat server and holds the application presentation/interface including the web-page with JavaScript and pictures. It broadcasts all of its application-specific requests to the **server** which contains the application logic and data processing. The application server is implemented as a Java Servlet listening to a specific port and producing request specific datasets for the client to visualize. First time the Servlet is started it parses the organisms-specific OXL file into memory and creates pre-build indices for fast searching. Since genome-scale networks can be very large and are stored in memory we recommend to have a minimum of 10GB RAM available on the server.

Software to install:
-----------------------

A fairly recent Java version (should already be installed on most systems)
Tomcat web server
QTLNetMiner
Maven
Eclipse Luna (version 4.4 or above) (For development and deployment, NetBeans IDE also works, this guide uses Eclipse)

Installation:
--------------

Everything follows tested on Fedora 20, Kernel 3.18.9-100.fc20.x86_64, Tomcat 7.0.52.0, Eclipse 4.4.2, Maven 3.1.1

Tomcat
---------

Install and start Tomcat on Fedora:

    sudo yum install tomcat tomcat-webapps tomcat-admin-webapps

.. on Ubuntu:

    sudo apt-get install tomcat tomcat-admin

To start Tomcat (it may already be running on Ubuntu, check using “service tomcat status”)

    sudo service tomcat start

At this point, “localhost:8080” and “localhost:8080/manager” should work. The second “manager” link should ask for username/password, you can set that one in /etc/tomcat/tomcat-users.xml like this (with a more reasonable password of course, with a bit of extra configuration it is also possible to use SHA or MD5 digested passwords instead of the insecure plaintext, see for example http://www.badllama.com/content/encrypting-tomcat-usersxml-passwords )

    <user name="admin" password="password" roles="admin,admin-gui,manager-gui" />

Restart tomcat:

    sudo service tomcat restart

and the login should work. If it doesn't work, check the logs under /var/log/tomcat/ to see what went wrong.

Later on, deployment manually and via Eclipse will talk to Tomcat's manager.

Maven
--------

Install Maven on Fedora:

    sudo yum install maven

.. on Ubuntu:

    sudo apt-get install maven

Maven is used to compile and deploy QTLNetMiner to your server.

Eclipse
---------

Install Eclipse:

    sudo yum (or apt-get) install eclipse

Install the Maven plugin m2e for Eclipse:
In Eclipse, go to Help → Install New Software...
Under “Work with:” enter “http://download.eclipse.org/technology/m2e/releases”
Press Enter
In the below list, something similar to “Maven Integration for Eclipse” should appear
Select this package, click “Next”, it will be installed
IMPORTANT: m2e needs at least Eclipse 4.4, if that one isn't installed the plugin installation will not work.
In newer versions of Eclipse there is a good chance that m2e is already installed and it will show this at this point.

QTLNetMiner
------------------

Get QTLNetMiner from Github:

    git clone  https://github.com/KeywanHP/QTLNetMiner

The folder “common” includes all of the actual functionality, the other folders (“arabidopsis”, “poplar”, etc.) contain project-specific settings. Remember DRY?

Import QTLNetMiner into Eclipse (optional):
Click “File → Import... → Maven → Existing Maven Projects” (If there is no Maven-option here, the installation of m2e was not successful or hasn't finished yet)
In the new window, click “Browse”
Navigate to the root directory of QTLNetMiner (the one with the files “README.md”, “LICENSE” and “pom.xml”), click “Select All”, click “Advanced” and select “[artifactId]” in “Name template”, and click “Finish”.

There will be a few warnings (like “GroupId is duplicate of parent groupId” but you can ignore these.

These projects also depend on Ondex code, so we have to import that one into Eclipse, as well. 

Customising both server and client
------------------------------------------

Now, you can copy/paste any of the available folders that are not “common” and use that as the base for your own project. As an example, let's make one for “human”. So copy “pig” to “human”:

    cp -r pig human

Customising “client”
--------------------------

There are two important files that have to be changed per project in the folder “client”: config.xml and utils-config.js

client/src/main/webapp/html/javascript/utils-config.js

    var data_url = "http://qtlnetminer-test.rothamsted.ac.uk/arabidopsis_data/";

has to be replaced by a valid path in which QTLNetMiner stores temporary results, for example, “http://localhost:8080/test_data”

    var data_url = "http://localhost:8080/test_data";

Make sure that this folder actually exists where your ROOT directory is and has the right owner (“tomcat:tomcat” by default).

Also change the other variables in that file to something reasonable for display purposes.

client/src/main/resources/config.xml

    <entry key="ServerHost">qtlnetminer-test</entry>

has to be replaced by a valid URL on which Tomcat runs, for example, localhost:8080 

    <entry key="ServerHost">localhost:8080</entry>

In client/src/main/webapp are also HTML and jpg files that have to be customised to fit to the new project.

Files that have to be changed:

    client/src/main/webapp/html/index.jsp

Change information about the given organism.

    client/src/main/webapp/html/release.html

Change details about the data used.

    client/src/main/webapp/html/image/organism.png
Replace by appropriate picture or delete (but then make sure to also delete the image in index.jsp).

    client/src/main/webapp/html/data/basemap.xml

This file needs start, end positions and colors for all chromosomes.

    client/src/main/webapp/html/data/geneposition.txt

This file needs start, end and chromosome for all known genes.

    client/src/main/webapp/html/data/sampleQuery.xml

Add details for sample queries.

    client/pom.xml

Replace the name of the organism and increase modelVersion if needed.

    QTLNetMiner/pom.xml

The QTLNetMiner pom.xml file also needs to have the name of your project folder (as a “module”) in order for it to be built and packaged as a part of the main QTLNetMiner project.


Customising “server”
--------------------------

The most important file to customize is server/src/main/resources/config.xml
The directory under <entry key="DataPath"> has to point to the same directory the “client” talks to under “var data_url” (i.e., "http://localhost:8080/test_data") but not from the point of view of the client (URL + path) but from the point of view of the server (complete path, i.e.,  “/var/www/qtltserver/test_data”).
The server port has to be identical to “client”'s server port.

The following files have to be customized as well:

    server/src/main/resources/chromosomes.xml

Change to the number and names of chromosomes.

    server/src/main/scripts/startup.sh:

The given OXL file has to point to the OXL file generated above using Ondex. Also adjust just the java parameters based on your server: -Xmx6G means that Java can use a maximum of 6 GB of RAM. Also change the path to Java from “/usr/java/latest/bin/java” to where it is on your server (use “which java” or just “java” should be enough, too).

Deploying QTLNetMiner's “server” and “client” to your server
----------------------------------------------------

First, the project has to be compiled into a package. In the root folder of your new project (in our example, “human”) there should be two folders you have now customised: “server” and “client”.

In the project's base directory (e.g., “human”) run 

    mvn package

which will iterate through the “server” and the “client” folder and package both projects into zip-files inside the sub-folders named “target”.

The final output should contain “BUILD SUCCESS”.

Deploying QTLNetMiner client
--------------------

The easiest way to deploy “client” manually is to find the newly created “.war”-file in client/target/ and upload that in the “Select WAR file to upload” field under your_server/manager, i.e., localhost:8080/manager.
In the Applications table on that page you will see a new Path, something like “QTLNetMinerHuman” based on what you entered in the pom.xml, which is your new web page.

You can also deploy the client using Eclipse as described here by talking to Tomcat's manager (setup described above): https://github.com/KeywanHP/QTLNetMiner/wiki/QTLNetMiner-Development-Installation-Guide#how-to-use-automatically-deploy-a-war-file-to-a-remote-tomcat-server

You can also use any automatic deployment software such as Hudson, but that's not the focus of this guide.

In the standard settings the client assumes that the server runs on the same machine, as shown by the file client/src/main/resources/config.xml. This file shows that the “client” queries the “server” on localhost on port 8189. 
You can change port and URL as long as you also change the relevant details in “server” in server/src/main/resources/config.xml

Deploying QTLNetMiner server
--------------------------------------

You have to make sure that client and server can talk to each other. This  gets hard once both programs run on different machines and things like firewalls etc. come into play. So far QTLNetMiner client and server always run on the same machine to circumvent this particular headache.

After running “mvn package”, there is a zip-archive in server/target/ ending in “packaged-distro.zip”, for example “pig-server-0.5.0-SNAPSHOT-packaged-distro.zip”. Copy this zip-file to your server and unzip it somewhere where you can find it again. Copy the OXL file from above into the same directory and make sure that the OXL file given in runme.sh points to the OXL file you just copied.

Check runme.sh again to see whether Xmx is set to something reasonable and whether the path to Java and the OXL file actually exist. Then, start runme.sh and check stdout.log for any errors. It is advisable to first start java manually without redirecting of STDOUT and STDERR and without nohup to catch any simple mistakes.

You should now be able to enter queries into the web-page, the server should create temporary files in your data_url's folder and results should be displayed.