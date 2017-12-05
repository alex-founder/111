# 如何获取

* 可访问样板组件`LITE-utils`的仓库: [*git@gitlab.alibaba-inc.com:iot-middleware/LITE-utils.git*](http://gitlab.alibaba-inc.com/iot-middleware/LITE-utils)
* 从`master`分支的`build-rules`目录复制得到编译系统的最新副本
* 也可以直接在`LITE-utils`中体验编译系统的工作方式和观察工作过程

# 常用命令

    make distclean:         清除一切编译过程产生的中间文件, 使当前目录仿佛和刚刚clone下来一样
    make [all]:             使用默认的平台配置文件开始编译
    make reconfig:          弹出多平台选择菜单, 用户可按数字键选择, 然后根据相应的硬件平台配置开始编译

# 高级命令

- **make test: 运行指定的测试集程序, 统计显示测试例的通过率和源代码的覆盖率**

         LITE-utils $ make test
         
         ...
         ...

         [0] RUNNING './utils-tests --list' with './utils-tests'

           [01] string.hexbuf_convert
           [02] string.hexstr_convert
           [03] string.format
           [04] string.format_nstring
           [05] string.format_nstring_ext
           [06] string.always_failure

         In total 6 case(s), matched 6 case(s)

         [1] RUNNING './utils-tests' with './utils-tests'

         TEST [01/06] string.hexbuf_convert .............................................. [SUCC]
         TEST [02/06] string.hexstr_convert .............................................. [SUCC]
         TEST [03/06] string.format ...................................................... [SUCC]
         TEST [04/06] string.format_nstring .............................................. [SUCC]
         TEST [05/06] string.format_nstring_ext .......................................... [SUCC]
         TEST [06/06] string.always_failure .............................................. [FAIL]
         ===========================================================================
         FAIL LIST:
           [01] string.always_failure in string_utils.c(246) expected [True]
         ---------------------------------------------------------------------------
         SUMMARY:
              TOTAL:    6
            SKIPPED:    0
            MATCHED:    6
               PASS:    5
             FAILED:    1
         ===========================================================================

         Processing [/home/edward/srcs/iot-middleware/LITE-utils/output/Coverage] for Coverage Brief

                Directory   Source File                 : Line Coverage            Function Coverage   

                      src / string_utils.c              : [ 81.20%  ] (108/133)    [ 93.33%  ] (14/15)     
               testsuites / utils_tests.c               : [ 77.77%  ] (7/9)        [ 100.00% ] (1/1)       
                      cut / cut.c                       : [ 77.33%  ] (58/75)      [ 75.00%  ] (3/4)       
                      src / mem_stats.c                 : [ 30.00%  ] (6/20)       [ 16.66%  ] (1/6)       
                 LITE-log / lite-log.c                  : [ 23.46%  ] (23/98)      [ 60.00%  ] (6/10)      
                      src / lite-utils_testsuites.c     : [ 0%      ] (0/63)       [ 0%      ] (0/3)       
                      src / json_token.c                : [ 0%      ] (0/62)       [ 0%      ] (0/3)       
                      src / json_parser.c               : [ 0%      ] (0/119)      [ 0%      ] (0/5)       
                  example / utils-example.c             : [ 0%      ] (0/8)        [ 0%      ] (0/1)       

- **make doc: 扫描源码目录中以`Doxygen`格式或者`Markdown`格式编写的注释, 在`html`目录产生帮助文档**

        LITE-utils $ make doc
        ...
        ...
        Patching output file 15/16
        Patching output file 16/16
        lookup cache used 256/65536 hits=882 misses=258
        finished...

- **make repo-list: 列出当前工程引用的外部组件和它们的云端仓库地址**

        LITE-utils $ make repo-list
        [external/log] git@gitlab.alibaba-inc.com:iot-middleware/LITE-log.git

> 以上输出表示在编译单元`external/log`, 引用了云端仓库位于`git@gitlab.alibaba-inc.com:iot-middleware/LITE-log.git`的外部组件`LITE-log`

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