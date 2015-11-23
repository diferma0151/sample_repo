# Bash commands #
## Process dependencies lookup ##

```
#!bash
$ ldd /path/to/program

```

To find out what libraries a particular executable depends on, you can use ldd command. This command invokes dynamic linker to find out library dependencies of an executable.
Note that it is NOT recommended to run ldd with any untrusted third-party executable because some versions of ldd may directly invoke the executable to identify its library dependencies, which can be security risk.
