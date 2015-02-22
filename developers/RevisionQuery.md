# Template-based Revision Query (Plan)

**This feature is NOT implemented yet.**

[TOC]

## Overview

* Provides asynchronous, exception-safe and customizable APIs to fetch
  revision information
* Built on top of [Repository Access and Command Execution](RepositoryAccess)
  functions
* Can accept revset query
* `csinfo`, `cslist` and `revpanel` will be superseded by this
* Templater functions in `repomodel` will be replaced by this

## Configuration

Templates can be configured by user.

~~~~{.ini}
# visible fields can be selected per widget
[thg-revfields]
# RevDetailsWidget has quiet (single-line) and verbose (expanded) styles
revdetails.quiet = rev summary
revdetails.verbose = cset branch obsolete close user ... git
# '|' means '\n' for quiet (label) style, '<p>' for verbose (panel) style?
update = rev branch tags | summary

# templates for widgets that use QTextDocument
[thg-revtemplates]
rev = {rev} (<tt>{node|short}</tt>)
tags = {tags % '<span class="tag"> {tag|escape} </span> '}
summary = {desc|firstline}

# custom field can be added by user
git.label = Git
git = {gitnode}

# templates for log model (repomodel)
[thg-logtemplates]
rev = {rev}
date = {date|localdate|shortdate}
~~~~

## API

TODO

### GUI

The following widgets can render HTML subset by using `QTextDocument`:

* `QLabel` for single revision
* `QTextEdit` for scrollable view

## Unresolved Issues

* How to handle unapplied MQ patches?
* Should templates be able to be configured per widget?
* Hyperlinks
* Stylesheets
* Cache
* How to fetch raw values at once?
...

## Issues to Consider

TODO

## See Also

* [Qt: Supported HTML Subset](http://qt-project.org/doc/qt-4.8/richtext-html-subset.html)
