# 如何获取

* 可访问样板组件`LITE-utils`的仓库: [*git@gitlab.alibaba-inc.com:iot-middleware/LITE-utils.git*](http://gitlab.alibaba-inc.com/iot-middleware/LITE-utils)
* 从`master`分支的`build-rules`目录复制得到编译系统的最新副本
* 也可以直接在`LITE-utils`中体验编译系统的工作方式和观察工作过程

# 常用命令

| 命令                  | 解释                                                                              |
|-----------------------|-----------------------------------------------------------------------------------|
| `make distclean`      | **清除一切编译过程产生的中间文件, 使当前目录仿佛和刚刚clone下来一样**             |
| `make [all]`          | **使用默认的平台配置文件开始编译**                                                |
| `make reconfig`       | **弹出多平台选择菜单, 用户可按数字键选择, 然后根据相应的硬件平台配置开始编译**    |

# 高级命令

| 命令                  | 解释                                                                                      |
|-----------------------|-------------------------------------------------------------------------------------------|
| `make test`           | **运行指定的测试集程序, 统计显示测试例的通过率和源代码的覆盖率**                          |
| `make doc`            | **扫描源码目录中以`Doxygen`格式或者`Markdown`格式编写的注释, 在`html`目录产生帮助文档**   |
| `make repo-list`      | **列出当前工程引用的外部组件和它们的云端仓库地址**                                        |
| `make repo-update`    | **对所有引用到的外部组件, 用云端仓库更新本地仓库**                                        |

# 高级命令示例

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