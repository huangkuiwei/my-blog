---
title: Node.js当中的全局成员与path模块的使用总结
date: 2019-03-12 21:16:02
author: huangkuiwei
img: http://n.sinaimg.cn/sinacn/20170615/26cc-fyhfxph0303973.jpg
tags: 
    - Node.js
---
## 一、node.js全局成员
### 1、global
在浏览器的平台环境当中，全局对象为window，即任何一个定义在全局环境当中的变量都可以用window这个对象获取到。node环境当中的全局对象为global,它类似于客户端javascript运行环境当中的window。
![效果图](/medias/postimages/27.png "效果图")

### 2、process
该对象用于获取当前Node进程的信息，一般用于获取环境变量之类的信息。
**2.1、process.env**
该属性用于获取当前操作系统当中配置的环境变量，为一个对象，其中每一个成员以键值对的形式出现。当使用process.env.PATH，则可以打印出环境变量当中PATH对应的值。
![效果图](/medias/postimages/28.png "效果图")

**2.2、process.argv**
该属性用于获取当前在命令行当中传入的参数，以字符串数组的形式打印出argv当中的所有成员。
![效果图](/medias/postimages/29.png "效果图")
在用node来运行某个js文件时，传递的参数紧跟其后，各个参数之间用空格相隔。在任何情况下，argv当中的第一个成员都是node.exe所在的绝对物理路径，第二个成员为正在执行的这个js文件的绝对物理路径，从第三个成员开始才是用户在命令行当中传入的各个参数。故我们可用process.argv.slice(2)的方式去掉前两个成员。

**2.3、process.stdout**
该属性用于在控制台进行标准输出的操作。
![效果图](/medias/postimages/30.png "效果图")

### 3、两个常用的模块内部的伪全局成员
**3.1、 __dirname**
该成员用于获取当前这个js文件所在目录（所在文件夹）的完成的绝对物理路径。该成员只在模块内部有效，在REPL环境当中失效。

**3.2、__filename**
该成员用于获取当前这个js文件的完成的绝对物理路径。该成员只在模块内部有效，在REPL环境当中失效。
![效果图](/medias/postimages/31.png "效果图")
由于在node当中所有的文件操作，为了避免出错，所有的文件路径都必须使用绝对物理路径，故这两个成员应用十分广泛。

## 二、node.js当中的path模块
### 1、path.join(p1,p2)
该方法用于完成路径的拼接，用多个字符串来表示多个路径，各个元素之间用逗号相隔。
![效果图](/medias/postimages/32.png "效果图")

### 2、path.basename(p[,ext])
该方法用于获取一个完成的文件路径当中的文件名部分，若传入第二个后缀名参数，则可以得到没有后缀名的文件名。
![效果图](/medias/postimages/33.png "效果图")

### 3、path.dirname(p)
该方法用于获取一个完整的文件路径当中的文件所在的目录（文件夹）的路径地址。
![效果图](/medias/postimages/34.png "效果图")

### 4、path.delimiter
该属性用于获取当前操作系统当中默认的路径分隔符，在windows下默认的路径分割符为";",而在linux系统下的默认的路径分隔符为":"。
![效果图](/medias/postimages/35.png "效果图")

### 5、path.extname(p)
该方法用于获取一个完整的文件路径当中文件的后缀名（扩展名）。
![效果图](/medias/postimages/36.png "效果图")

### 6、path.parse(pathString)
该方法用于将一个字符串类型的路径转化为一个路径对象（pathObject）。该路径对象当中包括文件目录，文件名，扩展名等。
![效果图](/medias/postimages/37.png "效果图")

### 7、path.format(pathObject)
该方法用于将一个路径对象转化为一个字符串类型的路径（pathString）。
![效果图](/medias/postimages/38.png "效果图")

### 8、path.isAbsolute(p)
该方法用于传入的路径字符串对应的路径是绝对路径还是相对路径，返回值为true或false。（这里只是对路径进行判断，不涉及文件操作，所以该路径当中的文件是否存在并不会影响其判断结果）。
![效果图](/medias/postimages/39.png "效果图")

### 9、path.normalize(p)
该方法用于常规化一个路径字符串，它会判断当前操作系统为windows还是linux，从而来选择正斜杠还是反斜杠来常规化这个路径。同时也会处理路径当中出现多个路径分割符的问题。
![效果图](/medias/postimages/40.png "效果图")

### 10、path.relative(from,to)
该方法用于获取to相对于from的相对路径，其中要求传入的from和to的参数均为路径字符串，并且都要求为绝对路径。
![效果图](/medias/postimages/41.png "效果图")

### 11、path.resolve([from...],to)
该方法类似于path.join()，可以传入多个绝对路径字符串或相对路径字符串，最后完成路径拼接。
![效果图](/medias/postimages/42.png "效果图")

### 12、path.sep
该属性用于获取当前操作系统当中默认使用的路径成员分隔符。windows系统和linux系统当中默认的路径成员分隔符是不同的。
![效果图](/medias/postimages/43.png "效果图")

### 13、path.win32和path.posix
这两个均为属性。
> 1. path：会根据当前操作系统来确定是使用windows的方式来操作路径，还是使用linux的方式来操作路径。
> 2. path.win32：允许在任意操作系统上使用windows的方式来操作路径。
> 3. path.posix：允许在任意操作系统上使用linux的方式来操作路径。

故在windows系统中，path==path.win32，而在linux系统当中，path==path.posix。
![效果图](/medias/postimages/44.png "效果图")
![效果图](/medias/postimages/45.png "效果图")





