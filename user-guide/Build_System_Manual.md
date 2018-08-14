# <a name="目录">目录</a>
+ [构建系统的概述](#构建系统的概述)
    * [如何获取](#如何获取)
    * [常用命令](#常用命令)
    * [如何开发](#如何开发)
    * [高级命令](#高级命令)
    * [组成部分](#组成部分)
    * [工作过程](#工作过程)
    * [详细工作过程](#详细工作过程)
    * [高级命令示例](#高级命令示例)
+ [构建系统的配置](#构建系统的配置)
    * [定制目录排布](#定制目录排布)
+ [定制工程全局的编译行为](#定制工程全局的编译行为)
    * [增加硬件平台配置文件](#增加硬件平台配置文件)
    * [定制单元测试](#定制单元测试)
+ [构建单元说明](#构建单元说明)
    * [定义](#定义)
    * [工作过程](#工作过程)
    * [开发过程](#开发过程)
    * [编写片段文件](#编写片段文件)
        - [输出相关](#输出相关)
        - [输入相关](#输入相关)
        - [组合相关](#组合相关)
        - [高级用法](#高级用法)
        - [其它](#其它)
+ [构建系统的调试](#构建系统的调试)
    * [调试开关](#调试开关)
    * [对单个构建单元调试](#对单个构建单元调试)

# <a name="构建系统的概述">构建系统的概述</a>

## <a name="如何获取">如何获取</a>

* 可访问样板组件`LITE-utils`的仓库: [*git@gitlab.alibaba-inc.com:iot-middleware/LITE-utils.git*](http://gitlab.alibaba-inc.com/iot-middleware/LITE-utils)
* 从`master`分支的`build-rules`目录复制得到构建系统的最新副本
* 也可以直接在`LITE-utils`中体验构建系统的工作方式和观察工作过程

## <a name="常用命令">常用命令</a>

| 命令                  | 解释                                                                              |
|-----------------------|-----------------------------------------------------------------------------------|
| `make distclean`      | **清除一切构建过程产生的中间文件, 使当前目录仿佛和刚刚clone下来一样**             |
| `make [all]`          | **使用默认的平台配置文件开始编译**                                                |
| `make reconfig`       | **弹出多平台选择菜单, 用户可按数字键选择, 然后根据相应的硬件平台配置开始编译**    |
| `make config`         | **显示当前被选择的平台配置文件**                                                  |
| `make help`           | **打印帮助文本**                                                                  |
| `make <directory>`    | **单独编译被<directory>指定的目录, 或者叫构建单元**                               |

## <a name="如何开发">如何开发</a>

* 访问[**构建系统配置**](https://code.aliyun.com/edward.yangx/public-docs/wikis/build/build-system-config), 了解如何配置来影响构建系统的行为
* 访问[**构建工程说明**](https://code.aliyun.com/edward.yangx/public-docs/wikis/build/build-system-proj), 了解如何定制工程全局的编译
* 访问[**构建单元说明**](https://code.aliyun.com/edward.yangx/public-docs/wikis/build/build-system-units), 了解如何增删改查你自己的构建单元
* 访问[**构建系统调试**](https://code.aliyun.com/edward.yangx/public-docs/wikis/build/build-system-debug), 了解在定制构建单元时, 如何自己调试

## <a name="高级命令">高级命令</a>

| 命令                  | 解释                                                                                      |
|-----------------------|-------------------------------------------------------------------------------------------|
| `make test`           | **运行指定的测试集程序, 统计显示测试例的通过率和源代码的覆盖率**                          |
| `make doc`            | **扫描源码目录中以`Doxygen`格式或者`Markdown`格式编写的注释, 在`html`目录产生帮助文档**   |
| `make repo-list`      | **列出当前工程引用的外部组件和它们的云端仓库地址**                                        |
| `make repo-update`    | **对所有引用到的外部组件, 用云端仓库更新本地仓库**                                        |

## <a name="组成部分">组成部分</a>

    LITE-utils$ tree -A -L 1
    .
    +-- build-rules     # 构建系统核心
    +-- example
    +-- external
    +-- makefile        # 工程的makefile
    +-- packages
    +-- project.mk      # 工程设置文件
    +-- src
    +-- testsuites

> 以`LITE-utils`为例, 对整个构建过程需要知道的只有如上三个部分:

| 组成部分      | 说明                                                                          |
|---------------|-------------------------------------------------------------------------------|
| `project.mk`  | 本工程的目录排布, 工程名称, 版本信息等, 可选                                  |
| `makefile`    | 本工程的makefile, 基于`GNU Make`, 通常只含有极少的内容, 指定编译的范围, 必选  |
| `build-rules` | 构建系统核心, 指定编译的规则, 不需要关注                                      |

## <a name="工作过程">工作过程</a>

可以从一个简单的`makefile`样例看起

     1  sinclude project.mk
     2  sinclude $(CONFIG_TPL)
     3
     4  SUBDIRS += \
     5      external/cut \
     6      src \
     7      example \
     8      testsuites \
     9
    10  CFLAGS += -DUTILS_SELF_TEST
    11
    12  include $(RULE_DIR)/rules.mk

0. 构建系统是基于`GNU Make`的简化系统, 所以工作过程的起点仍然是顶层的`makefile`
1. 读取`project.mk`, 如果当前工程的目录排布和默认的不一样, 则用当前设置, 可选
2. 读取`.config`文件, 这个文件其实是构建系统运行时所有输入的集合, 也称为*硬件平台配置文件*, 可选
3. 读取`SUBDIRS`变量, 这个变量指定了编译的范围, 必选
4. 读取`CFLAGS`或者`LDFLAGS`等变量, 顶层`makefile`的变量会被应用到每个构建单元的编译和链接, 可选
5. 读取`.../rules.mk`文件, 开始进入构建系统核心, 必选, 这又可以细分为如下过程

## <a name="详细工作过程">详细工作过程</a>

* 产生*硬件平台配置文件*, 这个过程是为了产生顶层的`.config`文件, 也就是所谓的`$(CONFIG_TPL)`
* 识别构建单元, 所谓构建单元在构建系统看来就是一个又一个包含`iot.mk`的目录, 如果不希望用`iot.mk`作为每个单元的编译片段文件, 在`project.mk`中设置`MAKE_SEGMENT`变量即可
* 从`$(SUBDIRS)`变量从前往后逐个编译每个构建单元
* 构建单元如果有依赖到其它构建单元, 则先去编译被依赖的单元; 若后者又有依赖的其它的单元, 则同样, 递归的先去编译其依赖; 以此类推

## <a name="高级命令示例">高级命令示例</a>

- **make test: 运行指定的测试集程序, 统计显示测试例的通过率和源代码的覆盖率**

![image](https://yuncodeweb.oss-cn-hangzhou.aliyuncs.com/uploads/edward.yangx/public-docs/52c806dd879f18cd7ea855a8549461f6/image.png)  

- **make doc: 扫描源码目录中以`Doxygen`格式或者`Markdown`格式编写的注释, 在`html`目录产生帮助文档**

        LITE-utils $ make doc
        ...
        ...
        Patching output file 15/16
        Patching output file 16/16
        lookup cache used 256/65536 hits=882 misses=258
        finished...

![image](https://yuncodeweb.oss-cn-hangzhou.aliyuncs.com/uploads/edward.yangx/public-docs/b9ea5662e866913efcaca28549d9f033/image.png)

- **make repo-list: 列出当前工程引用的外部组件和它们的云端仓库地址**

        LITE-utils $ make repo-list
        [external/log] git@gitlab.alibaba-inc.com:iot-middleware/LITE-log.git

> 以上输出表示在构建单元`external/log`, 引用了云端仓库位于`git@gitlab.alibaba-inc.com:iot-middleware/LITE-log.git`的外部组件`LITE-log`

- **make repo-update: 将所有引用的外部组件用云端仓库更新本地仓库**

        LITE-utils $ make repo-update
        [ LITE-log.git ] <= : git@gitlab.alibaba-inc.com:iot-middleware/LITE-log.git :: master
        + cd /home/edward/srcs/iot-middleware/LITE-utils/packages
        + rm -rf LITE-log.git
        + git clone -q --bare -b master --single-branch git@gitlab.alibaba-inc.com:iot-middleware/LITE-log.git LITE-log.git
        + rm -rf LITE-log.git/hooks/
        + mkdir -p LITE-log.git/hooks/
        + touch LITE-log.git/hooks/.gitkeep
        + touch LITE-log.git/refs/heads/.gitkeep LITE-log.git/refs/tags/.gitkeep
        + rm -rf /home/edward/srcs/iot-middleware/LITE-utils.pkgs/LITE-log
        + cd /home/edward/srcs/iot-middleware/LITE-utils
        + set +x

> 以上输出表示从`git@gitlab.alibaba-inc.com:iot-middleware/LITE-log.git`拉取最新的云端仓库, 更新到本地仓库`packages/LITE-log.git`

# <a name="构建系统的配置">构建系统的配置</a>

## <a name="定制目录排布">定制目录排布</a>

默认情况下, 构建系统采用如下的目录排布:

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
| `MAKE_SEGMENT`    | 构建单元中的片段文件, 开发者唯一需要编辑的文件, 指导构建系统如何编译该文件所在的构建单元 |
| `OUTPUT_DIR`      | 编译树中间目录 |
| `PACKAGE_DIR`     | 打包目录, 其中可以存放其它组件, 也可以存放压缩包形式的第三方软件, 例如`openssl-0.9.8.tar.gz`等 |
| `RULE_DIR`        | 构建系统核心目录, 如果不喜欢`build-rules`存放在自己的工程里, 完全可以通过这个变量把它移出去 |
| `TOP_DIR`         | 工程的顶级目录, 默认就是顶层`makefile`所在的路径, 极少改动 |

可以完全不对它们做任何改动, 依照上文的含义和默认值来排布自己工程

---
也可以通过顶层目录的`project.mk`, 可以改动它们, 例如:

    $ cat project.mk
    MAKE_SEGMENT := alink.mk

则表示在每个构建单元中, 指导构建过程的片段文件不再是默认的`iot.mk`, 而是`alink.mk`

# <a name="定制工程全局的编译行为">定制工程全局的编译行为</a>

## <a name="增加硬件平台配置文件">增加硬件平台配置文件</a>

在默认的`configs`目录, 或者在指定的`$(CONFIG_DIR)`目录下, 文件名形式为`config.<VENDOR>.<MODEL>`的文本文件, 会被构建系统认为是硬件平台配置文件, 每个文件对应一个嵌入式软硬件平台

* 其中<VENDOR>部分, 一般是指明嵌入式平台的软件OS提供方, 如`mxchip`, `ubuntu`, `win7`等. 另外, 这也会导致构建系统到`$(IMPORT_DIR)/<VENDOR>`目录下寻找预编译库的二进制库文件和头文件
* 其中<MODEL>部分, 一般是标明嵌入式平台的具体硬件型号, 如`mtk7687`, `qcom4004`等, 不过也可以写上其它信息, 因为构建系统不会去理解它, 比如`mingw32`, `x86-64`等

> 例如`config.mxchip.3080c`文件, 如果在`make reconfig`的时候被选择, 则会导致:

* 构建系统在`import/mxchip/`目录下寻找预编译库的二进制库文件和头文件
* 构建系统使用该文件内的变量指导编译行为, 具体来说, 可以根据说明使用如下变量

| 变量                  | 说明                                      |
|-----------------------|-------------------------------------------|
| `CONFIG_ENV_CFLAGS`   | 指定全局的`CFLAGS`编译选项, 传给`compiler`, 例如`CONFIG_ENV_CFLAGS += -DDEBUG` |
| `CONFIG_ENV_LDFLAGS`  | 指定全局的`LDFLAGS`链接选项, 传给`linker`, 例如`CONFIG_ENV_LDFLAGS += -lcrypto` |
| `CROSS_PREFIX`        | 指定交叉编译工具链共有的前缀, 例如`CROSS_PREFIX := arm-none-eabi-`, 会导致构建系统使用`arm-none-eabi-gcc`和`arm-none-eabi-ar`, 以及`arm-none-eabi-strip`等 |
| `OVERRIDE_CC`         | 当交叉工具链没有共有的前缀或者前缀不符合`prefix+gcc|ar|strip`类型时, 例如`armcc`, 可用`OVERRIDE_CC = armcc`单独指定C编译器 |
| `OVERRIDE_AR`         | 当交叉工具链没有共有的前缀或者前缀不符合`prefix+gcc|ar|strip`类型时, 例如`armar`, 可用`OVERRIDE_AR = armar`单独指定库压缩器 |
| `OVERRIDE_STRIP`      | 当交叉工具链没有共有的前缀或者前缀不符合`prefix+gcc|ar|strip`类型时, 例如`armcc`没有对应的strip程序, 可用`OVERRIDE_STRIP = true`单独指定strip程序不执行 |

## <a name="定制单元测试">定制单元测试</a>

如果你的工程含有配合构建系统的单元测试用例, 则可以在顶层`makefile`中添写类似如下语句, 告诉构建系统在`make test`命令中执行它们做单元测试, 并统计源码覆盖率

    UTEST_PROG := utils-tests

> 以上面的`LITE-utils`为例, 其中的`utils-tests`就是它的单元测试程序, 这是在构建单元`testsuites`中产生的一个`Linux`下可执行程序

    $ cat testsuites/iot.mk -n
         1  TARGET      := utils-tests
         2  DEPENDS     += external/cut src
         3  HDR_REFS    := src
         4
         5  LDFLAGS     += -llite-utils -llite-log -llite-cut

为什么如上的一个简单`iot.mk`片段文件, 就能指导构建系统生成可执行程序`utils-tests`, 可以访问[**构建单元说明**](#构建单元说明)

# <a name="构建单元说明">构建单元说明</a>

## <a name="定义">定义</a>

* 片段文件: 用来指导某个具体的文件夹下源码应如何获取和编译以及链接的makefile文本片段, 基本只包含变量的赋值
* 默认的片段文件是名为`iot.mk`的文本文件, 可以在工程顶层目录`project.mk`里, 用`MAKE_SEGMENT := <new>`更改片段文件名
* 从工程顶层目录以下, 每一个含有`iot.mk`的子目录, 都被构建系统认为是一个构建单元

> 举例来说:

> 假设在/path/to/project/project.mk文件中有
> 
>     MAKE_SEGEMENT := build.mk
 
> 那么, 如下的目录排布
> 
>     /path/to/project/bar/build.mk
>     /path/to/project/foo/sub1/build.mk
>     /path/to/project/foo/sub2/build.mk
>     /path/to/project/mmm/nnn/ppp/qqq/build.mk
 
> 会导致
> 
>     bar
>     foo/sub1
>     foo/sub2
>     mmm/nnn/ppp/qqq
 
> 都被认为是构建单元, 可以用类似`make bar`, `make foo/bar1`这样的命令单独编译, 也可以调试完成后通过修改顶层`makefile`集成到工程中

## <a name="工作过程">工作过程</a>

对整个工程进行构建的过程基本是遍历每个构建单元逐个构建, 而对每个构建单元的构建过程是:

* 构造编译的临时目录, 例如 `.O/bar` 目录, 对应源码 `bar` 构建单元
* 在编译的临时目录中动态生成或更新`makefile`和源代码
* 切换到临时目录中, 编译源代码, 产生目标代码

## <a name="开发过程">开发过程</a>

增加一个新的构建单元可以按如下步骤进行:

* 创建新单元对应的目录, 比如

        mkdir foo/bar

* 创建该单元的片段文件, 比如

        vi foo/bar/iot.mk
        1 LIBA_TARGET := libfoobar.a

* 为该单元添加一些源码文件, 比如

        cp .../*.c foo/bar/

* 这时, 在工程文件中运行`make reconfig`, 告诉构建系统新增了一个单元, 这个单元就可以被独立编译了

        make reconfig
        make foo/bar

* 单独的编译和运行没问题之后, 修改工程的`makefile`, 使这个新单元也加入到工程中

        vi makefile
        ...
        ...
        SUBDIRS += foo/bar
        ...
        ...

## <a name="编写片段文件">编写片段文件</a>

**简化编译的设计思想就是使每一个模块的开发者, 基本只需要编写片段文件就能构建自己的功能模块**

**而片段文件之所以能够起到简化的效果, 是因为它基本上只是一些"变量赋值"语句的集合, 编写它, 几乎可以不需要学习任何语言的任何语法**

---
以下是在片段文件中可以使用的变量名:

### <a name="输出相关">输出相关</a>

| 变量名            | 用处                                                                                          |
|-------------------|-----------------------------------------------------------------------------------------------|
| ORIGIN            | 如果你的模块希望按自己独特的方式编译, 比如`./configure ...; make; ...`, 需要设置此变量为1 |
| LIBSO_TARGET      | 设置本单元被编译出的`Linux`动态库文件名, 对它赋值导致系统帮助你产生`*.so`共享库文件 | 
| LIBA_TARGET       | 设置本单元被编译出的静态库文件名, 对它赋值导致系统帮助你产生`*.a`静态库文件 |
| TARGET            | 设置本单元被编译出的可执行程序文件名, 对它赋值导致系统帮助你产生`Linux`下的可执行程序 |
| KMOD_TARGET       | 设置本单元被编译出的内核模块文件名, 对它赋值导致系统帮助你产生`Linux`下的`*.ko`内核模块 |

**以上变量至少要有一个被设置, 这样构建系统才知道构建的输出是什么, 至多则可以同时被设置, 例如同时产生静态库文件和可执行程序, 甚至内核模块等**

### <a name="输入相关">输入相关</a>

| 变量名            | 用处                                                                                          |
|-------------------|-----------------------------------------------------------------------------------------------|
| LIB_SRCS          | 可选, 列出哪些`*.c`文件被编译成`*.a`或者`*.so` |
| SRCS              | 可选, 列出哪些`*.c`文件被编译成可执行程序 |
| EXTRA_SRCS        | 可选, 列出哪些源文件需要在开始编译之前被复制到临时的编译目录 |
| CFLAGS            | 可选, 列出需要特别应用在本模块上的编译选项 |
| LDFLAGS           | 可选, 列出需要特别应用在本模块上的链接选项 |

### <a name="组合相关">组合相关</a>

| 变量名            | 用处                                                                                          |
|-------------------|-----------------------------------------------------------------------------------------------|
| LIB_HEADERS       | 可选, 如果你的模块被编译完成提供一些头文件供其它模块使用, 可以在这个变量列出, 编译后会安装到系统目录 |
| LIB_HDRS_DIR      | 可选, 如果你的模块希望把被安装的头文件(`LIB_HEADERS`指定)安装到系统目录下的某个子目录, 可以用这个变量指定子目录的名字 |
| HDR_REFS          | 可选, 如果你的模块引用了其它模块的头文件, 那么需要设置这个变量, 列出被引用模块在工程里的相对路径 |
| DEPENDS           | 可选, 如果你的模块需要在其它模块构建完成后才能编译, 那么需要设置这个变量, 列出被依赖模块在工程里的相对路径 |

### <a name="高级用法">高级用法</a>

| 变量名            | 用处                                                                                          |
|-------------------|-----------------------------------------------------------------------------------------------|
| PKG_SOURCE        | 可选, 表示源码来自`*.c`之外的形式, 例如压缩包文件, 或者独立的git仓库等 |
| PKG_BRANCH        | 可选, 仅在`PKG_SOURCE`是git仓库的时候才有意义, 指定使用该仓库的哪个分支作为源码输入 |
| PKG_REVISION      | 可选, 仅在`PKG_SOURCE`是git仓库的时候才有意义, 指定使用该仓库的哪个分支上的哪个`commit`或者`tag` |
| PKG_UPSTREAM      | 可选, 仅在`PKG_SOURCE`是git仓库的时候才有意义, 指定使用哪个云端的git仓库地址来更新本地git仓库 |


### <a name="其它">其它</a>

#### <a name="产生多个可执行程序">产生多个可执行程序</a>

* 如果你的模块希望产生单个的可执行程序, 那么对变量`TARGET`和`SRCS`赋值就可以了, 但如果希望产生多个可执行程序, 比如

        TARGET = prog1 prog2 prog3

* 那么需要用`SRCS_<name>`变量对这多个可执行程序分别设定他们的源文件, 比如

        SRCS_prog1 = prog1.c
        SRCS_prog2 = prog2.c library_A.c
        SRCS_prog3 = prog3.c library_A.c library_B.c

* 目前除了`SRCS`变量, 没有其它变量支持这种带后缀的语法, 对多个可执行程序的`DEPENDS`, `LDFLAGS`等, 仍是整体指定

# <a name="构建系统的调试">构建系统的调试</a>

## <a name="调试开关">调试开关</a>

* 在`make ...`命令行中, 设置`TOP_Q`变量为空, 可打印工程顶层的执行逻辑, 例如硬件平台的选择, SDK主库的生成等

        make .... TOP_Q=

* 在`make ...`命令行中, 设置`Q`变量为空, 可打印模块内部的构建过程, 例如目标文件的生成, 头文件搜寻路径的组成等

        make .... Q=

## <a name="对单个构建单元调试">对单个构建单元调试</a>

* 可以用`make foo/bar`单独对`foo/bar`进行构建, 不过, 这可能需要先执行`make reconfig`
* 可以进入`.O/foo/bar`路径, 看到完整的编译临时目录, 有makefile和全部源码, 所以在这里执行`make`, 效果和`make foo/bar`等同
