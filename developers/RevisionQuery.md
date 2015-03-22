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
# visible fields can be selected per form
[thg-revfields]
# form[.style] = fields
# RevDetailsWidget has default (expanded) and compact (single-line) styles
revdetails = cset branch obsolete close user ... git
revdetails.compact = rev summary
# '|' means '\n' for quiet (label) style, '<p>' for verbose (panel) style?
update = rev branch tags | summary

# templates for widgets that use QTextDocument
[thg-revtemplates]
# field[.style] = template; or field.label = label
rev = {rev} (<tt>{node|short}</tt>)
rev.compact = {rev}
tags = {tags % '<span class="tag"> {tag|escape} </span> '}
summary = {desc|firstline}

# custom field can be added by user
git = {gitnode}

[thg-revlabels]
git = Git

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

### Mercurial Extensions

* `hg preparerevset` and `prepared(name)` predicate allow us to render the
  same query in different styles, and retrieve chunks form the large query.
* `thgrevfields(form, style)` template function applies the pre-configured
  template.

Example:

~~~~
# execute query, count items, and save it as "cslist0"
$ hg preparerevset cslist0 -r 'outgoing()' --count
# retrieve the first 20 items and the last for "prune" dialog in "list" style
$ hg log -r 'limit(prepared(cslist0), 20)' -T '{thgrevfields("prune", "list")}'
$ hg log -r 'last(prepared(cslist0))' -T '{thgrevfields("prune", "list")}'
~~~~

## Unresolved Issues

* How to handle unapplied MQ patches?
* Hyperlinks
* Stylesheets
* Cache
...

## Issues to Consider

TODO

## See Also

* [Qt: Supported HTML Subset](http://qt-project.org/doc/qt-4.8/richtext-html-subset.html)
