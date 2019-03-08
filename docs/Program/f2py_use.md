# 利用f2py实现python调用fortran

<!--ts-->
   * [利用f2py实现python调用fortran](#利用f2py实现python调用fortran)
      * [参考文献](#参考文献)
      * [引言](#引言)
      * [最快的直接使用方式](#最快的直接使用方式)
      * [生成.pyf文件的使用方法](#生成pyf文件的使用方法)
      * [在fortran源码中加入注释的使用方式](#在fortran源码中加入注释的使用方式)
      * [生成的具体python函数](#生成的具体python函数)
      * [与生成函数的参数传递](#与生成函数的参数传递)

<!-- Added by: xiejiahao, at:  -->

<!--te-->

## 参考文献

<https://docs.scipy.org/doc/numpy/f2py/getting-started.html#the-smart-way>

## 引言

f2py是包含在numpy中的一个工具，可以实现产生fortran程序的python接口功能。

由于fortran函数可能不区分输入输出参数，而python函数是必须区分的，为了明确输入输出参数，f2py有三种使用方式：

1. 不做任何修改直接执行命令生成python模块。f2py会自己识别输入输出参数，但不一定准确，可能会出错。
2. 先生成`.pyf`文件，在其中修改内容指定输入输出后生成python模块。
3. 修改fortran程序源码加入注释明确输入输出后执行命令生成python模块。 
   
下面详细介绍每种方式的使用流程：

## 最快的直接使用方式
```bash
f2py -c fname.f -m fname
```
其中`fname.f`为fortran文件的名字，`fname`为生成的python模块的名字。

## 生成`.pyf`文件的使用方法

```
f2py fib1.f -m fib2 -h fib1.pyf
```
使用以上的命令会生成名为`fib1.pyf`的文件，其内容如下：

```
!    -*- f90 -*-
python module fib2 ! in 
    interface  ! in :fib2
        subroutine fib(a,n) ! in :fib2:fib1.f
            real*8 dimension(n) :: a
            integer optional,check(len(a)>=n),depend(a) :: n=len(a)
        end subroutine fib
    end interface 
end python module fib2

! This file was auto-generated with f2py (version:2.28.198-1366).
! See http://cens.ioc.ee/projects/f2py2e/
```
其主要内容为解析的fortran文件的内容，会列出所有包含的`subroutine`的标题，参数和参数定义。

生成此文件的目的为可以在其中加入`intent(in)`,`intent(out)`,`depend(n)`字符来明确参数的输入输出和依赖特性。

修改后的`.pyf` 文件如下：

```
!    -*- f90 -*-
python module fib2 
    interface
        subroutine fib(a,n)
            real*8 dimension(n),intent(out),depend(n) :: a
            integer intent(in) :: n
        end subroutine fib
    end interface 
end python module fib2
```
随后执行如下命令生成python模块。

```
f2py -c fib2.pyf fib1.f
```

## 在fortran源码中加入注释的使用方式

直接在fortran源码中加入注释：

```fortran
!f2py intent(in) n
!f2py intent(out) a
!f2py depend(n) a
```
来明确输入输出及依赖，例子如下：
```fortran
! FILE: FIB3.F
      SUBROUTINE FIB(A,N)
!
!     CALCULATE FIRST N FIBONACCI NUMBERS
!
      INTEGER N
      REAL*8 A(N)
!f2py intent(in) n
!f2py intent(out) a
!f2py depend(n) a
      DO I=1,N
         IF (I.EQ.1) THEN
            A(I) = 0.0D0
         ELSEIF (I.EQ.2) THEN
            A(I) = 1.0D0
         ELSE 
            A(I) = A(I-1) + A(I-2)
         ENDIF
      ENDDO
      END
! END FILE FIB3.F
```
随后执行命令：
```bash
f2py -c -m fib3 fib3.f
```
即可生成python模块。

## 生成的具体python函数

在使用过程中，具体生成的python函数需要从运行过程中显示的信息来判断。


## 与生成函数的参数传递
如果涉及到数组，最好使用`numpy`模块来解决。

