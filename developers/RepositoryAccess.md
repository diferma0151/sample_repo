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

## Related Issues

* #2071 - crash when pushing with mercurial_keyring
* #1661 - "abort: Interrupted system" call during push with subrepos
* 074fd0fde0b6 - worker.py of Mercurial 2.6 only works in main thread

## See Also

* [[PATCH 0 of 7 RFC] rough idea to address instability of async command execution and polling](https://groups.google.com/d/msg/thg-dev/r2cWqYDg4iQ/JVg12dP1O1AJ)