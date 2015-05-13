# Subversion bindings for Python 2.7

## Thg 3.3.3 and earlier

In all versions up to and including THG 3.3.3, the python 2.7 SWIG bindings for subversion were packaged inside the TortoiseHg Windows installers.  Users could enable the convert extension or hgsubversion extensions and they would just work because a full copy of subversion and its bindings were installed together with TortoiseHg.

## Thg 3.4 and later

Beginning with release 3.4 of TortoiseHg, the subversion libraries and the Python 2.7 SWIG bindings for them have been removed from the TortoiseHg packages.  This was done primarily because of security problems in the subversion DLLs that we as TortoiseHg maintainers have no control over, but also to avoid having to package a second complete revision control system (svn) in every copy of TortoiseHg (and the major headaches these bindings have become).

This is inconsequential for most TortoiseHg users, the subversion bindings are only required when using 'hg convert' to convert a subversion repository, or using the hgsubversion extension to interact with a subversion server. If you do neither of these things, then you can just enjoy the 3MB smaller packages. If you do need either of those two features, continue reading.

We offer for download the python SWIG bindings that we were previously including in our binary installers, they are available here:

* https://bitbucket.org/tortoisehg/thg-winbuild/downloads/svn_1.7.5_py27_x86.zip
* https://bitbucket.org/tortoisehg/thg-winbuild/downloads/svn_1.7.5_py27_x64.zip

If you have a 64bit operating system, and thus installed the 64bit version of TortoiseHg, then you need the 64bit subversion bindings in svn_1.7.5_py27_x64.zip. Otherwise you need the x86 version. Each zip file contains two folders, libsvn/ and svn/.

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

Note that this is a one-time setup for your computer. Upgrading TortoiseHg will not affect Mercurial.ini or these Python bindings. If you uninstall TortoiseHg you will probably also want to remove these subversion bindings. Also note that this general method of inserting the subversion bindings into the frozen system path will also work for the Mercurial MSI packages, if you happen to use those instead of the TortoiseHg installers.

Also note that you do *not* have to use the subversion bindings that we were packaging, you can use any version of them that you like, but Windows builds of the Python 2.7 SWIG bindings for subversion are not easy to come by. 32-bit builds for relatively recent versions of subversion can be found at http://alagazam.net/.  64-bit builds are much less common (if you know of any, please add a link here. Anyone can edit this page).
