# 如何装备测试框架

## 使用最新的构建系统

参考[构建系统概述](https://code.aliyun.com/edward.yangx/public-docs/wikis/build/build-system-introduction)页面, 了解如何获取最新的构建系统到你的项目中, 这是装备测试框架的前提

也可以直接访问样板组件[LITE-utils](http://gitlab.alibaba-inc.com/iot-middleware/LITE-utils)的仓库, 其中测试框架和测试例已经就绪, 可以直接体验和观察单元测试框架的工作方式和效果

    $ git clone git@gitlab.alibaba-inc.com:iot-middleware/LITE-utils.git

## 构建测试框架库

按照如下步骤, 新建一个构建单元, 用它把测试框架库编译出来, 其实本质就是把`build-rules/misc/cut.c`编译成一个`liblite-cut.a`或者`liblite-cut.so`的库, 供测试主程序链接

### 创建构建单元

参考[构建单元说明](https://code.aliyun.com/edward.yangx/public-docs/wikis/build/build-system-units)页面, 可以按如下方式创建

    $ mkdir -p external/cut
    $ vi external/cut/iot.mk
    1  LIBA_TARGET     := liblite-cut.a
    2
    3  LIB_HEADERS     := cut.h
    4  EXTRA_SRCS      := $(RULE_DIR)/misc/cut.[ch]

* 片段文件`iot.mk`告诉构建系统如何编译框架库, 这个文件的文件名按照你工程中的`$(MAKE_SEGMENT)`变量来命名, 如果没有在`project.mk`中重定义, 默认是`iot.mk`
* 假设按照上面的例子来创建, 则构建单元的名字就是`external/cut`, 提供的库文件就是`liblite-cut.a`, 头文件就是`cut.h`, 测试主程序所在的构建单元将依赖它们

### 尝试编译测试框架的库
用如下的方式测试你已经可以成功的编译出测试框架库

    $ make reconfig
    $ make external/cut
    BUILDING WITH EXISTING CONFIGURATION:

    VENDOR :   generic
    MODEL  :   default

    [CC] cut.o                      <= cut.c                                    
                                       makefile
                                       cut.h
    [AR] liblite-cut.a              <= cut.o    
   
    $ ls -l .O/usr/lib/liblite-cut.a
    -rw-rw-rw- 1 edward edward 12434 12月 13 13:38 .O/usr/lib/liblite-cut.a

## 构建测试主程序

所谓测试主程序其实就是一个简单的可执行程序, 它逐个的执行所有的测试例, 并且统计和展示这些测试例的执行结果

### 创建构建单元

参考[构建单元说明](https://code.aliyun.com/edward.yangx/public-docs/wikis/build/build-system-units)页面, 可以按如下方式创建, **注意不要使用`test`作为名字**

    $ mkdir -p utest
    $ vi utest/iot.mk
    1 TARGET    := utest-prog
    2 DEPENDS   += external/cut
    3
    4 LDFLAGS   += -llite-cut
    5 LDFLAGS   += -ldemo-lib

* 片段文件`iot.mk`告诉构建系统如何编译测试主程序, 这个文件的文件名按照你工程中的`$(MAKE_SEGMENT)`变量来命名, 如果没有在`project.mk`中重定义, 默认是`iot.mk`
* 假设按照上面的例子来创建, 则构建单元的名字就是`utest`, 产生的可执行程序就是`utest-prog`, 它依赖`external/cut`在其之前编译, 会链接`liblite-cut.a`或者`liblite-cut.so`
* 需要明确你的测试主程序所测试的C函数是由什么库提供的, 在这个例子里, 被测试的C函数来自一个名为`libdemo-lib.a`的静态库或者名为`libdemo-lib.so`的动态库

### 添加测试集到主程序

接下来需要编写C语言的源文件, 产生测试主程序, 这通过在`main()`函数中直接调用`cut_main()`函数完成

    $ vi utest/utest-main.c
    1 #include "cut.h"
    2
    3 int main(int argc, char *argv[])
    4 {
    5     ADD_SUITE(demo_suite);
    6
    7     cut_main(argc, argv);
    8     return 0;
    9 }

* 源文件`utest-main.c`是用来提供`main()`函数的C文件, 文件名任意, 在这个例子中, 它编译产生测试主程序`utest-prog`
* 在这个例子中, 主测试程序执行的测试集只有`demo_suite`一个, 所谓测试集(suite)是一系列的测试例, 所谓测试例(case)是一系列调用接口和比对结果的C函数
* 访问[单元测试用例](https://code.aliyun.com/edward.yangx/public-docs/wikis/utest/ut-case)页面, 详细了解如何添加测试例, 组成测试集

## 结合测试主程序和构建系统

参考[构建工程说明](https://code.aliyun.com/edward.yangx/public-docs/wikis/build/build-system-proj)页面, 了解如何把你的测试主程序和构建系统联系起来

---
在上面的例子中, 可以编辑工程的顶层`makefile`, 添加如下语句

    $(call Add_Coverage_Progs, utest-prog --list)
    $(call Add_Coverage_Progs, utest-prog)
    $(call End_Coverage_Progs)

**之后用`make test`命令, 就可以执行测试主程序`utest-prog`, 执行测试集中的测试例, 统计和展示结果, 并统计测试例对代码的覆盖率**