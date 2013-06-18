{{{
#!python
** Mercurial version (2.0.2).  TortoiseHg version (2.2.2)
** Command: --nofork commit --listfile C:\Users\Annie\AppData\Local\Temp\THGD720.tmp
** CWD: C:\docs\Prolog\evil\evil
** Encoding: cp1252
** Extensions loaded: 
** Python version: 2.6.6 (r266:84297, Aug 24 2010, 18:13:38) [MSC v.1500 64 bit (AMD64)]
** Windows version: (6, 1, 7601, 2, 'Service Pack 1')
** Processor architecture: x64
** Qt-4.7.4 PyQt-4.8.6
Traceback (most recent call last):
  File "tortoisehg\hgqt\thread.pyo", line 264, in run
TypeError: sequence item 6: expected string, QString found

}}}

occured when mucking about with a broken makefile making a Prolog project with a file named ())).pl


(yes, that's a valid module name in Prolog.)

If you're considering becoming Amish after looking at this, now's a good time, they're about to harvest.

Moo!

===========

ok - cleaned up my local repo by stashing the change, removing the repo, cloning and reapplying change.

new commit also had problem - turns out the issue is that my commit message was ☺   (the unicode smiley)

