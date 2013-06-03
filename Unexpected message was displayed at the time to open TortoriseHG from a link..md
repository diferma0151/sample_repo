This is the log the the application displayed when I was opening TortoiseHG.

### Prerequisites: ###
* Have the TortoiseHG installed on Windows 7.
* Have the TortoiseHG Workbench pinned into the applications bar.


```
#!text
Log:
    #!python
    ** Mercurial version (2.5.4).  TortoiseHg version (2.7.2)
    ** Command: 
    ** CWD: C:\Windows\system32
    ** Encoding: cp1252
    ** Extensions loaded: 
    ** Python version: 2.7.3 (default, Apr 10 2012, 23:24:47) [MSC v.1500 64 bit (AMD64)]
    ** Windows version: sys.getwindowsversion(major=6, minor=1, build=7601, platform=2, service_pack='Service Pack 1')
    ** Processor architecture: x64
    ** Qt-4.8.4 PyQt-4.9.6 QScintilla-(unknown)
    Traceback (most recent call last):
      File "tortoisehg\hgqt\run.pyo", line 55, in dispatch
      File "tortoisehg\hgqt\run.pyo", line 248, in _runcatch
      File "tortoisehg\hgqt\run.pyo", line 324, in runcommand
      File "tortoisehg\hgqt\run.pyo", line 375, in _runcommand
      File "tortoisehg\hgqt\run.pyo", line 329, in checkargs
      File "tortoisehg\hgqt\run.pyo", line 323, in <lambda>
      File "mercurial\util.pyo", line 475, in check
      File "tortoisehg\hgqt\run.pyo", line 765, in log
      File "mercurial\demandimport.pyo", line 95, in _demandimport
      File "tortoisehg\hgqt\workbench.pyo", line 19, in <module>
      File "mercurial\demandimport.pyo", line 114, in _demandimport
      File "tortoisehg\hgqt\repowidget.pyo", line 29, in <module>
      File "mercurial\demandimport.pyo", line 114, in _demandimport
      File "tortoisehg\hgqt\revdetails.pyo", line 14, in <module>
      File "mercurial\demandimport.pyo", line 114, in _demandimport
      File "tortoisehg\hgqt\fileview.pyo", line 23, in <module>
      File "mercurial\demandimport.pyo", line 86, in __getattribute__
      File "mercurial\demandimport.pyo", line 58, in _load
      File "tortoisehg\hgqt\qscilib.pyo", line 19, in <module>
      File "mercurial\demandimport.pyo", line 95, in _demandimport
      File "PyQt4\Qsci.pyo", line 12, in <module>
      File "PyQt4\Qsci.pyo", line 10, in __load
    ImportError: DLL load failed: The specified procedure could not be found. 
```