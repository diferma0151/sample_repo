# Repository Access and Command Execution

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

### Capturing Output

By default, all command outputs are propagated through `outputReceived` signal.
This is very inefficient for transferring large chunks.

There are two alternative APIs:

* `sess.setCaptureOutput(True)` enables in-memory output buffer.  All data
  outputs (excluding ui messages) are buffered by `CmdSession` and can later
  be read by `QIODevice`-like interface.
* `sess.setOutputDevice(device)` redirects all data outputs to the specified
  `QIODevice`.  It can be used to write outputs to `QFile`.

### Executors

**CmdThread** (default < 3.1)

* lightweight and feature-rich
* cannot abort on I/O stall, #1507
* has several thread issues like #1661 or #2071

**CmdProc**

* high overhead (especially on Windows)
* ~~cannot abort *safely* (`kill -KILL` or `TerminateProcess` is used)~~
  (possible by win32ill extension)
* cannot terminate if `hg` is a batch file
* no interactive prompt
* ~~no progress and `ui.label`~~

**CmdServer** (default in 3.1)

* need to manage server process
* http://mercurial.selenic.com/wiki/CommandServer#Known_issues


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
* \#1571 - disable filesystem monitor while command running

In 2.10:

* new per-repository command runner which manages busy state and command queue
* `cmdui.Runner` and `cmdui.Dialog` has been replaced by the new command runner
* run web server in separate process

In 2.11:

* replace `cmdui.Widget` by new command runner
* remove manual busy counting, `increment/decrementBusyCount()`
* almost all widgets receive `RepoAgent` in place of raw `repo`
* move shortname and displayname to `RepoAgent`
* pass `ui` to `cmdcore.CmdAgent` so that it can read user configuration
* \#2208 - Support --config argument for 'thg workbench'
* obtain `RepoAgent` of subrepo through main's

In 3.0:

* extract reusable `QtUi` from thread.py
* add experimental command server client which uses `QProcess`

In 3.0.2 (unreleased):

* \#588 - Unified diff view doesn't work well with EOL extension
  → [discussion](http://thread.gmane.org/gmane.comp.version-control.mercurial.tortoisehg.user/3341/focus=3345)

In 3.1 (unreleased):

* missing `ui.error` label in command server
* \#3343 - Blackbox extension not working when using hg from within TortoiseHg
* \#1661 - "abort: Interrupted system" call during push with subrepos
* \#2071 - crash when pushing with mercurial_keyring
* \#2614 - `util.hgexecutable()` returns wrong path
* 074fd0fde0b6 - worker.py of Mercurial 2.6 only works in main thread
* \#3370 - Workbench UI getting dispresponsive on certain operations
* \#484 - Can't update to any revision (SVN subrepo)
* \#788 - Can't use git repo
* \#3637 - Redirect hook stdout, stderr to Output log
* \#3670 - WindowsError 6 in pipe decode/encode filter
* switch bundlerepo (and unionrepo) globally by RepoAgent
* manage `--hidden` flag globally by RepoAgent
* \#3735 - obosoleted changeset appears as "Child:" field unexpectedly
* add API to capture raw command output without emitting `outputReceived()`
  (for email preview, revset query, export to clipboard, etc.)
* replace threaded revset query
* reimplement wctxactions by using command APIs (WIP)
    * \#1347 - Add support for pre-<command> and post-<command> hooks
    * \#2217 - Adding files to source control in the separate thread
    * \#1953 - largefiles treated wrong when added by tortoise contextmenu-entry "add large..."

### TODOs

short-term (in 3.2):

1. pool `CmdAgent` instances for clone/init diag
   (and replace service management in `RepoManager`)
1. remove `CmdThread`
1. redesign `ui.error` and `dispatch` wrapper
1. add API to connect `QIODevice` to "I" channel of command server
   (in order to replace use of temporary files)
1. factor out change notification signals

long-term:

* fetch log and calculate revision graph asynchronously
* replace `visdiff.snapshot()` by `hg archive` ?
* replace `StatusThread`
* make `ManifestModel` fetch status asynchronously
* remove global cache of `thgrepository` instances
* keep dirstate/wctx up-to-date by RepoAgent?
* better control of `refreshWctx()`
* provide read/write config API
* reload root ui object when global setting is changed?
* how to call workbench from sub dialogs?
* drop `thgrepository` extension
* ...

## Issues to Consider

file-system monitoring:

* \#812, #1305, #1426, #1469, #1758, #2565 - RevlogError on strip,
  rebase, collapse, etc.
* \#1594, #2604 - error after qpop
* \#2470 - hg push in command line (to SVN with hgsubversion)
* \#3697 - No workbench refresh when first commit is rolled back

hidden, union repo:

* \#2552 - compare branch from different repos

thread/process:

* \#1507 - Stop button for pull that hangs
* \#3381 - Locked folders/files on Windows (again)

direct command call:

* InitDialog calls `hg.repository(..., create=1)`

graph, status updates:

* \#532 - Long operations block UI (Qt)

workingctx:

* \#3621 - Shelve tool does not always know about added/removed files

child to workbench:

* \#2354 - "Folder History" in revision browser does nothing

unicode:

* \#3246 - better handling of UnicodeEncodeError of command-line parameters

## See Also

* [[PATCH 0 of 7 RFC] rough idea to address instability of async command execution and polling](https://groups.google.com/d/msg/thg-dev/r2cWqYDg4iQ/JVg12dP1O1AJ)
