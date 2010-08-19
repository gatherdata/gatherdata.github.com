---
layout: default
title: Applying GATHERdata
---
# Applying GATHERdata

While a default GATHERdata deployment is useful as is, most
implementors have special requirements that build upon the
basic features, creating an "implementation application"
that uses GATHERdata as a framework.

Let's walk through the typical workflow of creating a custom
implementation application.

## GATHERdata End-to-End
It's all about XForms. They are the central document handled 
by GATHERdata. Typically, the form specifies the domain model
for the implementation application, describing what information
is being collected. Everything else is a consequence of the form.

An implementator starts by designing a form using Orbeon Form Builder. 
The published form gets sent out to mobile phones. Field workers use 
GATHERmobile to fill out form instances, then submit xforms instance 
data back to the server for storage and reporting.

## Orbeon Form Builder
Gather integrates with Orbeon's Form Builder[1] to provide a
web-based form editor.

* web application deployed to Tomcat
* embedded eXist database for form storage
* "published" forms saved to Gather
* form data is non-hierarchical (no repeating fields)

### Orbeon-to-Gather
Gather implements a web service interface which is 
compatible with the eXist database. Orbeon is
configured to use the Gather endpoints when publishing 
a form that is ready for use.

Published forms are available for download by mobile
phones running the GATHERmobile client. They are transformed
on the fly using an XSLT stylesheet. Not all XForms features
are supported by the JavaRosa engine, so the forms should be
tested before being made available for general users. (See
[Form Design](/forms/intro.html) to learn more)

## GATHERmobile
GATHERmobile is a J2ME application based on JavaRosa[3] which allows
users to fill out forms using their mobile phone. The primary
development environment is Windows, although Ubuntu is a popular
second choice. Most mobile handset manufacturers (notably Nokia)
only support Windows. 

* J2ME
* JavaRosa - open source mobile xforms application
* Windows development environment
* built-in and downloadable forms

## Storage


## Reporting

## References

 [1]: http://www.orbeon.com "Orbeon Forms"

 [2]: http://felix.apache.org/site/apache-felix-karaf.html "Apache Felix Karaf"

 [3]: http://code.javarosa.org "JavaRosa"
