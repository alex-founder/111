# 如何使用测试框架

在工程的顶层目录运行命令`make test`, 就会执行`UTEST_PROG`变量指定的单元测试主程序, 并统计通过率和覆盖率, 以`LITE-utils`工程为例:

    $ make test
    BUILDING WITH EXISTING CONFIGURATION:

    VENDOR :   generic
    MODEL  :   default
    ...
    ...

## 构建系统首先会列出当前已经编写的测试例:

    [0] RUNNING './utils-tests --list' with './utils-tests'

      [01] string.hexbuf_convert
      [02] string.hexstr_convert
      [03] string.format
      [04] string.format_nstring
      [05] string.format_nstring_ext
      [06] string.always_failure

    In total 6 case(s), matched 6 case(s)

其中的`string`是测试集的名字, 后面`hexbuf_convert`, `hexstr_convert`等等是测试例的名字, 测试例组成了测试集, 一般从测试例的名字可以看出它所测试的C函数

## 构建系统接着会运行这些测试例和统计通过情况

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

## 构建系统最后打印被执行的测试例对源代码的覆盖率

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