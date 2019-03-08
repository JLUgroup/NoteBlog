# Python中os.walk函数的用法（遍历文件夹下文件并获得路径）

 <!--ts-->
   * [Python中os.walk函数的用法（遍历文件夹下文件并获得路径）](#python中oswalk函数的用法遍历文件夹下文件并获得路径)
      * [参考文献](#参考文献)
      * [引言](#引言)
      * [os.walk使用](#oswalk使用)
      * [获得所有子文件路径（os.path.join使用）](#获得所有子文件路径ospathjoin使用)

<!-- Added by: xiejiahao, at:  -->

  <!--te-->

## 参考文献

<https://blog.csdn.net/bagboy_taobao_com/article/details/8938126>

<https://www.jianshu.com/p/bbad16822eab>

<https://blog.csdn.net/lom9357bye/article/details/79285170>

<http://python-docs.com/python-files/python-os-path-join.html>

## 引言
我们在使用python时时常会遇到调用某些文件的需求，这时我们就需要得到这些文件的路径。python强大的自带os模块使得获得路径变得很容易。下面介绍如何使用`os.walk`函数来遍历文件夹及子文件夹下所有文件并得到路径。

`os.walk`的完整定义形式如下：
```python
os.walk(top, topdown=True, onerror=None, followlinks=False)
```

参数：

* top:需要遍历目录的地址。
* topdown 为真，则优先遍历top目录，否则优先遍历top的子目录(默认为开启)。
* onerror 需要一个 callable 对象，当walk需要异常时，会调用。
* followlinks 如果为真，则会遍历目录下的快捷方式(linux 下是 symbolic link)实际所指的目录(默认关闭)。

以上四个参数一般只需要指定第一个文件夹路径，剩下的一般情况不需要指定。

## `os.walk`使用

os.walk 的返回值是一个生成器(generator),也就是说我们需要用循环不断的遍历它（不可以直接`print`），来获得所有的内容。

每次遍历的对象都是返回的是一个三元元组(root,dirs,files)

* root 所指的是当前正在遍历的这个文件夹的本身的地址
* dirs 是一个 list ，内容是该文件夹中所有的目录的名字(不包括子目录)
* files 同样是 list , 内容是该文件夹中所有的文件(不包括子目录)

>***注意，函数会自动改变`root`的值使得遍历所有的子文件夹。所以返回的三元元组的个数为所有子文件夹（包括子子文件夹，子子子文件夹等等）加上1（根文件夹）。***

举例：

对于具有以下结构的目录进行测试：

```
$ cd namesort/
$ tree
.
|-- namelist.txt
|-- nameout.txt
|-- namesorttest.py
`-- test
    |-- name2.txt
    |-- namelist.txt
    |-- nameout.txt
    `-- namesort.py
```

执行：
```python
import os
path = '/home/jhxie/Workspace/namesort'
for root,dirs,files in os.walk(path):
        print root,dirs,files
```
输出为：
```
/home/jhxie/Workspace/namesort ['test'] ['nameout.txt', 'namelist.txt', 'namesorttest.py']
/home/jhxie/Workspace/namesort/test [] ['nameout.txt', 'name2.txt', 'namelist.txt', 'namesort.py']
```

## 获得所有子文件路径（`os.path.join`使用）

由于`os.walk`获得的并不是路径，所以需要将其内容进行连接得到路径。

这时使用python自带函数`os.path.join`,其语法为：

```python
os.path.join(path1[, path2[, ...]])
```
其中嵌套的[]表示写在最前面的是高级目录，后面的是低级的，也就是按参数排列顺序拼接。

举例：
```python
os.path.join("home", "me", "mywork")
```
在Linux系统上会返回`home/me/mywork`  
在Windows系统上会返回`home\me\mywork`

>***可能大家已经注意到了，此函数并不是简单的字符串连接函数，你不需要在输入的参数字符串中加入分隔符，函数会根据你的系统自动加入对应的分隔符，这也是这个函数存在的意义所在。***

所以我们正好使用`os.path.join()`来处理上面生成的遍历结果：

```python
import os
path = '/home/jhxie/Workspace/namesort'
for root,dirs,files in os.walk(path):
        for file in files:
                print(os.path.join(root,file))
```
输出结果：
```
/home/jhxie/Workspace/namesort/nameout.txt
/home/jhxie/Workspace/namesort/namelist.txt
/home/jhxie/Workspace/namesort/namesorttest.py
/home/jhxie/Workspace/namesort/test/nameout.txt
/home/jhxie/Workspace/namesort/test/name2.txt
/home/jhxie/Workspace/namesort/test/namelist.txt
/home/jhxie/Workspace/namesort/test/namesort.py
```



