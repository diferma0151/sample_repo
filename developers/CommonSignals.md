# Common Qt Signals

This page describes Qt signals commonly used in our code briefly.

## Repository

* `configChanged()` - user/repository config changed
* `repositoryChanged()` - repository metadata cahnged
* `repositoryDestroyed()` - repository has been removed from file-system
* `workingDirectoryChanged()` - parent revision of working directory changed
* `workingBranchChanged()` - current named branch changed

## Widgets

* `finished(result)` - the window is closed (implemented in `QDialog`)
* `grepRequested(pattern, {all, rev})` - forward grep request
* `linkActivated(url)` - forward arbitrary request (see "Internal URLs")

### Command Execution

The following signals may be deprecated or moved by
[redesign of repository access API](RepositoryAccess).

* `output(message, label)` - forward Mercurial `ui.write` or `ui.write_err`
  message
* `makeLogVisible(visible)` - request to change visibility of "Output Log"
* `beginSuppressPrompt()`, `endSuppressPrompt()` - suppress prompt line of
  "Output Log" dock
* `progress(topic, pos, item, unit, total)` - forward Mercurial
  `ui.progress` message

### Internal URLs

* `cset:rev` - go to the specified revision
* `repo:path?rev` - open the specified repository
* `shelve:` - open shelve dialog
