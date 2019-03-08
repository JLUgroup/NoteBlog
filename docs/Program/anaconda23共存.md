# anaconda2 与 anaconda3 共存解决方案

<!--ts-->
   * [anaconda2 与 anaconda3 共存解决方案](#anaconda2-与-anaconda3-共存解决方案)
      * [参考文献](#参考文献)
      * [操作简介](#操作简介)

<!-- Added by: xiejiahao, at:  -->

<!--te-->
众所周知anaconda是特别好用的python环境管理方案，有时我们需要python2和python3共存的环境，并且希望两个环境中都包含anaconda自带的科学包，也就是说，希望两个anaconda共存。

经过努力，我们找到了成功的解决方案

## 参考文献

https://blog.csdn.net/jywowaa/article/details/78479554

## 操作简介
1. 首先确定以哪个环境为基础环境，我选择anaconda3 也就是python3环境为基础环境。于是首先安装anaconda3,并让其自动修改`.bashrc`文件。

2. 在安装完毕基础环境后，不需要创建虚拟环境（也不要修改bashrc文件），直接执行以下代码：
```bash
$ bash Anaconda2-5.3.0-Linux-x86_64.sh -b -p $HOME/anaconda3/envs/py2
$ rm -f $HOME/anaconda3/envs/py2/bin/conda*
$ rm -f $HOME/anaconda3/envs/py2/conda-meta/conda-*
$ rm -f $HOME/anaconda3/envs/py2/bin/activate
$ rm -f $HOME/anaconda3/envs/py2/bin/deactivate
$ cd $HOME/anaconda3/envs/py2/bin
$ ln -s ../../../bin/conda .
$ ln -s ../../../bin/activate .
$ ln -s ../../../bin/deactivate .
```
上面代码的解释为：
首先将anaconda2安装到anaconda3的`env`文件夹下的`py2`文件夹。`env`文件夹是anaconda用来存放虚拟环境的文件夹，所以这样安装相当于创建了一个虚拟环境。
但在使用此虚拟环境时，由于新安装的anaconda无法使用旧anaconda的`conda`,`activate`,`deactivate`命令会导致错误，所以把新安装的anaconda中这些命令删除并软连接为旧anaconda的命令，这样在使用过程中就不会出错了。

其它见参考文献。
