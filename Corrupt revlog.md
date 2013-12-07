    #!python
    ** Mercurial version (2.8).  TortoiseHg version (2.10)
    ** Command: --nofork update
    ** CWD: C:\Users\Александр\Documents\Visual Studio 2013\Projects\Awtl.Telemetry
    ** Encoding: cp1251
    ** Extensions loaded: rebase, eol
    ** Python version: 2.7.3 (default, Apr 10 2012, 23:24:47) [MSC v.1500 64 bit (AMD64)]
    ** Windows version: sys.getwindowsversion(major=6, minor=2, build=9200, platform=2, service_pack='')
    ** Processor architecture: x64
    ** Qt-4.8.4 PyQt-4.10.2 QScintilla-2.7.2
    Traceback (most recent call last):
      File "tortoisehg\hgqt\run.pyo", line 49, in dispatch
      File "tortoisehg\hgqt\run.pyo", line 226, in _runcatch
      File "tortoisehg\hgqt\run.pyo", line 306, in runcommand
      File "tortoisehg\hgqt\run.pyo", line 357, in _runcommand
      File "tortoisehg\hgqt\run.pyo", line 311, in checkargs
      File "tortoisehg\hgqt\run.pyo", line 305, in <lambda>
      File "tortoisehg\hgqt\qtapp.pyo", line 320, in __call__
      File "tortoisehg\hgqt\qtapp.pyo", line 360, in _createdialog
      File "tortoisehg\hgqt\thgrepo.pyo", line 467, in openRepoAgent
      File "tortoisehg\hgqt\thgrepo.pyo", line 57, in repository
      File "mercurial\hg.pyo", line 112, in repository
      File "mercurial\hg.pyo", line 107, in _peerorrepo
      File "hgext\eol.pyo", line 349, in reposetup
      File "hgext\eol.pyo", line 294, in _hgcleardirstate
      File "hgext\eol.pyo", line 287, in loadeol
      File "hgext\eol.pyo", line 212, in parseeol
      File "mercurial\localrepo.pyo", line 405, in __getitem__
      File "mercurial\context.pyo", line 233, in __init__
      File "mercurial\localrepo.pyo", line 27, in __get__
      File "mercurial\scmutil.pyo", line 956, in __get__
      File "mercurial\localrepo.pyo", line 375, in changelog
      File "mercurial\changelog.pyo", line 123, in __init__
      File "mercurial\revlog.pyo", line 253, in __init__
    RevlogError: index 00changelog.i is corrupted
    