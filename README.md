# Project Name : Application

## Description :
This is an application which will be build on a Jenkins server and deployed to the tomcat server

## Cloud Platform Used
- AWS

## Servers Used
- Ubuntu for Jenkins
- Centos for Tomcat

## Please note that it is assumed that the preferred security groups have already been setup in AWS to interact between the Jenkins server and tomcat server. Also, the desired inbound rules have been setup for the Jenkins and Tomcat to be displayed on the web browser

## Setting up Tomcat
For the above exercise I have used Tomcat 8.5.37.
Download the tomcat and extract it and keep it in /usr/local dir
Start the tomcat using ./startup.sh and below screen should be visible on http://3.20.238.13:8080/

![Image description](https://github.com/deepakautomation/Application/blob/master/09-04-20-1041.png)

Document root directory will be webapps for tomcat where tomcat will download and extract the code

We need to create a tomcat user which we will be providing in Jenkins server while deploying the app. Tomcat user can be created from below:

vi conf/tomcat-users.xml
<role rolename="manager-gui"/>
  <role rolename="manager-script"/>
  <user username="tomcat" password="12345" roles="manager-gui,manager-script"/>
  
  Next step is to disable the remote access for manager application. Though it is not recommended, need to do it for the assignment.
  This can be done from META-INF/context.xml
  
  Now remote user should be able to login to manager app user credentials tomcat, 12345
  ![Image description](https://github.com/deepakautomation/Application/blob/master/09-04-20-1057.png)
  
  ## Setting up Jenkins
  I have used ubuntu EC2 instance. There are set of commands which I have executed to setup the jenkins server.
  Jenkins url : http://18.191.142.121:8080/, admin , 12345
  
  ## Build the code and trigger build on every commit
  ![Image description](https://github.com/deepakautomation/Application/blob/master/09-04-20-1103.png)
  
  ### Please note that it is not possible to deploy .jar file to a tomcat in the right manner, therefore I have created a .war file of the project and will deploy it
  
  ## Copy Artifacts
  To copy the artifacts I have used plugin called Copy artifact
  ![Image description](https://github.com/deepakautomation/Application/blob/master/09-04-20-1227.png)
  
  Also, here I have defined which job should run next
  
  Also, I am not taking user input as in that case the process would be manual rather than automated. It will defeat the whole process of CICD.
  Plus I have never tried using manual intervention, so not picking it up.
  
  So as to generate a random .war everytime I have used ZENTIMESTAMP as the plugin which will generate a random .war file everytime the build is being run
  
  ![Image description](https://github.com/deepakautomation/Application/blob/master/09-04-20-1233.png)
  
  Workspace
  ![Image description](https://github.com/deepakautomation/Application/blob/master/09-04-20-1239.png)
  
  ideally it should have been handled in an artifact repository system like nexus
  
  ## Deploy Build
  In this build we will install a plugin called Build to container. This plugin will place the build in the tomcat webapps dir
  Here we need to mention the username and password of the tomcat defined above.
  
  ![Image description](https://github.com/deepakautomation/Application/blob/master/09-04-20-1247.png)
  
  Creating pipeline
  For this I have used Build Pipeline plugin. I could have also used JenkinsFile for this. But when Jenkins gives us easier feature, then why not to use it :)
  
  The pipeline can be triggered from the below and logs can be seen as well.
  
  ![Image description](https://github.com/deepakautomation/Application/blob/master/09-04-20-1312.png)
  
  ![Image description](https://github.com/deepakautomation/Application/blob/master/09-04-20-1308.png)
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
