# <a name="目录">目录</a>
+ [如何使用测试框架](#如何使用测试框架)
    * [构建系统首先会列出当前已经编写的测试例](#构建系统首先会列出当前已经编写的测试例)
    * [构建系统接着会运行这些测试例和统计通过情况](#构建系统接着会运行这些测试例和统计通过情况)
    * [构建系统最后打印被执行的测试例对源代码的覆盖率](#构建系统最后打印被执行的测试例对源代码的覆盖率)
+ [如何装备测试框架](#如何装备测试框架)
    * [使用最新的构建系统](#使用最新的构建系统)
    * [构建测试框架库](#构建测试框架库)
        - [创建构建单元](#创建构建单元)
        - [尝试编译测试框架的库](#尝试编译测试框架的库)
    * [构建测试主程序](#构建测试主程序)
        - [创建构建单元](#创建构建单元)
        - [添加测试集到主程序](#添加测试集到主程序)
    * [结合测试主程序和构建系统](#结合测试主程序和构建系统)
+ [如何编写测试例](#如何编写测试例)
    * [明确被测试的C函数](#明确被测试的C函数)
    * [添加测试开关和包含头文件](#添加测试开关和包含头文件)
    * [添加测试例](#添加测试例)
    * [添加测试集](#添加测试集)
    * [添加测试集入口](#添加测试集入口)
+ [测试例高级写法](#测试例高级写法)
    * [DATA(suite_name)](#DATA(suite_name))
    * [SETUP(suite_name)](#SETUP(suite_name))
    * [CASEs(suite_name, case_name)](#CASEs(suite_name, case_name))
    * [TEARDOWN(suite_name)](#TEARDOWN(suite_name))

# <a name="如何使用测试框架">如何使用测试框架</a>

在工程的顶层目录运行命令`make test`, 就会执行`UTEST_PROG`变量指定的单元测试主程序, 并统计通过率和覆盖率, 以`LITE-utils`工程为例:

    $ make test
    BUILDING WITH EXISTING CONFIGURATION:

    VENDOR :   generic
    MODEL  :   default
    ...
    ...

## <a name="构建系统首先会列出当前已经编写的测试例">构建系统首先会列出当前已经编写的测试例</a>

    [0] RUNNING './utils-tests --list' with './utils-tests'

      [01] string.hexbuf_convert
      [02] string.hexstr_convert
      [03] string.format
      [04] string.format_nstring
      [05] string.format_nstring_ext
      [06] string.always_failure

    In total 6 case(s), matched 6 case(s)

其中的`string`是测试集的名字, 后面`hexbuf_convert`, `hexstr_convert`等等是测试例的名字, 测试例组成了测试集, 一般从测试例的名字可以看出它所测试的C函数

## <a name="构建系统接着会运行这些测试例和统计通过情况">构建系统接着会运行这些测试例和统计通过情况</a>

    [1] RUNNING './utils-tests' with './utils-tests'

    TEST [01/06] string.hexbuf_convert .............................................. [SUCC]
    TEST [02/06] string.hexstr_convert .............................................. [SUCC]
    TEST [03/06] string.format ...................................................... [SUCC]
    TEST [04/06] string.format_nstring .............................................. [SUCC]
    TEST [05/06] string.format_nstring_ext .......................................... [SUCC]
    TEST [06/06] string.always_failure .............................................. [FAIL]
    ===========================================================================
    FAIL LIST:
      [01] string.always_failure in string_utils.c(252) expected [True]
    ---------------------------------------------------------------------------
    SUMMARY:
         TOTAL:    6
       SKIPPED:    0
       MATCHED:    6
          PASS:    5
        FAILED:    1
    ===========================================================================

其中以`[SUCC]`表示测试例通过了, 被测试的C函数执行结果符合预期, 以`[FAIL]`表示测试例没通过, 被测试的C函数执行结果不符合预期, 结尾的表格汇总了失败的信息和统计通过情况

## <a name="构建系统最后打印被执行的测试例对源代码的覆盖率">构建系统最后打印被执行的测试例对源代码的覆盖率</a>

    Processing [/home/edward/srcs/iot-middleware/LITE-utils/output/Coverage] for Coverage Brief

           Directory   Source File                 : Line Coverage            Function Coverage   

                 src / string_utils.c              : [ 80.29%  ] (110/137)    [ 93.33%  ] (14/15)     
          testsuites / utils-tests.c               : [ 77.77%  ] (7/9)        [ 100.00% ] (1/1)       
                 cut / cut.c                       : [ 77.33%  ] (58/75)      [ 75.00%  ] (3/4)       
                 src / mem_stats.c                 : [ 30.00%  ] (6/20)       [ 16.66%  ] (1/6)       
            LITE-log / lite-log.c                  : [ 23.46%  ] (23/98)      [ 60.00%  ] (6/10)      
                 src / json_token.c                : [ 0%      ] (0/265)      [ 0%      ] (0/7)       
                 src / json_parser.c               : [ 0%      ] (0/119)      [ 0%      ] (0/5)       
             example / utils-example.c             : [ 0%      ] (0/10)       [ 0%      ] (0/1)       
             example / utils-api_demo.c            : [ 0%      ] (0/87)       [ 0%      ] (0/5)       

其中`Directory`和`Source File`合起来表示源文件的最后一级目录名和文件名, `Line Coverage`是行覆盖率, 表示该源文件总行数中的多少行在测试例中被执行到了, `Function Coverage`是函数覆盖率, 表示该源文件中所有函数有多少个函数被执行到

最后, 如果在`[/home/edward/srcs/iot-middleware/LITE-utils/output/Coverage]`目录中, 用图形化界面打开`index.html`网页, 还可以具体看到是哪些行, 哪些函数被覆盖了, 哪些行, 哪些函数没有被覆盖到, 可帮助逐步提高覆盖率

# <a name="如何装备测试框架">如何装备测试框架</a>

## <a name="使用最新的构建系统">使用最新的构建系统</a>

参考[构建系统手册](https://code.aliyun.com/edward.yangx/public-docs/wikis/user-guide/Build_System_Manual)页面, 了解如何获取最新的构建系统到你的项目中, 这是装备测试框架的前提

**也可以访问样板组件[LITE-utils](http://gitlab.alibaba-inc.com/iot-middleware/LITE-utils)的仓库, 其中测试框架和测试例都已经就绪, 可以直接体验和观察单元测试框架的工作方式和效果**

    $ git clone git@gitlab.alibaba-inc.com:iot-middleware/LITE-utils.git

## <a name="构建测试框架库">构建测试框架库</a>

按照如下步骤, 新建一个构建单元, 用它把测试框架库编译出来, 其实本质就是把`build-rules/misc/cut.c`编译成一个`liblite-cut.a`或者`liblite-cut.so`的库, 供测试主程序链接

### <a name="创建构建单元">创建构建单元</a>

参考[构建系统手册](https://code.aliyun.com/edward.yangx/public-docs/wikis/user-guide/Build_System_Manual)页面, 可以按如下方式创建

    $ mkdir -p external/cut
    $ vi external/cut/iot.mk
    1  LIBA_TARGET     := liblite-cut.a
    2
    3  LIB_HEADERS     := cut.h
    4  EXTRA_SRCS      := $(RULE_DIR)/misc/cut.[ch]

* 片段文件`iot.mk`告诉构建系统如何编译框架库, 这个文件的文件名按照你工程中的`$(MAKE_SEGMENT)`变量来命名, 如果没有在`project.mk`中重定义, 默认是`iot.mk`
* 假设按照上面的例子来创建, 则构建单元的名字就是`external/cut`, 提供的库文件就是`liblite-cut.a`, 头文件就是`cut.h`, 测试主程序所在的构建单元将依赖它们

### <a name="尝试编译测试框架的库">尝试编译测试框架的库</a>
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

## <a name="构建测试主程序">构建测试主程序</a>

所谓测试主程序其实就是一个简单的可执行程序, 它逐个的执行所有的测试例, 并且统计和展示这些测试例的执行结果

### <a name="创建构建单元">创建构建单元</a>

参考[构建单元手册](https://code.aliyun.com/edward.yangx/public-docs/wikis/user-guide/Build_System_Manual)页面, 可以按如下方式创建, **注意不要使用`test`作为名字**

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

### <a name="添加测试集到主程序">添加测试集到主程序</a>

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

## <a name="结合测试主程序和构建系统">结合测试主程序和构建系统</a>

参考[构建工程说明](https://code.aliyun.com/edward.yangx/public-docs/wikis/build/build-system-proj)页面, 了解如何把你的测试主程序和构建系统联系起来

---
在上面的例子中, 可以编辑工程的顶层`makefile`, 添加如下语句

    UTEST_PROG := utest-prog

**之后用`make test`命令, 就可以执行测试主程序`utest-prog`, 执行测试集中的测试例, 统计和展示结果, 并统计测试例对代码的覆盖率**

# <a name="如何编写测试例">如何编写测试例</a>

**最简单的用法, 添加测试例只需用`CASE(suite_name, case_name)`定义测试例, 以及用`SUITE(suite_name)`定义测试集**

## <a name="明确被测试的C函数">明确被测试的C函数</a>

原则上每个测试例(case)调用和测试一个C函数, 测试例和被它测试的C函数需要写在同一个C文件里

> 举例来说, 有这样一个C函数需要测试, 它的输入是一个整数, 输出是这个整数的平方:

    int power(int arg) {
        return arg * arg;
    }

## <a name="添加测试开关和包含头文件">添加测试开关和包含头文件</a>

单元测试函数都是会增大目标文件的尺寸的, 因此必须增设一个编译的开关, 能够统一的控制去掉它们, 比如

    #ifdef XXXX_SELF_TEST
    #include "cut.h"
    ...
    ...
    #endif  /* XXXX_SELF_TEST */

例如上面的例子, 在`#ifdef XXXX_SELF_TEST`和`#endif`之间, 按照下文去添加测试例和测试集

## <a name="添加测试例">添加测试例</a>

现在我们给被测试的函数编写测试例, 其核心的逻辑就是提供入参调用该函数, 然后获取返回值和出参, 用`ASSERT_*()`宏来比较返回值或出参是否符合预期

> 以上面的例子, 我们提供一个入参为整数5, 测试返回值是否是符合期望的5*5 = 25

    CASE(demo_suite, demo_case) {
        int     result = 0;

        result = power(5);
        ASSERT_EQ(result, 25);
    }

* 在这个测试例中, `demo_suite`是它所归属的测试集的名字, `demo_case`是它自身的名字
* 判断被测试的函数是否工作的符合预期很简单, 获取返回值, 然后用`ASSERT_EQ`, 判断返回值是否等于25, 所有这样的断言宏可以看`cut.h`的如下部分

        #define ASSERT_FAIL()                       RAISE_EXCEPTION_WITH_MSG("[should not be here]")
        #define ASSERT_FALSE(cond)                  ASSERT_TRUE(!(cond))
        #define ASSERT_NULL(ptr)                    ASSERT_INT(ptr, ==, NULL)
        #define ASSERT_NOT_NULL(ptr)                ASSERT_INT(ptr, !=, NULL)
        #define ASSERT_EQ(actual, expected)         ASSERT_INT(actual, ==, expected)
        #define ASSERT_NE(actual, expected)         ASSERT_INT(actual, !=, expected)
        #define ASSERT_GT(actual, expected)         ASSERT_INT(actual,  >, expected)
        #define ASSERT_GE(actual, expected)         ASSERT_INT(actual, >=, expected)
        #define ASSERT_LT(actual, expected)         ASSERT_INT(actual,  <, expected)
        #define ASSERT_LE(actual, expected)         ASSERT_INT(actual, <=, expected)
        #define ASSERT_STR_EQ(actual, expected)     ASSERT_STR(actual, ==, expected)
        #define ASSERT_STR_NE(actual, expected)     ASSERT_STR(actual, !=, expected)
        #define ASSERT_STR_GT(actual, expected)     ASSERT_STR(actual,  >, expected)
        #define ASSERT_STR_LT(actual, expected)     ASSERT_STR(actual,  <, expected)

## <a name="添加测试集">添加测试集</a>

测试集是测试例的集合, 一般把功能相似的一些测试例放到一个测试集中, 语法是使用`SUITE()`宏, 比如

    SUITE(demo_suite) = {
        ADD_CASE(demo_suite, demo_case),
        ...
        ADD_CASE_NULL
    };

* 在这个测试集的例子中, `demo_suite`是测试集的名字, `demo_case`是被加入到该测试集中的测试例
* 结尾的`ADD_CASE_NULL`, 以及`}`后面的`:`是必须的

## <a name="添加测试集入口">添加测试集入口</a>

最后, 如果添加了一系列这样的测试集, 就可以在测试主程序中把这些入口都添加到`main()`之中, `cut_main()`调用之前

    int main(int argc, char *argv[]) {

        ADD_SUITE(demo_suite);
        ADD_SUITE(demo_suite1);
        ADD_SUITE(demo_suite2);

        ...
        cut_main(argc, argv);
    }

可以参考[如何装备测试框架](#如何装备测试框架), 了解更多测试集如何进入测试主程序, 乃至构建系统的说明

# <a name="测试例高级写法">测试例高级写法</a>

编写测试用例时, 用基本的`SUITE()`宏和`CASE()`宏, 已经可以完成测试目的了, 不过, 也有一些更高级的接口可用, 能起到更加减少工作量的作用

**具体来说, 就是让多个测试例使用同一套数据结构, 实现测试数据被创建和销毁的代码在多个测试例之间的复用, 相应的, 也不再用`CASE()`而是`CASEs()`宏来定义测试例**

## <a name="DATA(suite_name)">DATA(suite_name)</a>

这个宏的作用是创建一个结构体, 它后面的`{...}`里的内容就是结构体的成员变量, 这个结构体的实例被创建出来之后, 会在多个`CASEs()`宏定义的测试例间公用, 参考`LITE-utils`的例子:

    DATA(string)
    {
        char           *fmt;
        char           *str;
    };

> 如上的写法相当于定义了一个结构体, 它的成员有`fmt`和`str`

## <a name="SETUP(suite_name)">SETUP(suite_name)</a>

这个宏的作用是在每个`CASEs()`宏所创建的测试例开始的时候, 都把`DATA()`宏所定义的结构体实例创建出来, 用于下面的测试逻辑, 例如:

    SETUP(string)
    {
        data->fmt = cut_malloc(100);
        strcpy(data->fmt, "Integer: %d, String: '%s', Char: %c, Hex: %04x");
    }

> 如上的写法将导致在每个`CASEs()`定义的测试例开始运行的时候, 都有一个结构体实例被创建, 这个实例的变量名是`data`, 成员是`fmt`, 内容已经用`strcp()`填上了值

## <a name="CASEs(suite_name, case_name)">CASEs(suite_name, case_name)</a>

这个宏的作用是取代`CASE()`, 定义测试例, 不同的是, 它会固定在自己开始的地方调用`SETUP()`创建测试数据, 在自己结尾的地方调用`TEARDOWN()`销毁测试数据

    CASEs(string, format)
    {
        data->str = LITE_format_string(data->fmt, 100, "Hello", 'A', 640);
        log_info("(Length: %02d) '%s'", (int)strlen(data->str), data->str);

        ASSERT_EQ(49, strlen(data->str));
        ASSERT_STR_EQ(data->str, "Integer: 100, String: 'Hello', Char: A, Hex: 0280");
    }

> 如上的写法直接使用`SETUP()`时创建的测试数据`data`, 不需要额外的初始化, 结束时也不需要额外的销毁, 这些都是编写测试例的时候不再需要额外关注的了

## <a name="TEARDOWN(suite_name)">TEARDOWN(suite_name)</a>

这个宏的作用是在每个`CASEs()`所定义的测试例结尾自动的执行, 把测试数据销毁掉, 比如执行结束线程, 回收内存等资源清理的动作, 例如:

    TEARDOWN(string)
    {
        cut_free(data->fmt);
        cut_free(data->str);
    }

> 如上的写法就是把结构体`data`里的两个成员指针所申请的内存, 用`free`释放掉

**总而言之, 这样的组织方式可以让多个相似的测试例, 复用同样的创建/销毁测试数据结构的代码, 用`CASEs()`编写测试例的时候, 可以只关注测试逻辑, 减少了工作量**
