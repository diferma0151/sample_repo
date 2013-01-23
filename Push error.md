When I tried to push my newly created repository to bitbucket using the synchronise button I received the following error:
 
traceback as below:

{{{
#!python
** Mercurial version (2.2.2).  TortoiseHg version (2.4.1)
** Command: --nofork workbench
** CWD: C:\Users\poweruser\Documents\Aptana Studio 3 Workspace\JDL\administrator\com_miijdl
** Encoding: cp1252
** Extensions loaded: 
** Python version: 2.6.6 (r266:84297, Aug 24 2010, 18:13:38) [MSC v.1500 64 bit (AMD64)]
** Windows version: (6, 1, 7601, 2, 'Service Pack 1')
** Processor architecture: x64
** Qt-4.7.4 PyQt-4.8.6
Traceback (most recent call last):
  File "tortoisehg\hgqt\sync.pyo", line 154, in <lambda>
  File "tortoisehg\hgqt\sync.pyo", line 1099, in pushclicked
  File "tortoisehg\hgqt\sync.pyo", line 774, in run
TypeError: unsupported operand type(s) for +: 'NoneType' and 'str'

}}}