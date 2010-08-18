---
layout: default
title: Introduction to Forms
---

Introduction to Forms
---------------------
The term 'form' is one of those potentially troublesome generalizing words
that can easily lead to confusion. It's not as bad as using 'thing' to 
denote a subject of interest, but just barely.

Within the context of GATHERdata, the term 'form' has nothing to do with 
Plato's realm of ideals nor Wonder Twin powers. Whenever we say 'form' we
mean a template for collecting information. The act of using a form to
collect data creates an instance of the form, or a form instance. The data
that is collected is form instance data, or just instance data. 

While this may seem needlessly pedantic, it really does help to disambiguate
conversations about the data gathering workflow.

Oh, and while we're talking about terminology: fields and the value they
hold are usually referred to as questions and answers within the documentation.


## XForms
XForms are the specification we use for defining forms. Once upon a time
some committee probably hoped that xforms would solve the horror of implementing
data input screens in html. The goal with XForms was to provide an implementation
neutral way to specify three aspects of a form:

1. what information to get from the user (the model)
2. how to present questions to the user (the view)
3. impose constraints to help the user avoid screwing things up (the controller)

Of course, out on the web nobody uses XForms because Javascript libaries have
totally obviated the need for yet-another-specification. Simple forms are cake
when you can build full-up applications in Javascript.

For us data gathering enthusiasts, though, the implementation-neutral approach 
of XForms is perfect for specifying a form that could make sense on the web, on
a mobile phone, or even using an IVR system. 

## Orbeon Forms
The GATHERdata through-the-web form design tool is based on Orbeon, which ironically
has a great custom Javascript library to renders XForms.

Thanks to Orbeon, GATHERdata forms can be deployed directly to web pages, or 
transformed for use on reasonably capable mobile phones.

## JavaRosa Forms
JavaRosa is a mobile application framework oriented around handling XForms. The style
of XForms consumed by JavaRosa varies a little from the standard, so we use a some
trickery to transform an Orbeon XForm into a compatible JavaRosa version. 

There are additional design considerations when using Orbeon for designing foms
destined for a JavaRosa based mobile application. Read more about [Form Design][/forms/design.html].
 

References
----------
[1]: http://felix.apache.org/site/apache-felix-karaf.html  "Apache Felix Karaf"

