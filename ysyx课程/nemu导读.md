# man 工具
man 命令可以用来查看 Linux 系统的手册页，手册页是一种文档，它介绍了 Linux 的命令、程序和函数的用法和功能。要使用 man 命令查看手册页，你可以在终端输入：
man <命令名>

例如，要查看 ls 命令的手册页，你可以输入：

man ls

这样就会打开 ls 命令的手册页，你可以用上下箭头键或鼠标滚轮来浏览，用空格键或 PgDn 和 PgUp 键来翻页，用 Home 和 End 键来跳到开头或结尾，用 Q 键来退出。

手册页有不同的类别，比如 1 代表用户命令，2 代表系统调用，3 代表库函数等。你可以在手册页的标题中看到类别的编号，比如 ls (1)。如果你想查看某个类别的手册页，你可以在命令名前加上类别编号，比如：

man 2 open

这样就会打开 open 函数的手册页，而不是 open 命令的手册页。

如果你想搜索手册页中的内容，你可以按/键，然后输入你想查找的关键字，比如：

/option

这样就会高亮显示 option 这个词，并跳到第一个匹配的位置。你可以按 n 键来跳到下一个匹配的位置，或者按 N 键来跳到上一个匹配的位置。

# make 工具


`strace -o a.log make` 
``` shell
stat("makefile", {st_mode=S_IFREG|0644, st_size=37, ...}) = 0
stat("a.out", {st_mode=S_IFREG|0755, st_size=16464, ...}) = 0
stat("a.c", {st_mode=S_IFREG|0644, st_size=23, ...}) = 0
openat(AT_FDCWD, "/usr/share/locale/C.UTF-8/LC_MESSAGES/make.mo", O_RDONLY) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/share/locale/C.utf8/LC_MESSAGES/make.mo", O_RDONLY) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/share/locale/C/LC_MESSAGES/make.mo", O_RDONLY) = -1 ENOENT (No such file or directory)
op
```

## Stat
Stat 的系统调用
```
NAME
       stat - display file or file system status

SYNOPSIS
       stat [OPTION]... FILE...

DESCRIPTION
       Display file or file system status.

       Mandatory arguments to long options are mandatory for short options too.
```

```
man 2 stat
```

man 2 stat 是一个命令，它可以查看 stat 系统调用的手册页。stat 系统调用可以获取一个文件的状态信息，比如大小、类型、权限、时间等。它有三种形式：stat、fstat 和 lstat，分别对应不同的参数类型¹。

man 命令可以查看不同类别的手册页，比如 1 代表用户命令，2 代表系统调用，3 代表库函数等。如果不指定类别，man 会按照顺序查找第一个匹配的手册页。所以 man 2 stat 是为了确保查看的是系统调用的手册页，而不是用户命令的手册页。




```makefile
STAT(3)                                                   Linux Programmer's Manual                                                  STAT(2)

NAME
       stat, fstat, lstat, fstatat - get file status

SYNOPSIS
       #include <sys/types.h>
       #include <sys/stat.h>
       #include <unistd.h>

       int stat(const char *pathname, struct stat *statbuf);
       int fstat(int fd, struct stat *statbuf);
       int lstat(const char *pathname, struct stat *statbuf);

       #include <fcntl.h>           /* Definition of AT_* constants */
       #include <sys/stat.h>

       int fstatat(int dirfd, const char *pathname, struct stat *statbuf,
                   int flags);

   Feature Test Macro Requirements for glibc (see feature_test_macros(7)):

       lstat():
           /* glibc 2.19 and earlier */ _BSD_SOURCE
               || /* Since glibc 2.20 */ _DEFAULT_SOURCE
               || _XOPEN_SOURCE >= 500
               || /* Since glibc 2.10: */ _POSIX_C_SOURCE >= 200112L

       fstatat():
           Since glibc 2.10:
               _POSIX_C_SOURCE >= 200809L
           Before glibc 2.10:
               _ATFILE_SOURCE

DESCRIPTION
       These  functions  return  information about a file, in the buffer pointed to by statbuf.  No permissions are required on the file it‐
       self, but—in the case of stat(), fstatat(), and lstat()—execute (search) permission is required on all of the directories in pathname
       that lead to the file.

       stat() and fstatat() retrieve information about the file pointed to by pathname; the differences for fstatat() are described below.

       lstat()  is  identical  to stat(), except that if pathname is a symbolic link, then it returns information about the link itself, not
       the file that it refers to.

       fstat() is identical to stat(), except that the file about which information is to be retrieved is specified by the  file  descriptor
       fd.

   The stat structure
       All of these system calls return a stat structure, which contains the following fields:

           struct stat {
               dev_t     st_dev;         /* ID of device containing file */
               ino_t     st_ino;         /* Inode number */
               mode_t    st_mode;        /* File type and mode */
               nlink_t   st_nlink;       /* Number of hard links */
               uid_t     st_uid;         /* User ID of owner */
               gid_t     st_gid;         /* Group ID of owner */
               dev_t     st_rdev;        /* Device ID (if special file) */
               off_t     st_size;        /* Total size, in bytes */
               blksize_t st_blksize;     /* Block size for filesystem I/O */
               blkcnt_t  st_blocks;      /* Number of 512B blocks allocated */

               /* Since Linux 2.6, the kernel supports nanosecond
                  precision for the following timestamp fields.
                  For the details before Linux 2.6, see NOTES. */

               struct timespec st_atim;  /* Time of last access */
               struct timespec st_mtim;  /* Time of last modification */
               struct timespec st_ctim;  /* Time of last status change */

           #define st_atime st_atim.tv_sec      /* Backward compatibility */
           #define st_mtime st_mtim.tv_sec
           #define st_ctime st_ctim.tv_sec
           };

       Note:  the  order  of  fields in the stat structure varies somewhat across architectures.  In addition, the definition above does not
       show the padding bytes that may be present between some fields on various architectures.  Consult the glibc and kernel source code if
       you need to know the details.

       Note: for performance and simplicity reasons, different fields in the stat structure may contain state information from different mo‐
       ments during the execution of the system call.  For example, if st_mode or st_uid is changed by another process by  calling  chmod(2)
       or chown(2), stat() might return the old st_mode together with the new st_uid, or the old st_uid together with the new st_mode.

       The fields in the stat structure are as follows:

       st_dev This  field describes the device on which this file resides.  (The major(3) and minor(3) macros may be useful to decompose the
              device ID in this field.)

       st_ino This field contains the file's inode number.

       st_mode
              This field contains the file type and mode.  See inode(7) for further information.

       st_nlink
              This field contains the number of hard links to the file.

       st_uid This field contains the user ID of the owner of the file.

       st_gid This field contains the ID of the group owner of the file.
```

```
STAT(1)                                                         User Commands                                                        STAT(1)

NAME
       stat - display file or file system status

SYNOPSIS
       stat [OPTION]... FILE...

DESCRIPTION
       Display file or file system status.

       Mandatory arguments to long options are mandatory for short options too.

```
