# Abinit 软件使用

## 参考文献

[User guide](https://docs.abinit.org/)

[Home page](https://www.abinit.org/)

[Abinit wiki](https://wiki.abinit.org/doku.php)

[配置线性代数模块](https://wiki.abinit.org/doku.php?id=build:linear_algebra)

[ABINIT的前世今生](http://muchong.com/html/200812/1082565.html)

[**安装ppt**](https://wiki.abinit.org/lib/exe/fetch.php?media=build:installing-abinit.pdf)

## 引言

* What is ABINIT

ABINIT is a software suite to calculate the optical, mechanical, vibrational, and other observable properties of materials. Starting from the quantum equations of density functional theory, you can build up to advanced applications with perturbation theories based on DFT, and many-body Green's functions (GW and DMFT) .
ABINIT can calculate molecules, nanostructures and solids with any chemical composition, and comes with several complete and robust tables of atomic potentials.
On-line tutorials are available for the main features of the code, and several schools and workshops are organized each year.

ABINIT 是一个可以计算材料的光学，力学，震动和其它 ***可以观测到的性质*** 的软件。
软件从DFT理论的量子力学表达式出发，你可以建立基于DFT和微扰理论以及多体格林方程的应用。
ABINIT软件可以计算具有任何化学成分的分子，纳米结构和固体，并且自带几个完备和可靠的原子势列表。
程序的主要特征在线上教程中有介绍，并且每年会举行几个有关的学习论坛和worshop.

## 安装

### 下载解压

* 下载安装包`abinit-8.10.1.tar.gz`(以我安装的版本为例)
* 上传至服务器
* 解压：`tar -zxvf abinit-8.10.1.tar.gz`
* 进入到解压完的目录
  * 目录中包括的内容：
  * abinit源码和lib，分别在`src`和`lib`文件夹中(但我没有看到有`lib`文件夹)
  * 文档，在`doc`文件夹内，与在线教程是对应的
  * `tests`文件夹，包含测试用程序，也当作教学案例
  * `config`文件夹，产生makefile所需的文件
  * 不包含二进制可执行文件，需要编译产生

### **configure**

下面的configure步骤是安装最关键的步骤，这一步包括了环境的配置和具体生成具有什么功能的abinit的设置，设置后的编译是不变的，只有配置这一步有改变。

在集群上编译abinit的时候最好使用文件来储存configure信息，在其中详细配置编译器，数学库，串并行等特点。

现在在五洲上编译成功的configure file主要内容如下：

```bash
prefix="${HOME}/usr/mpiabinit"
CC="mpicc"
CXX="mpicxx"
FC="mpiifort"
enable_mpi="yes"
with_mpi_incs="-I/opt/intel/compilers_and_libraries_2018.1.163/linux/mpi/include64"
with_mpi_level="2"
with_mpi_libs="-L/opt/intel/compilers_and_libraries_2018.1.163/linux/mpi/lib64 -lmpi"
with_trio_flavor="netcdf"
with_fft_flavor="fftw3-mkl"
with_fft_incs="-I${MKLROOT}/include"
with_fft_libs="-L${MKLROOT}/lib/intel64 -Wl,--start-group  -lmkl_intel_lp64 -lmkl_sequential -lmkl_core -Wl,--end-group -lpthread -lm -ldl"
with_linalg_flavor="mkl"
with_linalg_incs="-I${MKLROOT}/include"
with_linalg_libs="-L${MKLROOT}/lib/intel64 -Wl,--start-group  -lmkl_intel_lp64 -lmkl_sequential -lmkl_core -Wl,--end-group -lpthread -lm -ldl"
with_dft_flavor="libxc"
```

### complie

* 执行`make mj4`意味4进程并行编译，此步还是很大概率出错的，或者卡住。如果卡住那么可以再次执行此命令，程序会跳过上次编译好的内容。
* 执行`make install`

## 使用

