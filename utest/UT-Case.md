# 如何编写测试例

**最简单的用法, 添加测试例只需用`CASE(suite_name, case_name)`定义测试例, 以及用`SUITE(suite_name)`定义测试集**

## 明确被测试的C函数

原则上每个测试例(case)调用和测试一个C函数, 测试例和被它测试的C函数需要写在同一个C文件里

> 举例来说, 有这样一个C函数需要测试, 它的输入是一个整数, 输出是这个整数的平方:

    int power(int arg) {
        return arg * arg;
    }

## 添加测试开关和包含头文件

单元测试函数都是会增大目标文件的尺寸的, 因此必须增设一个编译的开关, 能够统一的控制去掉它们, 比如

    #ifdef XXXX_SELF_TEST
    #include "cut.h"
    ...
    ...
    #endif  /* XXXX_SELF_TEST */

例如上面的例子, 在`#ifdef XXXX_SELF_TEST`和`#endif`之间, 按照下文去添加测试例和测试集

## 添加测试例

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

## 添加测试集

测试集是测试例的集合, 一般把功能相似的一些测试例放到一个测试集中, 语法是使用`SUITE()`宏, 比如

    SUITE(demo_suite) = {
        ADD_CASE(demo_suite, demo_case),
        ...
        ADD_CASE_NULL
    };

* 在这个测试集的例子中, `demo_suite`是测试集的名字, `demo_case`是被加入到该测试集中的测试例
* 结尾的`ADD_CASE_NULL`, 以及`}`后面的`:`是必须的

## 添加测试集入口

最后, 如果添加了一系列这样的测试集, 就可以在测试主程序中把这些入口都添加到`main()`之中, `cut_main()`调用之前

    int main(int argc, char *argv[]) {

        ADD_SUITE(demo_suite);
        ADD_SUITE(demo_suite1);
        ADD_SUITE(demo_suite2);

        ...
        cut_main(argc, argv);
    }

可以参考[单元测试框架](https://code.aliyun.com/edward.yangx/public-docs/wikis/utest/ut-framework)页面, 了解更多测试集如何进入测试主程序, 乃至构建系统的说明