this was created by user ed@jerisinger.com
{{{
#!python
** Mercurial version (2.0.2).  TortoiseHg version (2.2)
** Command: log
** CWD: /home/ed
** Encoding: UTF-8
** Extensions loaded: 
** Python version: 2.7.3 (default, Aug  1 2012, 05:16:07) [GCC 4.6.3]
** Qt-4.8.1 PyQt-4.9.1
Traceback (most recent call last):
  File "/usr/lib/python2.7/dist-packages/tortoisehg/hgqt/settings.py", line 941, in accept
    self.applyChanges()
  File "/usr/lib/python2.7/dist-packages/tortoisehg/hgqt/settings.py", line 919, in applyChanges
    self.conftabs.widget(i).applyChanges()
  File "/usr/lib/python2.7/dist-packages/tortoisehg/hgqt/settings.py", line 1269, in applyChanges
    wconfig.writefile(self.ini, self.fn)
  File "/usr/lib/python2.7/dist-packages/tortoisehg/util/wconfig.py", line 240, in writefile
    config.write(buf)
  File "/usr/lib/python2.7/dist-packages/tortoisehg/util/wconfig.py", line 171, in write
    ini = self._readini()
  File "/usr/lib/python2.7/dist-packages/tortoisehg/util/wconfig.py", line 199, in _readini
    return newini(fp)
  File "/usr/lib/python2.7/dist-packages/tortoisehg/util/wconfig.py", line 182, in newini
    return INIConfig(fp=fp, optionxformvalue=None)
  File "/usr/lib/python2.7/dist-packages/iniparse/ini.py", line 471, in __init__
    self._readfp(fp)
  File "/usr/lib/python2.7/dist-packages/iniparse/ini.py", line 565, in _readfp
    raise MissingSectionHeaderError(fname, linecount, line)
MissingSectionHeaderError: File contains no section headers.
file: /home/ed/Documents/TortoiseTest/.hg/hgrc, line: 1
'default = https://RisingerII@bitbucket.org/RisingerII/paprx-forms\n'

}}}

