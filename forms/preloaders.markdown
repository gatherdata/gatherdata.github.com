---
layout: default
title: Form Pre-loaders
---

Form Pre-loaders
----------------
Pre-loaders are used to initialize the answer to a form question
using custom code running on the mobile phone. This mechanism
enables a form to include other interesting information. 

Like...

## Persistent Properties

  preload/property/{property-name}

A persistent property will preserve its value from one form
to the next. This can save a user from providing the same
answer over and over again. If an answer is likely never to
change for a user (like their name), consider using a persitent
property to annoy them less.

## GPS Location

  preload/geo/{lat | lon | alt}

For phones with GPS support, this pre-loader can provide
latitude, longiture and altitude decimal values. 

## Facebook Status

  preload/fb/status

OK, just kidding. But, if you really wanted to you could
implement a custom pre-loader to grab any value available
on the mobile phone or a quick http-get.

Someday we'll write a helpful blog post aobut writing
your own pre-loader. 

## References
[1]: http://www.w3schools.com/xpath "XPath Tutorial"

