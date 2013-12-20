# Repository Access and Command Execution

**This feature is UNDER DEVELOPMENT.**

[TOC]

## Overview

* `RepoManager` manages life-cycle of `RepoAgent` and its filesystem monitor.
* `RepoAgent` owns `repository` object instead of extending it to thgrepository.
* `RepoAgent` (and `CmdAgent`) will receive all command requests and run them
  in sequence.
* Provides consistent *unicode APIs*

![](fig/repository-classes.png)

## Command Execution

`RepoAgent` and its backend, `CmdAgent`, provide the same APIs to execute
Mercurial commands asynchronously.

`runCommand(cmdline)` or `runCommandSequence([cmdline, ...])` start or queue
the specified command and return new `CmdSession` object which will provide
notification signals.

Example:

~~~~{.py}
    def __init__(self, ...):
        ...
        self._cmdsession = cmdcore.nullCmdSession()

    def runUpdate(self):
        if not self._cmdsession.isFinished():
            return  # command still running
        cmdline = hglib.buildcmdargs('update', rev='.')
        self._cmdsession = sess = self._repoagent.runCommand(cmdline, self)
        sess.commandFinished.connect(self._onUpdateFinished)

    @pyqtSlot(int)
    def _onUpdateFinished(self, ret):
        if ret != 0:
            cmdui.errorMessageBox(self._cmdsession, self)
...
~~~~

The relation between `CmdAgent` and `CmdSession` is similar to the one
between `QNetworkAccessManager` and `QNetworkReply`.

### Executors

**CmdThread** (default)

* lightweight and feature-rich
* cannot abort on I/O stall, #1507
* has several thread issues like #1661 or #2071

**CmdProc**

* high overhead (especially on Windows)
* cannot abort *safely* (`kill -KILL` or `TerminateProcess` is used)
* cannot terminate if `hg` is a batch file
* no interactive prompt
* no progress and `ui.label`

**Command Server Client** (planned)

* *not implemented*

### GUI

* `CmdControlDialog` for one-shot command with configurable parameters  
  (e.g. "archive", "clone", "update")
* `CmdSessionDialog` for log window of started command  
  (e.g. "verify", hidden log window of "quickop")
* `CmdSessionControlWidget` to implement dialog which needs unusual handling  
  (e.g. "compress", "import", "rebase")
* bare `LogWidget` and `ThgStatusBar` for others  
  (e.g. "commit", "sync")

* TODO: add utility for tag-like dialogs?

## Change History

In 2.9:

* introduce `RepoManager` and stub for `RepoAgent`
* redesign command API of `tortoisehg.hgqt.run`
* \#1783 - thg keeps a ref to a folderrepo for too long ?

In 2.10:

* new per-repository command runner which manages busy state and command queue
* `cmdui.Runner` and `cmdui.Dialog` has been replaced by the new command runner
* run web server in separate process

In 2.11 (unreleased):

* replace `cmdui.Widget` by new command runner
* remove manual busy counting, `increment/decrementBusyCount()`
* almost all widgets receive `RepoAgent` in place of raw `repo`
* move shortname and displayname to `RepoAgent`
* pass `ui` to `cmdcore.CmdAgent` so that it can read user configuration
* \#2208 - Support --config argument for 'thg workbench'

### TODOs

short-term (in 2.11?):

1. extract reusable `QtUi` from thread.py
1. add experimental command server client which uses `QProcess`

long-term:

* keep dirstate/wctx up-to-date by RepoAgent?
* better control of `refreshWctx()`
* provide read/write config API
* bundlerepo and `--hidden` flag
* reload root ui object when global setting is changed?
* how to call workbench from sub dialogs?
* drop `thgrepository` extension
* ...

## Issues to Consider

file-system monitoring:

* \#812, #1305, #1426, #1469, #1571, #1758, #2565 - RevlogError on strip,
  rebase, collapse, etc.
* \#1594, #2604 - error after qpop
* \#2470 - hg push in command line (to SVN with hgsubversion)

repo/ui object:

* \#588 - Unified diff view doesn't work well with EOL extension
  → [discussion](http://thread.gmane.org/gmane.comp.version-control.mercurial.tortoisehg.user/3341/focus=3345)
* \#3343 - Blackbox extension not working when using hg from within TortoiseHg

thread/process:

* \#1507 - Stop button for pull that hangs
* \#1661 - "abort: Interrupted system" call during push with subrepos
* \#2071 - crash when pushing with mercurial_keyring
* \#2614 - `util.hgexecutable()` returns wrong path
* 074fd0fde0b6 - worker.py of Mercurial 2.6 only works in main thread
- \#3370 - Workbench UI getting dispresponsive on certain operations
- \#3381 - Locked folders/files on Windows (again)
* \#3533 - 'generator already executing' during Update / Pull

unicode:

* \#3246 - better handling of UnicodeEncodeError of command-line parameters

## See Also

* [[PATCH 0 of 7 RFC] rough idea to address instability of async command execution and polling](https://groups.google.com/d/msg/thg-dev/r2cWqYDg4iQ/JVg12dP1O1AJ)
