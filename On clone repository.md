When I clone repository A to repo B throw error!
But my repo A have problems: some files "in manifests not found" (after check %hg verify) because those files grow old and I removed them.
So here source error:


```
#!python

{{{
#!python
** Mercurial version (2.2.3).  TortoiseHg version (2.4.2)
** Command: 
** CWD: D:\PROGRAMMING\[...]\Repo
** Encoding: cp1251
** Extensions loaded: 
** Python version: 2.7.3 (default, Apr 10 2012, 23:24:47) [MSC v.1500 64 bit (AMD64)]
** Windows version: sys.getwindowsversion(major=6, minor=1, build=7601, platform=2, service_pack='Service Pack 1')
** Processor architecture: x64
** Qt-4.8.0 PyQt-4.9.1
Traceback (most recent call last):
  File "tortoisehg\hgqt\repotreemodel.pyo", line 366, in updateCommonPaths
  File "tortoisehg\hgqt\repotreeitem.pyo", line 507, in updateCommonPath
  File "genericpath.pyo", line 72, in commonprefix
UnicodeDecodeError: 'ascii' codec can't decode byte 0xcc in position 15: ordinal not in range(128)

}}}
```