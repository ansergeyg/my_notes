Java downloads:

USE THIS:

https://www.oracle.com/java/technologies/downloads/


NEVER USE THIS IF YOU WANT TO INSTALL THE LATEST VERSION OF JAVA AND ENVIRONMENT:

https://www.java.com/en/download/manual.jsp

If you used the path above and it brought to you some version of JRE that was not right and then you uninstalled that version and installed the latest version

you may face this issue:
         
C:\solr\solr-9.4.0\bin>solr start

ERROR: The system was unable to find the specified registry key or value.

ERROR: The system was unable to find the specified registry key or value.

Please set the JAVA_HOME environment variable to the path where you installed Java 1.8+

How to fix:

open windwow start menu and start typing environmenal varible you will see in the search: Edit system environmental variables. Click on it. In the popped up window click on Environmental Variables.

Add new system variable:

name: JAVA_HOME

value: Path to your jdk: (example: C:\Program Files\Java\jdk-21)

Then edit the path variable by adding:

%JAVA_HOME%\bin

Save everything.

Don't forget to restart your system.
