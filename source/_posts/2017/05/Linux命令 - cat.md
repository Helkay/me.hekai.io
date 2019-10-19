---
title: Linux命令 - cat
categories:
  - Blog
tags:
  - Linux
abbrlink: 45036
date: 2017-05-15 00:00:00
---

### 功能：
* 显示文件的内容

``` bash
cat filename
```

* 创建文件

``` bash
cat > filename
```

值得一提的是执行这个命令之后会进入一个输入模式,只会保存回车后的内容

* 合并内容

``` bash
cat file1 file2 > file
```

将 file1 和 file2 的内容合并到 file 中,按照从左至右的先后顺序,如果 file 已存在则会覆盖 file中的内容


### 使用权限：
所有使用者

### 参数说明：
**-n :** 输出每行的行数编号

``` bash
$cat -n test 
1	This is the 1th line
2	
3	This is the 3th line
4	
5	This is the 5th line
```

**-b :** 和 -n 差不多,只是不输出空白行

``` bash
$cat -b test
1	This is the 1th line

2	This is the 3th line

3	This is the 5th line 
```

**-s :** 将连续两行以上的空白行,用一行空白行来代替

原文件内容(数字为行号)如下

```
  1 This is the 1th line
  2 
  3 
  4 This is the 4th line
  5 
  6 
  7 
  8 
  9 This is the 9th line
```

使用-s输出结果如下

``` bash
$cat -s test 
This is the 1th line

This is the 4th line

This is the 9th line
```

**-v :** 使用 ^ 和 M- 符号，除了 LFD 和 TAB 之外

**-E :** 或 --show-ends : 在每行结束处显示 $

**-T :** 或 --show-tabs: 将 TAB 字符显示为 ^I

**-A :** 等价于 -vET

**-e :** 等价于"-vE"选项

**-t :** 等价于"-vT"选项






