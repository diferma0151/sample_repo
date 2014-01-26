# Common Qt Signals

This page briefly describes Qt signals commonly used in our code.

## Repository

* `configChanged()` - user/repository config changed
* `repositoryChanged()` - repository metadata changed
* `repositoryDestroyed()` - repository has been removed from file-system
  (won't be emitted on Windows because open files cannot be removed)
* `workingDirectoryChanged()` - parent revision of working directory changed
* `workingBranchChanged()` - current named branch changed

## Widgets

* `grepRequested(pattern, {all, rev})` - forward grep request
* `linkActivated(url)` - forward arbitrary request (see "Internal URLs")
* `showMessage(msg)` - forward status message

### Command Execution

* `commandFinished(ret)` or `commandFinished(cmdsession)`
* `outputReceived(message, label)` - forward Mercurial `ui.write` or
  `ui.write_err` message
* `progressReceived(topic, pos, item, unit, total)` - forward Mercurial
  `ui.progress` message

See also [Repository Access and Command Execution](RepositoryAccess).

### Internal URLs

* `cset:rev` - go to the specified revision
* `repo:path?rev` - open the specified repository
* `shelve:` - open shelve dialog
