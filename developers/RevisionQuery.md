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

## Change History

In Mercurial 3.4:

* fix `{get(extras, key)}`
* fix `{function()|filter}`

In Mercurial 3.5:

* [template support in formatter](https://selenic.com/repo/hg/rev/0c6f98398f8a)

In Mercurial 3.6 (unreleased):

* `{localdate(date, "UTC")}` template function

### TODOs

short-term (in Mercurial 3.6, TortoiseHg 3.7?):

1. implement prepared revset extension (WIP)
1. implement template extension (WIP)
1. implement APIs for asynchronous query (WIP)
1. `limit(set, n, offset)` revset?
1. add incremental fetcher?

long-term:

* add replacement for `csinfo`, `cslist` and `revpanel`
* rewrite `repomodel`
...

unresolved issues:

* How to handle unapplied MQ patches?
* Hyperlinks
* Stylesheets
* Cache
...

## Issues to Consider

TODO

annotate:

* \#4284 - KeyError on annotate

repomodel:

* \#532 - Long operations block UI (Qt)
* \#580 - Sorting order in workbench
* \#1991 - Highlight parents of merge
* \#2038 - Local Time log column should be MY local time
* \#2210 - Workbench, reversion list get slow when showing 'Changes' column
  for large repo
* \#4033 - graph: highlight ancestors of selected revision
* \#4099 - Updated dynamic tags not updating in GUI (hg-git)
* \#4259 - Latest tag distance in revision history

revdetails/revpanel:

* \#1035 - thg file log full changeset description
* \#1382 - Log message viewer needs word-wrap
* \#1620 - Issue Tracking Named Branch?
* \#2642 - Show graft/transplant branch in changeset details panel

## See Also

* [Qt: Supported HTML Subset](http://qt-project.org/doc/qt-4.8/richtext-html-subset.html)
