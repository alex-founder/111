# 如何装备测试框架

## 使用最新的构建系统

参考[构建系统概述](https://code.aliyun.com/edward.yangx/public-docs/wikis/build/build-system-introduction)页面, 了解如何获取最新的构建系统到你的项目中, 这是装备测试框架的前提

也可以直接访问样板组件[LITE-utils](http://gitlab.alibaba-inc.com/iot-middleware/LITE-utils)的仓库, 其中测试框架和测试例已经就绪, 可以直接体验和观察单元测试框架的工作方式和效果

    $ git clone git@gitlab.alibaba-inc.com:iot-middleware/LITE-utils.git

## 构建测试框架库

按照如下步骤, 新建一个构建单元, 用它把测试框架库编译出来, 其实本质就是把`build-rules/misc/cut.c`编译成一个`libcut.a`或者`libcut.so`的库, 供测试主程序链接

### 创建构建单元

参考[构建单元说明](https://code.aliyun.com/edward.yangx/public-docs/wikis/build/build-system-units)页面, 可以按如下方式创建

    $ mkdir -p external/cut

    $ vi external/cut/iot.mk
    1  LIBA_TARGET     := liblite-cut.a
    2
    3  LIB_HEADERS     := cut.h
    4  EXTRA_SRCS      := $(RULE_DIR)/misc/cut.[ch]

* 片段文件`iot.mk`告诉构建系统如何编译框架库, 这个文件的文件名按照你工程中的`$(MAKE_SEGMENT)`变量来命名, 如果没有在`project.mk`中重定义, 默认是`iot.mk`
* 假设按照上面的例子来创建, 则构建单元的名字就是`external/cut`, 测试主程序所在的构建单元将依赖它

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

## 添加测试集和测试例