Installed THG last week. Everything seemed to be OK. Was able to clone repo, and create and check-in files as well as push them.

Today THG started behaving badly, and eventually I located the instruction to execute the thg binary at the command line. It caused a dialog box with the following content to be generated, along with directions to report it to the bug tracker.


    #!python
    ** Mercurial version (2.6.2).  TortoiseHg version (2.8.1)
    ** Command: 
    ** CWD: C:\unifiedPlatform\up
    ** Encoding: cp1252
    ** Extensions loaded: mercurial_keyring, rebase, extdiff
    ** Python version: 2.7.3 (default, Apr 10 2012, 23:24:47) [MSC v.1500 64 bit (AMD64)]
    ** Windows version: sys.getwindowsversion(major=6, minor=1, build=7601, platform=2, service_pack='Service Pack 1')
    ** Processor architecture: x64
    ** Qt-4.8.4 PyQt-4.9.6 QScintilla-2.7
    Traceback (most recent call last):
      File "tortoisehg\hgqt\run.pyo", line 558, in __call__
      File "tortoisehg\hgqt\workbench.pyo", line 1324, in run
      File "tortoisehg\hgqt\workbench.pyo", line 654, in showRepo
      File "tortoisehg\hgqt\workbench.pyo", line 1001, in _openRepo
      File "tortoisehg\hgqt\workbench.pyo", line 823, in addRepoTab
      File "tortoisehg\hgqt\repowidget.pyo", line 116, in __init__
      File "tortoisehg\hgqt\repowidget.pyo", line 167, in setupUi
      File "tortoisehg\hgqt\repofilter.pyo", line 172, in __init__
      File "tortoisehg\hgqt\repofilter.pyo", line 440, in refresh
      File "tortoisehg\hgqt\repofilter.pyo", line 388, in _updateBranchFilter
      File "mercurial\util.pyo", line 277, in __get__
      File "tortoisehg\hgqt\thgrepo.pyo", line 449, in namedbranches
      File "mercurial\localrepo.pyo", line 659, in branchtags
      File "mercurial\localrepo.pyo", line 636, in branchmap
      File "mercurial\branchmap.pyo", line 75, in updatecache
      File "mercurial\localrepo.pyo", line 636, in branchmap
      File "mercurial\branchmap.pyo", line 75, in updatecache
      File "mercurial\localrepo.pyo", line 636, in branchmap
      File "mercurial\branchmap.pyo", line 62, in updatecache
      File "mercurial\repoview.pyo", line 169, in changelog
      File "mercurial\repoview.pyo", line 116, in filterrevs
      File "mercurial\repoview.pyo", line 48, in computeunserved
      File "mercurial\phases.pyo", line 408, in hassecret
      File "mercurial\localrepo.pyo", line 27, in __get__
      File "mercurial\scmutil.pyo", line 885, in __get__
      File "mercurial\localrepo.pyo", line 344, in _phasecache
      File "mercurial\phases.pyo", line 147, in __init__
      File "mercurial\phases.pyo", line 130, in _readroots
    ValueError: need more than 1 value to unpack
       