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

## Issues to Consider

file-system monitoring:

* #1469 - integrity check failed (probably) on strip or rebase
* #2470 - hg push in command line (to SVN with hgsubversion)

repo object:

* #588 - Unified diff view doesn't work well with EOL extension
  → [discussion](http://thread.gmane.org/gmane.comp.version-control.mercurial.tortoisehg.user/3341/focus=3345)

threading:

* #2071 - crash when pushing with mercurial_keyring
* #1661 - "abort: Interrupted system" call during push with subrepos
* 074fd0fde0b6 - worker.py of Mercurial 2.6 only works in main thread

## See Also

* [[PATCH 0 of 7 RFC] rough idea to address instability of async command execution and polling](https://groups.google.com/d/msg/thg-dev/r2cWqYDg4iQ/JVg12dP1O1AJ)