= Thg 3.3.3 and earlier =

In all versions up to and including THG 3.3.3, the python 2.7 SWIG bindings for subversion were packaged inside the TortoiseHg Windows installers.  Users could enable the convert extension or hgsubversion extensions and they would just work because a full copy of subversion and its bindings were installed together with TortoiseHg.

= Thg 3.4 and later =

Beginning with release 3.4 of TortoiseHg, the subversion libraries and the Python 2.7 SWIG bindings for them have been removed from the TortoiseHg packages.  This was done primarily because of security problems in the subversion DLLs that we as TortoiseHg maintainers have no control over, but also to avoid having to package a second complete revision control system (svn) in every copy of TortoiseHg (and the major headaches these bindings have become).

Windows builds of the Python 2.7 SWIG bindings for subversion are not easy to come by. 32-bit builds for relatively recent versions of subversion can be found at http://alagazam.net/.  64-bit builds are much less common (if you know of any, please add a link here. Anyone can edit this page).

We offer for download the python SWIG bindings that we were previously including in our binary installers, they are available here:

* https://bitbucket.org/tortoisehg/thg-winbuild/downloads/svn_1.7.5_py27_x86.zip
* https://bitbucket.org/tortoisehg/thg-winbuild/downloads/svn_1.7.5_py27_x64.zip

If you have a 64bit Operating System, and thus installed the 64bit version of TortoiseHg, then you need the 64bit subversion bindings in svn_1.7.5_py27_x64.zip. Otherwise you need the x86 version.  If you can find functional python bindings for more recent versions of subversion, then great (please add a link to them here).

Each zip file contains two folders, libsvn/ and svn/. These two folders are typically copied into your natively installed Python site-packages folder, but since TortoiseHg is packaged as a 'frozen' Python environment there is no site-packages folder but there is a simple workaround for this. One can use a small Mercurial extension to add any arbitrary folder into the Python system path.

The complete setup steps are as follows:

1. download python bindings for subversion, using one of the two links above or from some other location, and  uncompress them into a folder on your machine. For this example, let's assume you have uncompressed them into C:\stuff and now you have C:\stuff\libsvn and C:\stuff\svn folders full of python code and subversion DLLs.

2. next, download https://bitbucket.org/tortoisehg/thg-winbuild/downloads/insertpath.py (a small text file) and copy that into the same root folder, in this example it would result in C:\stuff\insertpath.py

3. last, edit your Mercurial.ini file (in %HOME% or %USERPROFILE% folder, generally C:\Users\yourname\mercurial.ini) and enable this extension by adding two lines:


```
#!python

[extensions]
svnbindings = C:\stuff\insertpath.py

```

If your mercurial.ini file already has an [extensions] section, you can simply add this new extension to that section (order is unimportant).

Note that this is a one-time setup for your computer. Upgrading TortoiseHg will not affect Mercurial.ini or these Python bindings. If you uninstall TortoiseHg you will probably also want to remove these subversion bindings.