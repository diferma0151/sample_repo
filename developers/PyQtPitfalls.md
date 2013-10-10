# Bugs and Pitfalls of PyQt

[TOC]

## Signals & Slots

### QTimer.singleShot cannot be connected to signal by new-style connection

*In PyQt < 4.7.5, Issue #2239*

~~~~{.py}
filled = pyqtSignal()
QTimer.singleShot(0, self.filled)
~~~~

Workaround: use old-style connection

~~~~{.py}
QTimer.singleShot(0, self, SIGNAL('filled()'))
~~~~

### Some signals having QModelIndex argument cannot be connected to decorated slots

*In PyQt 4.7.4, Changeset f9693f2f5807, ...*

~~~~{.py}
selectionModel().currentChanged.connect(self.onCurrentChanged)

@pyqtSlot(QModelIndex)
def onCurrentChanged(self, index):
    ...
~~~~

Workarounds:

* comment out `@pyqtSlot()`
* remove `index` parameter (UNTESTED)

See http://thread.gmane.org/gmane.comp.python.pyqt-pykde/19836

### sender() returns wrong object if decorated as pyqtSlot

*In PyQt < 4.7.4, Issue #2170*

~~~~{.py}
@pyqtSlot(unicode)
def _updateRepoTabTitle(self, title):
    # self.sender() will return None or unrelated object because of @pyqtSlot decorator
    index = self.repoTabsWidget.indexOf(self.sender())
    self.repoTabsWidget.setTabText(index, title)
~~~~

Workarounds:

* comment out `@pyqtSlot`
* use [QSignalMapper](http://qt-project.org/doc/qt-4.8/qsignalmapper.html)

See http://www.riverbankcomputing.com/pipermail/pyqt/2010-August/027557.html

### sender() returns wrong object if initiated by pyqtSlot-decorated slot of the same object

*In all versions of PyQt, Issue #3320, #3393*

~~~~{.py}
@pyqtSlot()
def editSettings(self):
    dlg = SettingsDialog()
    dlg.exec_()  # blocking

#@pyqtSlot(unicode)
def _updateRepoTabTitle(self, title):
    # self.sender() will return unrelated object during `editSettings()`
    ...
~~~~

Workarounds:

* Mark/unmark both slots as `@pyqtSlot`
* use [QSignalMapper](http://qt-project.org/doc/qt-4.8/qsignalmapper.html)

### New-style connection to QSignalMapper.map silently ignored

*In PyQt 4.6, 4.7.x?*

~~~~{.py}
# not work because connection made via proxy object
rw.titleChanged.connect(mapper.map)
~~~~

Workaround: use old-style connection

~~~~{.py}
QObject.connect(rw, SIGNAL('titleChanged(QString)'),
                mapper, SLOT('map()'))
~~~~

See http://www.riverbankcomputing.com/pipermail/pyqt/2010-March/026113.html


## QString

### Equality check fails if left-hand side is non-ascii unicode

*Changeset 5719b55f4f26, ...*

~~~~{.py}
>>> QString(u'é') == u'é'
True
>>> u'é' == QString(u'é')
UnicodeWarning: Unicode equal comparison failed to convert both arguments to Unicode
- interpreting them as being unequal
False
~~~~

Workaround: cast to `unicode` ASAP

### Incompatible with unicode in dict and set key because of different hash value

*Issue #1883, ...*

~~~~{.py}
>>> 'foo' in {QString('foo'): 0}
False
~~~~

Workaround: cast to `unicode` ASAP