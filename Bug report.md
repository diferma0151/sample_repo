{{{
#!python
** Mercurial version (2.0.2).  TortoiseHg version (2.2)
** Command: log
** CWD: /home/chani
** Encoding: UTF-8
** Extensions loaded: 
** Python version: 2.7.3 (default, Aug  1 2012, 05:14:39) [GCC 4.6.3]
** Qt-4.8.1 PyQt-4.9.1
Traceback (most recent call last):
  File "/usr/lib/python2.7/dist-packages/tortoisehg/hgqt/workbench.py", line 767, in openRepository
    self._openRepo(hglib.fromunicode(path), False)
  File "/usr/lib/python2.7/dist-packages/tortoisehg/hgqt/workbench.py", line 777, in _openRepo
    self.addRepoTab(repo)
  File "/usr/lib/python2.7/dist-packages/tortoisehg/hgqt/workbench.py", line 650, in addRepoTab
    rw = RepoWidget(repo, self)
  File "/usr/lib/python2.7/dist-packages/tortoisehg/hgqt/repowidget.py", line 79, in __init__
    self.repolen = len(repo)
  File "/usr/lib/python2.7/dist-packages/mercurial/localrepo.py", line 218, in __len__
    return len(self.changelog)
  File "/usr/lib/python2.7/dist-packages/mercurial/scmutil.py", line 802, in __get__
    entry.obj = self.func(obj)
  File "/usr/lib/python2.7/dist-packages/mercurial/localrepo.py", line 176, in changelog
    c = changelog.changelog(self.sopener)
  File "/usr/lib/python2.7/dist-packages/mercurial/changelog.py", line 113, in __init__
    revlog.revlog.__init__(self, opener, "00changelog.i")
  File "/usr/lib/python2.7/dist-packages/mercurial/revlog.py", line 263, in __init__
    % (self.indexfile, fmt))
RevlogError: index 00changelog.i unknown format 2

}}}