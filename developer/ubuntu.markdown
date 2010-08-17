---
layout: default
title: Ubuntu Developer Environment
---

Ubuntu Developer Environment
============================


## Ubuntu + VirtualBox

Oracle's VirtualBox makes it easy to spin up Virtual Machines on demand,
giving you a perfect environment for developing and working with GATHERdata.

1. Download, then install VirtualBox[1]
   * get the version appropriate for your host platform
2. Download Ubuntu Desktop[2] ISO image
   * again, get the version appropriate for your host
3. Create a new Virtual Machine
   * Name: whatever-you-prefer
   * Operating System: Linux
   * Version: Ubuntu
   * Memory: at least 1024MB
   * Storage: dynamic expanding, at least 8GB
4. Attach Ubuntu ISO to the VM
   1. Select the Virtual Machine
   2. Click "Settings"
   3. Go to "Storage" section
   4. Select the DVD icon in the "Storage Tree"
   5. Under "Attributes" click the icon next to "CD/DVD Device:"
      * this opens the "Virtual Media Manager"
   6. Click the "Add" icon
   7. Browse to the Ubuntu ISO image, select it
   8. Click "OK" to dismiss the "Settings"
5. Boot the VM, follow Ubuntu install instructions
   * default/recommended settings are fine
   * enjoy the slideshow, or practice making origami cranes 
6. Finished? Unmount the DVD image, reboot

## Virtual Box Guest Utils

For better integration with the Host OS, install
the Virtual Box Guest Utils. Open a terminal, then:

1. `sudo apt-get install virtualbox-ose-guest-utils`
2. `sudo apt-get install virtualbox-ose-guest-X11`

To add a Shared Folder to the machine: reboot, then
mount the shared folder. Like this: 

1. `mkdir host`
2. `sudo mount -t vboxsf folder_name path_to_mount_point`

Note that the folder name and mount directory should be
different, otherwise `mount` may throw a protocol complaint.

## Developer Tools

To build your own GATHERdata implementation, or to pitch in
developing the common services, you'll need the following tools:

1. Install Sun JDK[3]
   1. `sudo apt-get install sun-java6-bin sun-java6-jre sun-java6-jdk`
   2. `sudo update-java-alternatives -s java-6-sun`
2. Install maven[4]
   * `sudo apt-get install maven2`
3. Install git[5]
   * `sudo apt-get install git-core`
4. Instal tig (optional)
   * `sudo apt-get intall tig`

## Get the Source

The project source code is currently broken up into a collection of
"features" focused on providing various services, with each hosted in
a separate repository. This will soon be refactored into a monolithic
repository and a build system that produces multiple artifacts. 

Until then you have to pull down each feature separately. For instance:

1. Browse the GATHERdata project on GitHub[6]
2. Clone a project
   * `git clone git://github.com/gatherdata/gather-commons.git`
3. Build the project
   * `cd gather-commons`
   * `mvn clean install`
4. Run the project
   * `mvn pax:provision`
5. Update from repository
   * `git pull`

## Gather Features

The Gather project source code is grouped into separate features,
each of which is kept in its own repository.

* gather-commons
  * common models, parent classes and utilities
  * wrapped 3rd party libraries 
  * http://github.com/gatherdata/gather-commons
* gather-archiver
  * archival storage
  * keeps raw data with metadata and SHA1 hash
  * http://github.com/gatherdata/gather-archiver
* gather-alert
  * alert system
  * applies scripted "rules" to incoming data
  * send alert notifications 
  * http://github.com/gatherdata/gather-alert
* gather-camel
  * message routing using Apache Camel
  * will be merged into commons and individual features
  * can be used for a "headless" gather server (no web ui)
  * http://githhub.com/gatherdata/gather-camel
* gather-data
  * generic form data storage
  * currently "single-bucket"
  * dynamic tables will be available soon (merged from private branch)
  * http://github.com/gatherdata/gather-data
* gather-forms
  * support for Orbeon forms
  * support for JavaRosa forms
* gather-sling 
  * Apache Sling web application framework

## Application Servers

1. Tomcat 6
2. Apache Karaf
3. MySQL


## Optional Installs

* Avahi - zeroconf local IP names
  1. `sudo apt-get install avahi-daemon`
  2. `cd /etc/avahi/services`
  3. `sudo cp /usr/share/doc/avahi-daemon/examples/ssh.service .`
* TIG - ncurses based GIT repository browser
  * `sudo apt-get install tig`


## GATHERmobile

Interested in developing on the mobile phone side? Read up about [JavaRosa](/developer/javarosa.html).


## References

 [1]: http://www.virtualbox.org/ "VirtualBox"
 [2]: http://www.ubuntu.com/GetUbuntu/download "Ubuntu Desktop Download"
 [3]: http://java.sun.com/javase/downloads/index.jsp "Sun JDK"
 [4]: http://maven.apache.org/ "Apache Maven"
 [5]: http://git-scm.com/ "Git SCM"
 [6]: https://github.com/gatherdata "GitHub"


