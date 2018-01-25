# Petstore CI

## What this is
This is an attempt to make a complete but not production quality system for automatically building a Java Grails program and using the new build to update an example Tomcat web application server running on a Vagrant based VM


## How to get started using it

1) Start with an Ubuntu system, such as a laptop.  This was tested on 14.04 but should work unmodified on newer versions
1) Install the vagrant and Virtualbox systems.  See https://www.vagrantup.com/intro/getting-started/install.html and https://www.virtualbox.org/manual/ch01.html
1) Install a java 8 compiler locally.  See http://www.webupd8.org/2014/03/how-to-install-oracle-java-8-in-debian.html
1) Install git locally: these are used to do the build.  The command to do this is "yum -y install git"
1) Clone this repository (https://github.com/vorsprung/petstore-ci-demo) onto an Ubuntu system.  The command to do this is "git clone https://github.com/vorsprung/petstore-ci-demo"
1) cd into the petstore-ci-demo directory
1) If you wish to use a different repository edit the installer file and alter the GITURL variable
1) Issue the command "bash ./installer".  See below for a description of what the installer installs
1) The process is now running, any changes to the repository should be picked up in a few minutes and put "live" on the Virtualbox based Tomcat web app server.  To stop the system, issue the command "bash ./uninstaller"

## Details of what the installer program does

The installer program is a bash shell script.  
Initially, it sets up a cron job to run itself every 2 minutes.  If your grails program is likely to take more than 2 minutes to build then you should increase the  time, locking for active tasks is not supported
Next, there is an initial "clone" of the site
These two steps are only supposed to happen once to set things up
After this, a "git pull" fetches and merges changes.  This command should run everytime the installer script is called from the crontab
If the "git pull" gets no changes, everything stops there

Otherwise, a build is run locally and then control passes to the Vagrant box

## Details of what the Vagrant file does
The Vagrant file brings up an Ubuntu 16.04 box and then executes a built in shell script
This script configures the box to disable the default password, ensures some required packages are installed and then copies in the new WAR file and restarts the Tomcat web app server
