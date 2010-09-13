---
layout: default
title: Apache Felix Karaf
---

Apache Felix Karaf
==================
Apache Felix Karaf[1] is the OSGi runtime of choice for Gather 
deployments.

Karaf includes a shell environment for managing the runtime. In the
following steps, unix-shell scripts will start with a '$' and Karaf
shell scripts start with a '>'.

Install Apache Karaf
--------------------

1. `$ cd /srv/gather`
2. `$ sudo wget http://apache.cs.utah.edu/felix/apache-felix-karaf-1.4.0.tar.gz`
3. `$ sudo tar xzf apache-felix-karaf-1.4.0.tar.gz`
4. `$ sudo ln -s apache-felix-karaf-1.4.0 karaf`

Configure Proxy Settings
------------------------

Karaf uses maven URLs for downloading jars from repositories. If your
server uses an http proxy, then maven must also be configured to use 
that proxy. 

Edit the global maven settings.xml file, located at `/etc/maven2/settings.xml`
and edit the proxy settings as indicated by 
[Maven Proxy Settings](http://maven.apache.org/guides/mini/guide-proxies.html).

Then, Karaf needs to be told where to find the global settings file. Edit
the pax-url configuration located at `/srv/gather/karaf/etc/org.ops4j.pax.url.mvn.cfg`
and set the value of `org.ops4j.pax.url.mvn.settings`. For Ubuntu, it would be:

    org.ops4j.pax.url.mvn.settings=/etc/maven2/settings.xml


Install Service Wrapper
-----------------------
Karaf includes a linux initialization script wrapper that can be
deployed, enabling it to start and stop as a system service.
To install the service, follow these steps:

1. `$ cd /srv/gather/karaf`
2. `$ sudo ./bin/karaf`
3. `> features:install wrapper`
4. `> wrapper:install`
5. `> shutdown`

Then follow the instructions provided by the wrapper. If the wrapper
script fails to start karaf, it is possible that the Java Service 
Wrapper which was installed is wrong. The error complaint will be
something like:

    exec: 541: /srv/gather/apache-felix-karaf-1.4.0/bin/karaf-wrapper: not found

Follow the instructions below to get the appropriate version and 
replace the broken bits. 

Also, the wrapper-service script attempts to place the data directory
in the wrong place. Having the data directory somewhere outside of 
karaf makes sense, though there is an open jira issue to enable that.
For now, edit the karaf-service script:

1. Edit the service script
   * `vi /srv/gather/karaf/bin/karaf-service`
2. Add a data directory variable to the top

    `$KARAF_DATA="/srv/gather/apache-felix-karaf-1.4.0/data"`

3. Change the PIDDIR to use the data directory variable

    `PIDDIR="$KARAF_DATA`

4. Edit the start() function, using the `KARAF_DATA` variable
   in place of the relative path to the data directory. 

For the last step, change this...

    if [ ! -d ../../data ]; then
        mkdir ../../data
    fi
    if [ ! -d ../../data/log ]; then
        mkdir ../../data/log
    fi

to this...

    if [ ! -d $KARAF_DATA ]; then
        mkdir $KARAF_DATA
    fi
    if [ ! -d $KARAF_DATA/log ]; then
        mkdir $KARAF_DATA/log
    fi


Fix a broken karaf-wrapper
--------------------------

1. Download the correct Java Service Wrapper for you platform
   * get the "Community Edition"
   * download to a temp working area
   * http://wrapper.tanukisoftware.org/doc/english/download.jsp
   * `wget http://wrapper.tanukisoftware.org/download/3.3.9/wrapper-linux-x86-64-3.3.9.tar.gz`
2. Expand the archive
   * `tar xvzf wrapper-linux-x86-64-3.3.9.tar.gz`
3. Copy the pieces into the karaf install
   * `cd wrapper-linux-x86-64-3.3.9`
   * `sudo cp ./bin/wrapper /srv/gather/karaf/bin/karaf-wrapper`
   * `sudo cp ./lib/libwrapper.so /srv/gather/karaf/lib/`
   * `sudo cp ./lib/wrapper.jar /srv/gather/karaf/lib/karaf-wrapper.jar`
4. Try starting the service
   * `sudo /usr/sbin/service karaf-service start`

Create a client instance
------------------------
A client instance lets you log in to the karaf server shell. 
You'll have to run karaf directly, create the instance, 
restart it as a service, then run the client and ssh over
to the server instance.

If the server is already running as a linux service, first 
stop it using `sudo /usr/sbin/service karaf-service stop`.
Then change to the `/srv/gather/karaf`

1. `$ sudo ./bin/karaf`
2. `> admin:create client`
3. `> shutdown`
4. `$ sudo service karaf-service start`
5. `$ cd /srv/gather/karaf/instances/client/`
6. `$ sudo ./bin/karaf client`
7. `> ssh -p 8101 -l karaf -P karaf localhost`

After ssh'ing to the server instance, notice that the karaf
prompt has changed from `karaf@client` to `karaf@root`.
When you're done, just `^d` to end your session. You'll return
to the client, which you'll also end with a `^d`.

Customize Karaf
---------------
The configuration files in `/srv/gather/karaf/etc` need to
be modified for GATHER. We'll download an artifact from the
GATHER public repository, then expand it to overlay updated
configuration.

First, shutdown the karaf service and clean out the data directory:

1. `$ cd /srv/gather/karaf`
2. `$ sudo service karaf-service stop`
3. `$ sudo rm -rf data/`

Then get the custom configuration:

1. `$ sudo wget http://repository.gatherdata.org/content/repositories/releases/org/gatherdata/gather-karaf/org.gatherdata.karaf.base/1.4.0/org.gatherdata.karaf.base-1.4.0-etc.tar.gz`
2. `$ sudo tar xvzf org.gatherdata.karaf.base-1.4.0-etc.tar.gz`

If you've already modified any of the configurations (perhaps
for pax-url proxy settings) your changes may be lost. 

Restart karaf, log in and check the feature listing:

1. `$ sudo service karaf-service start`
2. `$ cd instances/client`
3. `$ sudo ./bin/karaf client`
4. `> ssh -p 8101 -l karaf -P karaf localhost`
5. `> features:list`

You should see a long listing of gather- and camel- features.
The camel-karaf feature repository introduces an outdated 
dependency on the karaf-1.1.0-SNAPSHOT repository. Remove
that before continuing.

* `> features:removeUrl mvn:org.apache.felix.karaf/apache-felix-karaf/1.1.0-SNAPSHOT/xml/features`

GATHER expects two external web applications to be deployed:
Orbeon Forms and Pentaho Reporting. Both must be deployed to
Tomcat (or similar application server). The base urls must
be adjusted in the file `etc/org.gatherdata.app.sling.cfg`.

Install GATHER Features
-----------------------
By default, the base features are:

* ssh,management


To these, add these karaf features:

1. `> features:install obr`
2. `> features:install http`
3. `> features:install webconsole`
4. `> features:install spring`

Then, add the GATHER features in this order:

1. `> features:install google-peaberry`
2. `> features:install gather-core`
3. `> features:install gather-commons.db4o`
   * for features that use [DB4O Persistence](http://www.db4o.com/)
4. `> features:install eclipselink`
   * For features that use [EclipseLink JPA](http://www.eclipse.org/eclipselink/)
5. `> features:install gather-hsqldb`
   * For JPA storage to [HSQLDB](http://hsqldb.org/)
   * includes HSQLDB library and embedded server
6. `> features:install gather-mysql`
   * For JPA storage to [MySQL](http://www.mysql.com/)
   * includes MySQL JDBC driver
7. `> features:install gather-archiver.db4o`
   * basic raw-data storage
8. `> features:install gather-alert.all`
   * alert service
9. `> features:install gather-forms.db4o`
   * support for orbeon forms publishing
10. `> features:install gather-data.jpa`
   * flexible data storage
11. `> features:install camel-osgi`
   * OSGi-friendly routing
12. `> features:install gather-camel`
   * gather common camel components 
13. `> features:install sling-dependencies`
   * general dependencies for Apache Sling
14. `> features:install sling-karaf`
   * Karaf specific dependencies for Sling
15. `> features:install apache-sling`
   * Apache Sling framework
16. `> features:install gather-sling`
   * Gather Sling-based UI

References
----------

[1] http://felix.apache.org/site/apache-felix-karaf.html "Apache Felix Karaf"
[2] http://sling.apache.org/site/index.html "Apache Sling"

