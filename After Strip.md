    #!python
    ** Mercurial version (2.5.2).  TortoiseHg version (2.7.1+10-9d9e943058c8)
    ** Command: 
    ** CWD: /home/gdx
    ** Encoding: UTF-8
    ** Extensions loaded: extdiff, graphlog, hgk, inotify, purge, record, transplant, rebase, histedit, interhg, mq, highlight
    ** Python version: 2.7.3 (default, Dec 22 2012, 21:14:12) [GCC 4.7.2]
    ** System: Linux gdx-nb 3.8.2-1-ARCH #1 SMP PREEMPT Mon Mar 4 09:06:43 CET 2013 x86_64
    ** Qt-4.8.4 PyQt-4.10 QScintilla-2.7.1
    Traceback (most recent call last):
      File "/usr/lib/python2.7/site-packages/tortoisehg/hgqt/workbench.py", line 739, in repoTabChanged
        self.updateMenu()
      File "/usr/lib/python2.7/site-packages/tortoisehg/hgqt/workbench.py", line 647, in updateMenu
        self._updateWindowTitle()
      File "/usr/lib/python2.7/site-packages/tortoisehg/hgqt/workbench.py", line 658, in _updateWindowTitle
        self.setWindowTitle(_('%s - TortoiseHg Workbench') % w.title())
      File "/usr/lib/python2.7/site-packages/tortoisehg/hgqt/repowidget.py", line 274, in title
        elif self.repomodel.branch():
      File "/usr/lib/python2.7/site-packages/tortoisehg/hgqt/repomodel.py", line 222, in branch
        return self.filterbranch
    AttributeError: 'HgRepoListModel' object has no attribute 'filterbranch'
    