# Repository Access and Command Execution (Planned)

**This feature is NOT implemented yet.**

## Overview

* introduces a wrapper to repository object, called RepoAgent

~~~~
    +-------------+
    |  Widget(s)  |
    +-----+-------+
  +-------+--------------------------------+
  |       |use                             |
  | +-----v-------+                        |use
  | |  RepoAgent  |                        |
  | +-------------+                        |
  |   (aka ThgRepoWrapper)                 |
  |       +-------------------+------------|----+
  |use    |own                |own         |    |create (per Widget)
  | +-----v------+     +------v------+  +--v----v-------+
  +-> repository |     | RepoWatcher |  | CommandRunner |
    +------------+     +-------------+  +---------------+
                                          (like cmdui.Core)
~~~~

* RepoAgent owns repository object instead of extending it to thgrepository.
* RepoAgent will receive all command requests and run them in sequence.
  It may use cmdserver process to address thread issues [1].
* While running commands, RepoAgent disables RepoWatcher. This will resolve
  temporary LookupError while rebasing, qrefresh, etc.

## TODOs

short-term:

1. eliminate backward dependencies to `run` module from widgets (IN PROGRESS)
1. redesign command API so that it can receive per-repository object
1. implement stubs for new classes
1. have Workbench use new classes (manager layer?)
1. pass stubs to all widgets

long-term:

* implement per-repository command queue
* drop `thgrepository` extension
* use command server via `QProcess`
* better control of `refreshWctx()`
* ...

## Issues to Consider

file-system monitoring:

* #812, #1469 - RevlogError on strip, rebase, collapse, etc.
* #1594, #2604 - error after qpop
* #1783 - thg keeps a ref to a folderrepo for too long ?
* #2470 - hg push in command line (to SVN with hgsubversion)

repo/ui object:

* #588 - Unified diff view doesn't work well with EOL extension
  → [discussion](http://thread.gmane.org/gmane.comp.version-control.mercurial.tortoisehg.user/3341/focus=3345)
* #2208 - Support --config argument for 'thg workbench'

threading:

* #2071 - crash when pushing with mercurial_keyring
* #1661 - "abort: Interrupted system" call during push with subrepos
* 074fd0fde0b6 - worker.py of Mercurial 2.6 only works in main thread

## See Also

* [[PATCH 0 of 7 RFC] rough idea to address instability of async command execution and polling](https://groups.google.com/d/msg/thg-dev/r2cWqYDg4iQ/JVg12dP1O1AJ)