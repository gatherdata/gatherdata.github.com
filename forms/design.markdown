---
layout: default
title: Form Design
---

Form Design with Orbeon Form Builder
------------------------------------


## XPath 
XPath is a language for navigating an XML document, used within an XForm
to express various constraints like "only show this question if the
answer to some other question was blue." 

Hop over to W3Schools for a great [XPath Tutorial][1].

## Supported Field Controls
Orbeon lets you create much more elaborate XForms than will run on
JavaRosa. Until everyone is using Android phones, restrict form
controls to the following:

* Single-line text
  * any data type
* Radio Buttons
  * data must be String

## Skip Logic
Skip logic is another way of saying "don't show this question to the
user if such-and-such is true." Within an XForm skip logic is specified
using an xpath expression for the `relevant` attribute of a field.
Orbeon conveniently refers that attribute as the `visibility` of the
control. 

## Default Value
Standard XForms have a `default` attibute which contains an xpath expression
that should be used to generate the default answer to a question. Orbeon
calls this the `initialization` value. 

JavaRosa supports its own variation on this facility: a field can have a
pre-loaded value. A normal `default` value is limited to whatever you can
derive from the XForm using an xpath expression. A pre-loader essentially
maps a custom method call with arguments that is executed by the JavaRosa
engine. You could, for instance, grab a unique identifier for the phone
and initialize an answer with that value. 

Orbeon knows nothing about this. To use pre-loader values, we use an
xpath expression which Orbeon will accept, but which later gets transformed
into the required JavaRosa pre-loader call. It looks like this:

  preload/{preloader-name}/{preloader-arguments}

Where {preloader-name} is the name of a particular pre-loader and {preloader-arguments}
are values to pass to the pre-loader. See [Preloaders](/forms/preloaders.html) for 
available implementations. 

## Repeating Section
Often there may be a set of questions that make sense to answer
multiple times on a single form. Like having multiple items on
an order form. Orbeon Form Builder does not support this, but JavaRosa
does. Unlike other work-arounds, this feels like hacking because
you have to edit the raw html to make it happen.

First, define a section containing the question which you'd like
to have repeated. Add the questions. 

Then, use the "Edit Source" button in Form Builder to edit the
raw XHTML. Find the section which you'd like to have repeat. It will
look something like this:

    <fr:section id="some_name-section"
        bind="some_name-bind">

Just add one extra attribute to that:

    <fr:section id="some_name-section"
        bind="some_name-bind"
        repeat="true()">

That will indicate to the transform process that this section should
be made into a JavaRosa compatible repeating section. Obviously this
means that the form will behave differently in Orbeon and on the mobile
phone. But, oh well. 

References
----------
[1]: http://www.w3schools.com/xpath "XPath Tutorial"
[2]: http://felix.apache.org/site/apache-felix-karaf.html  "Apache Felix Karaf"

