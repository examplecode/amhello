# 使用automake 和 autoconf 工具 #

本示例用来展示gnu autoconf 及automake的使用方法.

## step1: 创建项目 ##


项目的开始我们首先要确定项目的目录布局. 以经典的hello world为例，我们先准备好了示例文件. 项目的布局如下

    .
    ├── README.md
    └── src
        └── hello.c




## step2: 使用autoscan 生成 configure.scan ##

这个命令会为我们生成configure.in的一个模板，我们需要把configure.scan 重命名为configue.ac或configrue.in

    autoscan
    mv configure.scan configure.ac

生成的configure.ac模板内容如下:

    AC_PREREQ([2.68])
    AC_INIT([FULL-PACKAGE-NAME], [VERSION], [BUG-REPORT-ADDRESS])
    AC_CONFIG_SRCDIR([src/hello.c])
    AC_CONFIG_HEADERS([config.h])

    # Checks for programs.
    AC_PROG_CC

    # Checks for libraries.

    # Checks for header files.
    AC_CHECK_HEADERS([stdlib.h])

    # Checks for typedefs, structures, and compiler characteristics.

    # Checks for library functions.

    AC_OUTPUT

## step3: 修改模板文件并根据configure.ac 用aclocal生成aclocal.m4文件 ##

修改过文件如下

   AC_PREREQ([2.68])
    AC_INIT([amhello], [1.0], [chengkai.me@gmail.com])
    #新增automake的初始化，否则运行automake时会出错
    AM_INIT_AUTOMAKE([-Wall -Werror foreign])
                                                                                                          
    AC_CONFIG_SRCDIR([src/hello.c])
    #目前不使用config.h可以先屏蔽掉
    #AC_CONFIG_HEADERS([config.h])
    #定义生成Makefile的名称
    AC_CONFIG_FILES([
                      Makefile
                       src/Makefile
                       ])
    
    
    # Checks for programs.
    AC_PROG_CC
    
    # Checks for libraries.
    
    # Checks for header files.
    AC_CHECK_HEADERS([stdlib.h])
    
    # Checks for typedefs, structures, and compiler characteristics.
    
    # Checks for library functions.
    
    AC_OUTPUT


生成aclocal.m4文件

    aclocal

## step4: 运行autoconf生成configure文件 ##

    autoconf
    
## step5: 提供Makefile.am文件 ##

Makefile.am是生成Makefile的蓝本，在这个示范中我们需要分别在项目根目录和源码目录提供一个Makefile.am文件。

amhello/Makefile.am

    SUBDIRS = src
    dist_doc_DATA = README.md

amhello/src/Makefile.am

    bin_PROGRAMS = hello
    hello_SOURCES = hello.c


## step6: 运行automake 生成Makefle.in文件 ##
automake 会根据configure.ac及Makefile.am生成Makefile.in文件，而Makefile.in文件在运行configure时作为生成Makefile的依据。 下面的命令也会生成一些辅助脚本如'install-sh','INSTALL'等

    automake --add-missin

## step7: 生成Makefile并编译打包发布 ##
到此我们就可以完成了整个autoconf,automake 的配置工作了。下面的命令就可以完成makefile的生成，编译，安装和打包工作

    ./configure    //生成makefile 
    make           //编译
    sudo make install  //安装
    make distcheck    //生成amhello-1.0.tar.gz
    


## 参考资料 ##

http://www.gnu.org/savannah-checkouts/gnu/autoconf/manual/autoconf-2.69/html_node/The-GNU-Build-System.html#The-GNU-Build-System

http://www.gnu.org/software/automake/manual/html_node/Creating-amhello.html#Creating-amhello
</markdown>
