---
layout: default
title: Pentaho Reporting
---


Pentaho Reporting
=================

Gather uses Pentaho[1] for reporting.

## Install Pentaho

* `cd /srv/gather/`
* `sudo mkdir pentaho`
* `cd pentaho`
* `sudo tar xvzf ~/biserver-ce-3.5.2.stable.tar.gz`

## Install pentaho-service boot script

* `sudo cp pentaho-service /etc/init.d/`
* follow instructions at the Pentaho Wiki[2]

## References

 [1]: http://reporting.pentaho.org/ "Pentaho Reporting"
 [2]: http://wiki.pentaho.com/display/ServerDoc2x/Starting+the+BI+Server+At+Boot+Time+On+Linux "Pentaho on Linux"
