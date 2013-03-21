    #!python
    ** Mercurial version (2.5.2).  TortoiseHg version (2.7.1)
    ** Command: 
    ** CWD: C:\Program Files\TortoiseHg
    ** Encoding: cp1251
    ** Extensions loaded: transplant, hggit, rebase, hgsubversion, mq, convert, histedit, extdiff
    ** Python version: 2.7.3 (default, Apr 10 2012, 23:24:47) [MSC v.1500 64 bit (AMD64)]
    ** Windows version: sys.getwindowsversion(major=6, minor=2, build=9200, platform=2, service_pack='')
    ** Processor architecture: x64
    ** Qt-4.8.4 PyQt-4.9.6 QScintilla-2.7
    Traceback (most recent call last):
      File "tortoisehg\hgqt\commit.pyo", line 577, in repositoryChanged
      File "tortoisehg\hgqt\commit.pyo", line 622, in refresh
      File "tortoisehg\hgqt\csinfo.pyo", line 426, in update
      File "tortoisehg\hgqt\csinfo.pyo", line 339, in get_markup
      File "tortoisehg\hgqt\csinfo.pyo", line 285, in get_markup
      File "tortoisehg\hgqt\csinfo.pyo", line 225, in get_data
      File "tortoisehg\hgqt\revpanel.pyo", line 59, in data_func
      File "mercurial\context.pyo", line 199, in branch
      File "mercurial\util.pyo", line 246, in __get__
      File "mercurial\context.pyo", line 146, in _changeset
      File "mercurial\changelog.pyo", line 282, in read
      File "mercurial\revlog.pyo", line 931, in revision
      File "mercurial\revlog.pyo", line 940, in _checkhash
    RevlogError: integrity check failed on 00changelog.i:630
    