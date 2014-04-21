# Encoding Strategy

## Overview

* All Mercurial inputs and outputs are in local encoding
* All QStrings are in unicode
* All strings passed to Qt are converted to a QString (in API 1)
* All strings returned by Qt APIs are QString (in API 1)
* All strings emitted through signals are converted to QString (in API 1)
* Our gettext wrapper _() returns unicode strings
* Configuration files are assumed to be in local encoding by Mercurial
* Repository contents may be saved in arbitrary encoding

## API

* `hglib.tounicode()` converts local string to unicode (Mercurial to Qt)
* `hglib.fromunicode()` converts unicode or QString to local string (Qt to Mercurial)
* Built-in `unicode()` can be used to convert QString to unicode
* `fileencoding.guessencoding()` can be used to select possible encoding for
  file

## Known issues

command:

TODO

file content:

* \#3596 - Missing handling of content encoding in FileDiffDialog
* \#3715 - MQ: Edit patch reject: "Could not translate the file content from
  native encoding."

## See also

* http://mercurial.selenic.com/wiki/EncodingStrategy
* http://pyqt.sourceforge.net/Docs/PyQt4/incompatible_apis.html
