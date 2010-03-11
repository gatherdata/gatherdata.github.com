Ubuntu Developer Environment
============================


## Ubuntu + VirtualBox

Oracle's VirtualBox makes it easy to spin up Virtual Machines on demand.

1. Download, then install VirtualBox[1]
2. Download Ubuntu Desktop[2] ISO image
3. Create a new Virtual Machine
   * Operating System: Linux
   * Version: Ubuntu
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
   * default settings are fine
6. Select pre-configured software
   * "LAMP server"
   * "OpenSSH server"
   * "Tomcat Java server"
7. Unmount the DVD image, reboot

## Developer Tools

1. Install Sun JDK[3]
   1. `sudo apt-get install sun-java6-bin sun-java6-jre sun-java6-jdk`
   2. `sudo update-java-alternatives -s java-6-sun`
2. Install maven[4]
   * `sudo apt-get install maven2`
3. Install git[5]
   * `sudo apt-get install git-core`

## Get the Source

1. Browse the gather project on GitHub[6]
2. Clone a project
   * `git clone git://github.com/gatherdata/gather-commons.git`
3. Build the project
   * `cd gather-commons`
   * `mvn clean install`
4. Run the project
   * `mvn pax:provision`
5. Update from repository
   * `git pull`

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


## References

 [1]: http://www.virtualbox.org/ "VirtualBox"
 [2]: http://www.ubuntu.com/GetUbuntu/download "Ubuntu Desktop Download"
 [3]: http://java.sun.com/javase/downloads/index.jsp "Sun JDK"
 [4]: http://maven.apache.org/ "Apache Maven"
 [5]: http://git-scm.com/ "Git SCM"
 [6]: https://github.com/ "GitHub"


