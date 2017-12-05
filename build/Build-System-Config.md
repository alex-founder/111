# 编译系统配置

## 定制目录排布

默认情况下, 编译系统采用如下的目录排布:

    CONFIG_DIR      ?= $(TOP_DIR)/configs
    DIST_DIR        ?= $(TOP_DIR)/output
    FINAL_DIR       ?= $(DIST_DIR)/release
    IMPORT_DIR      ?= $(TOP_DIR)/import
    LCOV_DIR        ?= Coverage
    MAKE_SEGMENT    ?= iot.mk
    OUTPUT_DIR      ?= $(TOP_DIR)/.O
    PACKAGE_DIR     ?= $(TOP_DIR)/packages
    RULE_DIR        ?= $(TOP_DIR)/build-rules
    TOP_DIR         ?= $(CURDIR)

含义如下:

| 变量名            | 说明                                      |
|-------------------|-------------------------------------------|
| `CONFIG_DIR`      | *硬件平台配置文件*的存放目录, 其中的文件需要以`config.XXX.YYY`形式命名, 其中`XXX`一般表示操作系统, `YYY`一般表示具体的硬件型号 |
| `DIST_DIR`        | 最终的编译产物, 例如可执行程序, SDK的总库等, 存放的顶层目录 |
| `FINAL_DIR`       | 最终的编译产物摆放时, 会放在`$(FINAL_DIR)`下, 这个变量可以指定最终摆放位置在顶层目录下的子目录路径 |
| `IMPORT_DIR`      | 输入目录, 用于摆放预编译的二进制库, 以及使用这些预编译库对应的头文件 |
| `LCOV_DIR`        | 覆盖率文件产生目录, 用于存放`lcov`软件产生的中间文件和最终的`HTML`格式图形化文件 |
| `MAKE_SEGMENT`    | 编译单元中的片段文件, 开发者唯一需要编辑的文件, 指导编译系统如何编译该文件所在的编译单元 |
| `OUTPUT_DIR`      | 编译树中间目录 |
| `PACKAGE_DIR`     | 打包目录, 其中可以存放其它组件, 也可以存放压缩包形式的第三方软件, 例如`openssl-0.9.8.tar.gz`等 |
| `RULE_DIR`        | 编译系统核心目录, 如果不喜欢`build-rules`存放在自己的工程里, 完全可以通过这个变量把它移出去 |
| `TOP_DIR`         | 工程的顶级目录, 默认就是顶层`makefile`所在的路径, 极少改动 |

可以完全不对它们做任何改动, 依照上文的含义和默认值来排布自己工程

---
也可以通过顶层目录的`project.mk`, 可以改动它们, 例如:

    $ cat project.mk
    MAKE_SEGMENT := alink.mk

则表示在每个编译单元中, 指导编译过程的片段文件不再是默认的`iot.mk`, 而是`alink.mk`

## 定制单元测试

如果你的工程含有配合编译系统的单元测试用例, 则可以在顶层`makefile`中写类似如下语句, 告诉编译系统如何在`make test`命令中执行它们, 并统计源码覆盖率

    $(call Add_Coverage_Progs, utils-tests --list)
    $(call Add_Coverage_Progs, utils-tests)
    $(call End_Coverage_Progs)

语法是:

    $(call Add_Coverage_Progs, <program> <argument-list>)
    ...
    # <program> 是单元测试程序的名字
    # <argument-list> 是单元测试程序对应的参数列表
    ...
    # 重复1-N次, 告诉编译系统依次执行所有的单元测试程序
    ...
    $(call End_Coverage_Progs)
    # 告诉编译系统, 到这里为止单元测试程序列举完毕

> 以上面的`LITE-utils`为例, 其中的`utils-tests`就是它的单元测试程序, 这是在编译单元`testsuites`中产生的一个`Linux`下可执行程序

    $ cat testsuites/iot.mk -n
         1  TARGET      := utils-tests
         2  DEPENDS     += external/cut src
         3  HDR_REFS    := src
         4
         5  LDFLAGS     += -llite-utils -llite-log -llite-cut

为什么如上的一个简单`iot.mk`片段文件, 就能指导编译系统生成可执行程序`utils-tests`, 可以访问[**编译单元说明**](https://code.aliyun.com/edward.yangx/public-docs/wikis/build/build-system-units)
