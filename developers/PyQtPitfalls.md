# Bugs and Pitfalls of PyQt4

[TOC]

----

## Common

### Qt functions don't accept keyword arguments

*In PyQt 4.6, Changeset 8d357e3ae4d8, ...*

BAD:
~~~~{.py}
filename = QFileDialog.getOpenFileName(parent=self, caption=...)
~~~~

GOOD:
~~~~{.py}
filename = QFileDialog.getOpenFileName(self, caption, ...)
~~~~

See http://pyqt.sourceforge.net/Docs/PyQt4/keyword_arguments.html

----

## Signals & Slots

### lambda slot cannot be disconnected even if underlying QObject is destoryed

*Issue #1267, #2386, ..., Changeset 1f16cc87f8a9, ...*

BAD:
~~~~{.py}
def __init__(self):
    def applyFilter():
        self.le.text()
        ...
    self.le.textEdited.connect(applyFilter)
    # made reference cycle. self will be half-dead if deleted by Qt object tree
~~~~

GOOD:
~~~~{.py}
def __init__(self):
    self.le.textEdited.connect(self._applyFilter)

@pyqtSlot()
def _applyFilter(self):
    self.le.text()
    ...
~~~~


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

*In PyQt < 4.7.4, Issue #2170, Changeset d95dff50c7f2, ...*

~~~~{.py}
@pyqtSlot(unicode)
def _updateRepoTabTitle(self, title):
    # self.sender() will return None or unrelated object because of @pyqtSlot decorator
    index = self.repoTabsWidget.indexOf(self.sender())
    self.repoTabsWidget.setTabText(index, title)
~~~~

Workarounds:

* ~~comment out `@pyqtSlot`~~ use `@qtlib.senderSafeSlot@`
* use [QSignalMapper](http://qt-project.org/doc/qt-4.8/qsignalmapper.html)

See http://www.riverbankcomputing.com/pipermail/pyqt/2010-August/027557.html

### sender() returns wrong object if initiated by decorated slot of the same object

*In all versions of PyQt, Issue #3320, #3393*

~~~~{.py}
@pyqtSlot()
def editSettings(self):
    dlg = SettingsDialog()
    dlg.exec_()  # blocking

#@pyqtSlot(unicode)
def _updateRepoTabTitle(self, title):
    # self.sender() will return the sender of editSettings() during editSettings()
    ...
~~~~

Workarounds:

* Mark/unmark both slots as `@pyqtSlot`
* use [QSignalMapper](http://qt-project.org/doc/qt-4.8/qsignalmapper.html)

### New-style connection may fail to resolve the best matching slot

*In PyQt 4.6, Changeset 97d063521574*

~~~~{.py}
@pyqtSlot()
def popPatch(self, patch=None):
    return self._runPop(patch)

# fail to find SLOT(popPatch()) and connect triggered(checked) to
# popPatch(patch) via proxy object
qpopAct.triggered.connect(self.patchActions.popPatch)
~~~~

Workaround: specify signal type explicitly
~~~~{.py}
qpopAct.triggered[()].connect(self.patchActions.popPatch)
~~~~

or use old-style connection

See http://www.riverbankcomputing.com/pipermail/pyqt/2010-March/026113.html

### New-style connection to QSignalMapper.map silently ignored

*In PyQt 4.6*

~~~~{.py}
# not work because connection made via proxy object
rw.titleChanged.connect(mapper.map)
~~~~

Workaround: use old-style connection
~~~~{.py}
QObject.connect(rw, SIGNAL('titleChanged(QString)'),
                mapper, SLOT('map()'))
~~~~

or specify signal type explicitly if available

See http://www.riverbankcomputing.com/pipermail/pyqt/2010-March/026113.html

----

## QString

*NOTE: QString is removed in PyQt5 an PySide.*

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

Don't use `QString`. Cast to `unicode` ASAP.

### Incompatible with unicode in dict and set key because of different hash value

*Issue #1883, ...*

~~~~{.py}
>>> 'foo' in {QString('foo'): 0}
False
~~~~

Don't use `QString`. Cast to `unicode` ASAP.

----

## Item Views

### Cannot access to QStyleOptionViewItemV<n> attributes from initStyleOption

*In PyQt < 4.7.1*

No workaround.  `sip.cast` will cause segfault.

> v4.7.1 17th March 2010
>   - A QStyleOption instance will be wrapped as a specific sub-class where
>     possible.

See http://www.riverbankcomputing.com/pipermail/pyqt/2010-March/026021.html

----

## Widgets

### Editable QComboBox does not allow case-sensitive input

*Issue #165, #2276*

> By default, for an editable combo box, a QCompleter that performs case
> insensitive inline completion is automatically created.
> -- [QComboBox::setCompleter](http://qt-project.org/doc/qt-4.8/qcombobox.html#setCompleter)

Workaround: use `qtlib.allowCaseChangingInput(combo)`

### QCompleter(parent) may lead to random crash due to ownership bug

*In PyQt < 4.11.4, Changeset 36b8e6315d05*

~~~~{.py}
completer = QCompleter(self)  # set wrong ownership
~~~~

Workaround: use `QCompleter(model, parent)`
~~~~{.py}
completer = QCompleter(None, self)
~~~~

See http://thread.gmane.org/gmane.comp.python.pyqt-pykde/27768/focus=27769

### Dialog that is resizable only in horizontal direction

*Changeset 709bcfb9c860*

1. set appropriate size policy to child widgets
2. restrict the size of the top-level layout by `setSizeConstraint(QLayout.SetMinAndMaxSize)`

As of Qt 4.8, `QFormLayout` needs to be wrapped by `QWidget` because `QFormLayout` does not calculate the `maximumSize()`.

See https://bugreports.qt-project.org/browse/QTBUG-17730

### Status tip of context menu is not propagated without QWidget-type parent

*Changeset ffc5458e92e8*

Unfortunately, `QStatusTipEvent` is not sent to the parent of the `QMenu`,
but to the parent of `QAction`, if populated in a popup menu.
And unhandled `QStatusTipEvent` is not propagated to the parent if it isn't a widget type.

Workaround: own `QAction` by `QWidget`

See https://bugreports.qt-project.org/browse/QTBUG-16114

### QWIDGETSIZE_MAX is not defined

Workaround: use `qtlib.QWIDGETSIZE_MAX`

See http://thread.gmane.org/gmane.comp.python.pyqt-pykde/28353/focus=28357
