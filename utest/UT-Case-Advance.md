# 测试例高级写法

编写测试用例时, 用基本的`SUITE()`宏和`CASE()`宏, 已经可以完成测试目的了, 不过, 也有一些更高级的接口可用, 能起到更加减少工作量的作用

**具体来说, 就是让多个测试例使用同一套数据结构, 实现测试数据被创建和销毁的代码在多个测试例之间的复用, 相应的, 也不再用`CASE()`而是`CASEs()`宏来定义测试例**

## DATA(suite_name)

这个宏的作用是创建一个结构体, 它后面的`{...}`里的内容就是结构体的成员变量, 这个结构体的实例被创建出来之后, 会在多个`CASEs()`宏定义的测试例间公用, 参考`LITE-utils`的例子:

    DATA(string)
    {
        char           *fmt;
        char           *str;
    };

> 如上的写法相当于定义了一个结构体, 它的成员有`fmt`和`str`

## SETUP(suite_name)

这个宏的作用是在每个`CASEs()`宏所创建的测试例开始的时候, 都把`DATA()`宏所定义的结构体实例创建出来, 用于下面的测试逻辑, 例如:

    SETUP(string)
    {
        data->fmt = cut_malloc(100);
        strcpy(data->fmt, "Integer: %d, String: '%s', Char: %c, Hex: %04x");
    }

> 如上的写法将导致在每个`CASEs()`定义的测试例开始运行的时候, 都有一个结构体实例被创建, 这个实例的变量名是`data`, 成员是`fmt`, 内容已经用`strcp()`填上了值

## CASEs(suite_name, case_name)

这个宏的作用是取代`CASE()`, 定义测试例, 不同的是, 它会固定在自己开始的地方调用`SETUP()`创建测试数据, 在自己结尾的地方调用`TEARDOWN()`销毁测试数据

    CASEs(string, format)
    {
        data->str = LITE_format_string(data->fmt, 100, "Hello", 'A', 640);
        log_info("(Length: %02d) '%s'", (int)strlen(data->str), data->str);

        ASSERT_EQ(49, strlen(data->str));
        ASSERT_STR_EQ(data->str, "Integer: 100, String: 'Hello', Char: A, Hex: 0280");
    }

> 如上的写法直接使用`SETUP()`时创建的测试数据`data`, 不需要额外的初始化, 结束时也不需要额外的销毁, 这些都是编写测试例的时候不再需要额外关注的了

## TEARDOWN(suite_name)

这个宏的作用是在每个`CASEs()`所定义的测试例结尾自动的执行, 把测试数据销毁掉, 比如执行结束线程, 回收内存等资源清理的动作, 例如:

    TEARDOWN(string)
    {
        cut_free(data->fmt);
        cut_free(data->str);
    }

> 如上的写法就是把结构体`data`里的两个成员指针所申请的内存, 用`free`释放掉

**总而言之, 这样的组织方式可以让多个相似的测试例, 复用同样的创建/销毁测试数据结构的代码, 用`CASEs()`编写测试例的时候, 可以只关注测试逻辑, 减少了工作量**