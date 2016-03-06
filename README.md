With promises, `fetch`, `querySelectorAll`, weak maps, arrow functions, etc. many old features of jquery are just old. Usages changed too, nobody should still use data-attributes or XML for example.

But there are still important points missed by all the 20 LOC "alternatives", which make them much too verbose to use and unable to do anything advanced.

In this document we'll try to define what should be in a "jquery alternative" for today's browsers, and for when a framework like mithril or Angular doesn't push it out of the landscape.

Globally we need

- expressivity (including chaining)
- efficiency (jQuery is too slow)
- memory safety

Element wrapping:
=================

In order to provide all the wanted features, including function chaining, there are two general solutions:

- add many functions to native DOM objet prototypes
- wrap DOM elements (as is done in jQuery)

This document will assume the second solution is chosen. This makes it easier to handle the same way empty, one-element and multi-elements collections.

Ajax:
=====

The starting point being `window.fetch`, there's nothing generic missing.

It might be interesting to have a small REST/JSON dedicated extension but it's probably off scope.

Event handling:
===============

We need `on` and `off` to ensure

- methods working the same for collections of any size (including the empty ones)
- delegation

Data:
=====

The only important use is the ability to map data to DOM elements in a way ensuring there's no memory leak or data transformation.

Assuming there's no point in calling the `data` function on collections of more than one element, and that we're not interested in magical parsing of data attributes, it's [easy](https://github.com/Canop/miaou/blob/master/src%2Fmain-js%2Fjquery.dat.js) with WeakMap and Map.

SVG:
====

SVG element manipulation should be done as easily as for other elements (whenever possible and logical, the goal isn't to cross origin or document boundaries).

each, map, reduce:
==================

A short function to get an array from any collection (including the empty or one-element ones) should be enough today. The wrapping object should also act as a decorator for ease of use.

Object initialization:
======================

We want the constructor to provide, depending on the argument:

- html or svg parsing
- direct element(s) wrapping
- query based selection

Class and CSS manipulation:
===========================

We globally need the same functions as jQuery, they will be simpler to provide now though.

Effects:
========

*To be discussed*

Dimensions:
===========

*To be discussed, better API are probably possible*

Traversing:
===========

Most jQuery functions are useful here. We especially need the find, closest, filter, end, eq, not, children functions. We'll avoid parsing the selector by just using the querySelectorAll function. In order to provide `:visible` filtering a specific filtering function will probably be used.
