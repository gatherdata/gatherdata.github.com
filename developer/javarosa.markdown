---
layout: default
title: JavaRosa Developer
---


JavaRosa Developer
==================
These notes are Ubuntu variations for the steps
described in the JavaRosa "Getting Started" wiki 
page[1].

## Developer Tools

Mercurial is the version control system used by the
JavaRosa project. For Ubuntu, the install is easy...

* `sudo apt-get install mercurial`

The Sun Web Toolkit contains the libraries and binaries
needed for building J2ME mobile applications.

* Download Sun WTK 2.5.2 then run the script.
   * Install to `/opt/WTK2.5.2`

J2ME Polish is the mobile UI framework used by JavaRosa.

* Download j2me polish. 
  * Install in `/opt/j2me-polish`

Get the JavaRosa source code from bitbucket.

* `hg clone http://bitbucket.org/javarosa/javarosa`

## j2me-demo-app

The J2ME Demo App is the project which builds a
JavaRosa based mobile application. For Ubuntu,
the paths must be corrected for the ant script
to build. Edit the build.properties file:

* `polish.home=/opt/J2ME-Polish/`
* `wtk.home=/opt/WTK2.5.2`

## Eclipse

Eclipse under Ubuntu expects the name of the project
to match the name of a directory. For two JavaRosa
projects, that is not the case. To fix this, we'll
rename the projects, then add symbolic links in the
file system so the ant scripts still work.

* Rename "lib" to "javarosa-libs"
  * right-click on "lib"
  * Refactor -> Rename
  * change to "javarosa-libs"
* Rename "core" to "javarosa-core"
  * right-click on "core"
  * Refactor -> Rename
  * change to "javarosa-core"
* Rebuild everything
  * Project -> Clean
* Fix the command line
  * `cd ${JAVAROSA_HOME}`
  * `ln -s javarosa-core core`
  * `ln -s javarosa-libs lib`
* Test that the ant script builds
  * `cd j2me/demo-app/`
  * `ant BuildCleanRunEmulator`

## Create your own JavaRosa application

The JavaRosa demo application is a great default
mobile application. To start working with your own 
custom application, first make a copy of the 
"j2me-demo-app". In Eclipse:

1. right-click on "j2me-demo-app"
2. select "copy"
3. right-click in a blank area of the "Package" view
4. select "paste"
5. change the project name
   * we'll use "my-j2me" as an example

If you accepted the default location, your new project
will build in Eclipse, but the `ant` build will fail.
Edit the build.properties to correct the paths:

* `dir.root=${basedir}/../`
* `dir.jrlibs=${dir.root}/j2me/buildfiles/`
* `dir.resources-external=${dir.root}/j2me/shared-resources/resources/`

Now "my-j2me" will build from the command line using
a clean build: `ant BuildCleanRunEmulator`

## JavaRosa build.properties

The simplest customization to JavaRosa can be made
by editing the build.properties file. Some of the
properties to note are:

* `app.name`
  * name of the application as it wil be shown on the phone
  * can contain spaces, but then `app.jarName` should be changed
* `app.jarName`
  * name of the jar file to build
  * defaults to `${app.name}.jar`
  * should not contain spaces
* `app.class`
  * fully qualified classname which starts the application
  * must be a midlet class
* `device.identifier=Generic/DefaultColorPhone`
  * J2ME Polish identifier for the specific phone model
  * use `Generic/DefaultColorPhone` while developing
  * change to a specific phone model when building a release

## My J2ME Code Changes

Before making any changes to the code, we should change the
base package names to avoid confusion with the original
demo application. In Eclipse:

1. expand the "my-j2me" project, then the "src" directory
2. for each java package...
   1. right-click on the package name
   2. select "Refactor -> Rename"
   3. change the `org.javarosa.demo` portion of the name
      to `my.javarosa`
3. fix imports
   * the refactor may not have correctly updated imports
   * for each problem, manually update to the new package
   * or, do a search/replace text search on just this 
     project, replacing `org.javarosa.demo` with the
     new package name `my.javarosa`
4. change the midlet name in the build.properties 
   * `app.class= my.javarosa.midlet.JRDemoMidlet`
5. test the build
   * `ant BuildCleanRunEmulator`

## JRDemoContext

The launch sequence of the application starts with the
J2ME Midlet `my.javarosa.midlet.JRDemoMidlet`, which
uses `my.javarosa.applogic.JRDemoContext` to initialize
the settings, storage and application features.

* JRDemoContext#loadForms()
  * writes built-in forms to local storage
  * to add a form, first copy the file to `resources`
    then, add a line here to load the form

## JRDemoLoginState

This state is entered after the splash screen is displayed.

* JRDemoLoginState#loggedIn()
  * called upon successful authentication
  * starts the next state
  * to skip directly to form list:
    * comment out the line for `JRDemoPatientSelectState`
    * uncomment the line for `JRDemoFormListState`

## References

 [1] http://bitbucket.org/javarosa/javarosa/wiki/GettingStarted "JavaRosa Getting Started"






~/workspace/javarosa
~/workspace/upchrosa


