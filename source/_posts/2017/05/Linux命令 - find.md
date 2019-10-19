---
title: Linux命令 - find
categories:
  - 知识点
tags:
  - Linux
abbrlink: 2085
date: 2017-05-15 00:00:00
---


### 功能：
在指定目录下查找文件,如果不设置任何参数,则find命令将在当前目录下查找子目录与文件

### 命令格式：

``` bash
find pathname -options [-print -exec -ok ...]
```

**pathname:**  find命令所查找的目录路径。例如用.来表示当前目录，用/来表示系统根目录。 

**-print:**  find命令将匹配的文件输出到标准输出。 

**-exec:**  find命令对匹配的文件执行该参数所给出的shell命令。相应命令的形式为'command' {  } \;，注意{   }和\；之间的空格。 

**-ok:**  和-exec的作用相同，只不过以一种更为安全的模式来执行该参数所给出的shell命令，在执行每一个命令之前，都会给出提示，让用户来确定是否执


### 命令选项：

**-name :**   按照文件名查找文件。

``` bash
find . -name filename

#查找当前目录包括子目录及子目录里的目录里以.txt结尾的文件
find . -name '*.txt'

```

**-perm :**   按照文件权限来查找文件。

``` bash
find . -perm 755
```

顺带一提  在linux下权限数字rwx  r=4，w=2，x=1 

**-prune :**  使用这一选项可以使find命令不在当前指定的目录中查找,如果同时使用-depth选项，那么-prune将被find命令忽略。

``` bash
#查找当前目录下的子目录及子目录中 名为dir文件夹中以.txt结尾的所有文件
find . -path '*dir*' -name '*.txt'

#只查找当前目录下 以.txt结尾的文件
find . -depth 1 -name '*.txt'
find . -maxdepth 1 -name '.txt'

#查找当前目录下名为dir的子目录中所有以.txt结尾的文件
find . -path './dir*' -name '*.txt'

#查找dir目录中除了子目录dir0下其他子目录中以.txtx结尾的所有文件
find . -path './dir0' -prune -o -name '*.txt' -print

```

**-user :**   按照文件属主来查找文件。

``` bash
find . -user Helkay
```

**-group :**  按照文件所属的组来查找文件。

``` bash
find . -group staff
```

**-mtime -n +n :**  按照文件的更改时间来查找文件， - n表示文件更改时间距现在n天以内,+ n表示文件更改时间距现在n天以前。find命令还有-atime和-ctime 选项，但它们都和-m time选项。

**-nogroup :**  查找无有效所属组的文件,即该文件所属的组在/etc/groups中不存在。

``` bash
#列出/home内不属于本地组的文件或目录
find /home -nogroup                   
```

**-nouser :**   查找无有效属主的文件,即该文件的属主在/etc/passwd中不存在。

``` bash
#列出/home内不属于本地用户的文件或目录  
find /home -nouser                                    
```

**-newer file1 ! file2 :**  查找更改时间比文件file1新但比文件file2旧的文件。

``` bash
find /home -newer tmp.txt ! tmp1.txt
```

**-type :**  查找某一类型的文件,诸如：

* b - 块设备文件。
* d - 目录。
* c - 字符设备文件。
* p - 管道文件。
* l - 符号链接文件。
* f - 普通文件。

``` bash 
find . -type d
```


**-size n：[c,k] :** 查找文件长度为n块的文件,带有c时表示文件长度以字节计。-depth：在查找文件时,首先查找当前目录中的文件,然后再在其子目录中查找。

``` bash
#查找当前目录下(包括子目录)文件大于100k 小于200k 的文件
find . -size +100k -size -200k
```

**-fstype :** 查找位于某一类型文件系统中的文件,这些文件系统类型通常可以在配置文件/etc/fstab中找到,该配置文件中包含了本系统中有关文件系统的信息。

``` bash
find / -fstype ext2
```

**-mount :** 在查找文件时不跨越文件系统mount点。

**-follow :** 如果find命令遇到符号链接文件,就跟踪至链接所指向的文件。

**-cpio :**对匹配的文件使用cpio命令,将这些文件备份到磁带设备中。

另外,下面三个的区别:

**-amin n :**  查找系统中最后N分钟访问的文件

**-atime n :** 查找系统中最后n*24小时访问的文件

**-cmin n :**  查找系统中最后N分钟被改变文件状态的文件

**-ctime n :** 查找系统中最后n*24小时被改变文件状态的文件

**-mmin n :**  查找系统中最后N分钟被改变文件数据的文件

**-mtime n :** 查找系统中最后n*24小时被改变文件数据的文件

