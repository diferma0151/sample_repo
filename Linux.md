# Bash commands #
## Process dependencies lookup ##
### Check shared library dependencies of a program executable ###
To find out what libraries a particular executable depends on, you can use ldd command. This command invokes dynamic linker to find out library dependencies of an executable.

```
#!bash
$ ldd /path/to/program

```
![1.jpg](https://bitbucket.org/repo/48R4X/images/159489852-1.jpg)

Note that it is **NOT recommended** to run ldd with any untrusted third-party executable because some versions of ldd may directly invoke the executable to identify its library dependencies, which can be security risk.

Instead, a safer way to show library dependencies of an unknown application binary is to use the following command.

```
#!bash
$ objdump -p /path/to/program | grep NEEDED

```

![2.png](https://bitbucket.org/repo/48R4X/images/2304139892-2.png)
### Check shared library dependencies of a running process ###
If you want to find out what shared libraries are loaded by a running process, you can use pldd command, which shows all shared objects loaded into a process at run-time.

```
#!bash
$ sudo pldd <PID>
```

![3.jpg](https://bitbucket.org/repo/48R4X/images/1488258748-3.jpg)
Note that you need root privilege to run pldd command.

Alternatively, a command line utility called pmap, which reports memory map of a process, can also show shared library dependencies of a running process.

```
#!bash
$ sudo pmap <PID>>
```
![4.jpg](https://bitbucket.org/repo/48R4X/images/2061893745-4.jpg)
