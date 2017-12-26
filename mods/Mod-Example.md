# 组件化示例

## 组件化MQTT模块

### 目录
+ 一. 创建仓库
+ 二. 在项目中使用组件仓库
+ 三. 维护被组件化的代码

### 一. 创建仓库

+ 访问组件仓库: [http://gitlab.alibaba-inc.com/groups/iot-middleware](http://gitlab.alibaba-inc.com/groups/iot-middleware), 点击`+NEW PROJECT`按钮, 创建线上仓库

![image](https://yuncodeweb.oss-cn-hangzhou.aliyuncs.com/uploads/edward.yangx/public-docs/69179f7a41dc7a3ff2028fd50483d4cc/image.png)

+ 选择`Public`访问权限, 决定组件的名字, 比如`Link-MQTT`, 点击`+CREATE PROJECT`按钮

> 这样就形成了地址为**git@gitlab.alibaba-inc.com:iot-middleware/Link-MQTT.git**的线上仓库, 以后用来存放组件的源代码

+ 在本地Linux机器上完成仓库的首次push

> 假设我们现在本地Linux机器上, 在`~/srcs/iotx-sdk-c/src/mqtt`目录存放了组件的源代码, 要把它们推送到线上

> 首先用`mkdir`创建一个目录, 作为`git push`时的工作目录, 把需要的文件复制进来

        edward @ ~/temp$ mkdir Link-MQTT

        edward @ ~/temp$ cp -rvf ~/srcs/iotx-sdk-c/src/mqtt/* Link-MQTT/
        '/home/edward/srcs/iotx-sdk-c/src/mqtt/iot.mk' -> 'Link-MQTT/iot.mk'
        '/home/edward/srcs/iotx-sdk-c/src/mqtt/mqtt_client.c' -> 'Link-MQTT/mqtt_client.c'
        '/home/edward/srcs/iotx-sdk-c/src/mqtt/mqtt_client.h' -> 'Link-MQTT/mqtt_client.h'
        '/home/edward/srcs/iotx-sdk-c/src/mqtt/MQTTPacket' -> 'Link-MQTT/MQTTPacket'
        '/home/edward/srcs/iotx-sdk-c/src/mqtt/MQTTPacket/MQTTConnect.h' -> 'Link-MQTT/MQTTPacket/MQTTConnect.h'
        '/home/edward/srcs/iotx-sdk-c/src/mqtt/MQTTPacket/MQTTPacket.c' -> 'Link-MQTT/MQTTPacket/MQTTPacket.c'
        '/home/edward/srcs/iotx-sdk-c/src/mqtt/MQTTPacket/MQTTConnectClient.c' -> 'Link-MQTT/MQTTPacket/MQTTConnectClient.c'
        '/home/edward/srcs/iotx-sdk-c/src/mqtt/MQTTPacket/MQTTDeserializePublish.c' -> 'Link-MQTT/MQTTPacket/MQTTDeserializePublish.c'
        '/home/edward/srcs/iotx-sdk-c/src/mqtt/MQTTPacket/MQTTPacket.h' -> 'Link-MQTT/MQTTPacket/MQTTPacket.h'
        '/home/edward/srcs/iotx-sdk-c/src/mqtt/MQTTPacket/MQTTPublish.h' -> 'Link-MQTT/MQTTPacket/MQTTPublish.h'
        '/home/edward/srcs/iotx-sdk-c/src/mqtt/MQTTPacket/MQTTSerializePublish.c' -> 'Link-MQTT/MQTTPacket/MQTTSerializePublish.c'
        '/home/edward/srcs/iotx-sdk-c/src/mqtt/MQTTPacket/MQTTSubscribe.h' -> 'Link-MQTT/MQTTPacket/MQTTSubscribe.h'
        '/home/edward/srcs/iotx-sdk-c/src/mqtt/MQTTPacket/MQTTSubscribeClient.c' -> 'Link-MQTT/MQTTPacket/MQTTSubscribeClient.c'
        '/home/edward/srcs/iotx-sdk-c/src/mqtt/MQTTPacket/MQTTUnsubscribe.h' -> 'Link-MQTT/MQTTPacket/MQTTUnsubscribe.h'
        '/home/edward/srcs/iotx-sdk-c/src/mqtt/MQTTPacket/MQTTUnsubscribeClient.c' -> 'Link-MQTT/MQTTPacket/MQTTUnsubscribeClient.c'

> 删除多余的文件

        edward @ ~/temp$ rm -vf Link-MQTT/iot.mk 
        removed 'Link-MQTT/iot.mk'

> 进入工作目录, 创建`git`工作的基础

        edward @ ~/temp$ cd Link-MQTT/
        edward @ ~/temp/Link-MQTT$ git init
        Initialized empty Git repository in /home/edward/temp/Link-MQTT/.git/
        edward @ ~/temp/Link-MQTT$ git remote add origin git@gitlab.alibaba-inc.com:iot-middleware/Link-MQTT.git

> 形成初始`commit`, 推送到线上仓库

        edward @ ~/temp/Link-MQTT$ git add -A
        edward @ ~/temp/Link-MQTT$ git commit -m 'init'
        [master (root-commit) 19f1227] init
         13 files changed, 4135 insertions(+)
         create mode 100644 MQTTPacket/MQTTConnect.h
         create mode 100644 MQTTPacket/MQTTConnectClient.c
         create mode 100644 MQTTPacket/MQTTDeserializePublish.c
         create mode 100644 MQTTPacket/MQTTPacket.c
         create mode 100644 MQTTPacket/MQTTPacket.h
         create mode 100644 MQTTPacket/MQTTPublish.h
         create mode 100644 MQTTPacket/MQTTSerializePublish.c
         create mode 100644 MQTTPacket/MQTTSubscribe.h
         create mode 100644 MQTTPacket/MQTTSubscribeClient.c
         create mode 100644 MQTTPacket/MQTTUnsubscribe.h
         create mode 100644 MQTTPacket/MQTTUnsubscribeClient.c
         create mode 100644 mqtt_client.c
         create mode 100644 mqtt_client.h
        edward @ ~/temp/Link-MQTT$ git push -u origin master
        Counting objects: 16, done.
        Delta compression using up to 2 threads.
        Compressing objects: 100% (16/16), done.
        Writing objects: 100% (16/16), 26.63 KiB | 0 bytes/s, done.
        Total 16 (delta 7), reused 0 (delta 0)
        To git@gitlab.alibaba-inc.com:iot-middleware/Link-MQTT.git
         * [new branch]      master -> master
        Branch master set up to track remote branch master from origin.

> 最后可以验证一下自己创建的线上仓库是否已经可以正常工作了

        edward @ ~/temp/Link-MQTT$ cd /tmp
        edward @ /tmp$ git clone git@gitlab.alibaba-inc.com:iot-middleware/Link-MQTT.git
        Cloning into 'Link-MQTT'...
        remote: Counting objects: 16, done.
        remote: Total 16 (delta 0), reused 0 (delta 0)
        Receiving objects: 100% (16/16), 35.78 KiB | 0 bytes/s, done.
        Checking connectivity... done.
        edward @ /tmp$ ls Link-MQTT/
        mqtt_client.c  mqtt_client.h  MQTTPacket

### 二. 在项目仓库中使用组件仓库

> 在这个例子中, 我们假设有`IoT套件项目`存放在`~/srcs/iotx-sdk-c`目录, 其中的`src/mqtt`目录, 需要替换成上面刚刚创建的`Link-MQTT`

+ 进入项目目录, 删除与`Link-MQTT`组件中重复的文件

        edward @ ~/srcs/iotx-sdk-c/src/mqtt$ ls
        iot.mk  mqtt_client.c  mqtt_client.h  MQTTPacket
        edward @ ~/srcs/iotx-sdk-c/src/mqtt$ rm -rf MQTTPacket/ mqtt_client.c mqtt_client.h
        edward @ ~/srcs/iotx-sdk-c/src/mqtt$ ls
        iot.mk

+ 改写片段文件, 填写线上地址, 这里用`PKG_SOURCE`变量指定组件在本地存放的裸仓库目录名, 用`PKG_UPSTREAM`变量指定组件的线上地址

        edward @ ~/srcs/iotx-sdk-c$ cat -n src/mqtt/iot.mk 
             1  LIBA_TARGET     := libiot_mqtt.a
             2  HDR_REFS        := src
             3  DEPENDS         := src/log src/utils
             4
             5  PKG_SOURCE      := Link-MQTT.git
             6  PKG_UPSTREAM    := git@gitlab.alibaba-inc.com:iot-middleware/Link-MQTT.git

+ 到项目顶层目录, 运行`make reconfig`, 再运行`make repo-list`, 验证一下被填写的组件地址已经被编译系统成功识别

        edward @ ~/srcs/iotx-sdk-c$ make reconfig
        SELECT A CONFIGURATION:

        1) config.ubuntu.x86
        2) config.win7.mingw32
        #? 1

        SELECTED CONFIGURATION:

        VENDOR :   ubuntu
        MODEL  :   x86


        CONFIGURE .............................. [sample]
        CONFIGURE .............................. [src/coap]
        CONFIGURE .............................. [src/guider]
        CONFIGURE .............................. [src/http]
        CONFIGURE .............................. [src/log]
        CONFIGURE .............................. [src/mqtt]
        CONFIGURE .............................. [src/ota]
        CONFIGURE .............................. [src/platform]
        CONFIGURE .............................. [src/sdk-impl]
        CONFIGURE .............................. [src/sdk-tests]
        CONFIGURE .............................. [src/shadow]
        CONFIGURE .............................. [src/subdev]
        CONFIGURE .............................. [src/system]
        CONFIGURE .............................. [src/tfs]
        CONFIGURE .............................. [src/tls]
        CONFIGURE .............................. [src/utils]

        edward @ ~/srcs/iotx-sdk-c$ make repo-list
        [src/mqtt] git@gitlab.alibaba-inc.com:iot-middleware/Link-MQTT.git

+ 接着运行`make repo-update`, 它会把线上的组件仓库更新到本地, 使得以后的编译过程不需要联网

        edward @ ~/srcs/iotx-sdk-c$ make repo-update
        [ Link-MQTT.git ] <= : git@gitlab.alibaba-inc.com:iot-middleware/Link-MQTT.git :: master
        + cd /home/edward/srcs/iotx-sdk-c/src/packages
        + rm -rf Link-MQTT.git
        + git clone -q --bare -b master --single-branch git@gitlab.alibaba-inc.com:iot-middleware/Link-MQTT.git Link-MQTT.git
        + rm -rf Link-MQTT.git/hooks/
        + mkdir -p Link-MQTT.git/hooks/
        + touch Link-MQTT.git/hooks/.gitkeep
        + touch Link-MQTT.git/refs/heads/.gitkeep Link-MQTT.git/refs/tags/.gitkeep
        + rm -rf /home/edward/srcs/iotx-sdk-c.pkgs/Link-MQTT
        + cd /home/edward/srcs/iotx-sdk-c
        + set +x
        edward @ ~/srcs/iotx-sdk-c$ ls src/packages/Link-MQTT.git/
        branches  config  description  HEAD  hooks  info  objects  packed-refs  refs

+ 这样就完成了组件仓库`src/packages/Link-MQTT.git`的创建, 可以用`make src/mqtt`编译它一下, 验证已经可以单独编译

        edward @ ~/srcs/iotx-sdk-c$ make src/mqtt      
        BUILDING WITH EXISTING CONFIGURATION:

        VENDOR :   ubuntu
        MODEL  :   x86

        [CC] json_token.o               <= json_token.c                                    
                                           makefile
                                           lite-utils_internal.h
                                           lite-utils_config.h
                                           lite-utils.h
                                           lite-list.h
                                           json_parser.h
        [CC] json_parser.o              <= json_parser.c                  
        ...
        ...
        [CC] mqtt_client.o              <= mqtt_client.c                                    
                                           makefile
                                           MQTTPacket.h
                                           MQTTConnect.h
                                           MQTTPublish.h
                                           MQTTSubscribe.h
                                           MQTTUnsubscribe.h
                                           mqtt_client.h
        [AR] libiot_mqtt.a              <= mqtt_client.o     

        edward @ ~/srcs/iotx-sdk-c$ ls .O/usr/lib/
        libiot_log.a  libiot_mqtt.a  libiot_utils.a

### 提交自己的组件化改动到项目仓库

> 组件化改动到的目录主要是`src/mqtt`和`src/packages`, 用`git add`让它们进入提交范围

        edward @ ~/srcs/iotx-sdk-c$ git add -A src/mqtt
        edward @ ~/srcs/iotx-sdk-c$ git add src/packages/ -A
        edward @ ~/srcs/iotx-sdk-c$ git status
        On branch develop
        Your branch is up-to-date with 'origin/develop'.
        Changes to be committed:
          (use "git reset HEAD <file>..." to unstage)

                new file:   src/mqtt/Link-MQTT
                deleted:    src/mqtt/MQTTPacket/MQTTConnect.h
                deleted:    src/mqtt/MQTTPacket/MQTTConnectClient.c
                deleted:    src/mqtt/MQTTPacket/MQTTDeserializePublish.c
                deleted:    src/mqtt/MQTTPacket/MQTTPacket.c
                deleted:    src/mqtt/MQTTPacket/MQTTPacket.h
                deleted:    src/mqtt/MQTTPacket/MQTTPublish.h
                deleted:    src/mqtt/MQTTPacket/MQTTSerializePublish.c
                deleted:    src/mqtt/MQTTPacket/MQTTSubscribe.h
                deleted:    src/mqtt/MQTTPacket/MQTTSubscribeClient.c
                deleted:    src/mqtt/MQTTPacket/MQTTUnsubscribe.h
                deleted:    src/mqtt/MQTTPacket/MQTTUnsubscribeClient.c
                modified:   src/mqtt/iot.mk
                deleted:    src/mqtt/mqtt_client.c
                deleted:    src/mqtt/mqtt_client.h
                new file:   src/packages/Link-MQTT.git/HEAD
                new file:   src/packages/Link-MQTT.git/config
                new file:   src/packages/Link-MQTT.git/description
                new file:   src/packages/Link-MQTT.git/hooks/.gitkeep
                new file:   src/packages/Link-MQTT.git/info/exclude
                new file:   src/packages/Link-MQTT.git/objects/pack/pack-e179418df7c1ef40cd14d2b0b9ca05c06fe7a8d7.idx
                new file:   src/packages/Link-MQTT.git/objects/pack/pack-e179418df7c1ef40cd14d2b0b9ca05c06fe7a8d7.pack
                new file:   src/packages/Link-MQTT.git/packed-refs
                new file:   src/packages/Link-MQTT.git/refs/heads/.gitkeep
                new file:   src/packages/Link-MQTT.git/refs/tags/.gitkeep

> 形成commit

        edward @ ~/srcs/iotx-sdk-c$ git commit -m '[src/mqtt] modulize with Link-MQTT'
        [skip ] src/mqtt/Link-MQTT
        [skip ] src/packages/Link-MQTT.git/HEAD
        [skip ] src/packages/Link-MQTT.git/config
        [skip ] src/packages/Link-MQTT.git/description
        [skip ] src/packages/Link-MQTT.git/hooks/.gitkeep
        [skip ] src/packages/Link-MQTT.git/info/exclude
        [skip ] src/packages/Link-MQTT.git/objects/pack/pack-e179418df7c1ef40cd14d2b0b9ca05c06fe7a8d7.idx
        [skip ] src/packages/Link-MQTT.git/objects/pack/pack-e179418df7c1ef40cd14d2b0b9ca05c06fe7a8d7.pack
        [skip ] src/packages/Link-MQTT.git/packed-refs
        [skip ] src/packages/Link-MQTT.git/refs/heads/.gitkeep
        [skip ] src/packages/Link-MQTT.git/refs/tags/.gitkeep
        [develop 448afee] [src/mqtt] modulize with Link-MQTT
         25 files changed, 23 insertions(+), 4137 deletions(-)
         create mode 120000 src/mqtt/Link-MQTT
         delete mode 100644 src/mqtt/MQTTPacket/MQTTConnect.h
         delete mode 100644 src/mqtt/MQTTPacket/MQTTConnectClient.c
         delete mode 100644 src/mqtt/MQTTPacket/MQTTDeserializePublish.c
         delete mode 100644 src/mqtt/MQTTPacket/MQTTPacket.c
         delete mode 100644 src/mqtt/MQTTPacket/MQTTPacket.h
         delete mode 100644 src/mqtt/MQTTPacket/MQTTPublish.h
         delete mode 100644 src/mqtt/MQTTPacket/MQTTSerializePublish.c
         delete mode 100644 src/mqtt/MQTTPacket/MQTTSubscribe.h
         delete mode 100644 src/mqtt/MQTTPacket/MQTTSubscribeClient.c
         delete mode 100644 src/mqtt/MQTTPacket/MQTTUnsubscribe.h
         delete mode 100644 src/mqtt/MQTTPacket/MQTTUnsubscribeClient.c
         delete mode 100644 src/mqtt/mqtt_client.c
         delete mode 100644 src/mqtt/mqtt_client.h
         create mode 100644 src/packages/Link-MQTT.git/HEAD
         create mode 100644 src/packages/Link-MQTT.git/config
         create mode 100644 src/packages/Link-MQTT.git/description
         create mode 100644 src/packages/Link-MQTT.git/hooks/.gitkeep
         create mode 100644 src/packages/Link-MQTT.git/info/exclude
         create mode 100644 src/packages/Link-MQTT.git/objects/pack/pack-e179418df7c1ef40cd14d2b0b9ca05c06fe7a8d7.idx
         create mode 100644 src/packages/Link-MQTT.git/objects/pack/pack-e179418df7c1ef40cd14d2b0b9ca05c06fe7a8d7.pack
         create mode 100644 src/packages/Link-MQTT.git/packed-refs
         create mode 100644 src/packages/Link-MQTT.git/refs/heads/.gitkeep
         create mode 100644 src/packages/Link-MQTT.git/refs/tags/.gitkeep

> 推送commit到线上项目仓库

        edward @ ~/srcs/iotx-sdk-c$ git pull --rebase
        Current branch develop is up to date.
        edward @ ~/srcs/iotx-sdk-c$ git push
        Counting objects: 15, done.
        Delta compression using up to 2 threads.
        Compressing objects: 100% (13/13), done.
        Writing objects: 100% (15/15), 37.75 KiB | 0 bytes/s, done.
        Total 15 (delta 3), reused 1 (delta 0)
        To git@gitlab.alibaba-inc.com:IoTx/iotx-sdk-c.git
           50f79e0..448afee  develop -> develop
        edward @ ~/srcs/iotx-sdk-c$ git log -1
        commit 448afee17935c557cb0b643f57d0eca8e27accbc
        Author: Yang, Xiao <yusheng.yx@alibaba-inc.com>
        Date:   Tue Dec 26 11:25:17 2017 +0800

            [src/mqtt] modulize with Link-MQTT

### 三. 维护被组件化的代码

+ 确定被组件化的代码存放的本地目录, 比如`~/srcs/iotx-sdk-c.pkgs/Link-MQTT`

> 以上面的例子, 假设套件仓库在`~/srcs/iotx-sdk-c`, 则所有被组件化的模块都会解压到`~/srcs/iotx-sdk-c.pkgs`

    edward @ ~/srcs/iotx-sdk-c.pkgs$ ls
    Link-MQTT  LITE-log  LITE-utils  mbedtls-in-iotkit

+ 使用`Source Insight`等编辑器把解压目录的源码加入到工程
+ 编辑... 编译... 验证
+ 确定要提交的时候, 首先提交到组件本身仓库, 使用的位置是组件解压目录, 使用的命令是`git push upstream`

        edward @ ~/srcs/iotx-sdk-c.pkgs$ cd Link-MQTT/
        edward @ ~/srcs/iotx-sdk-c.pkgs/Link-MQTT$ git remote -v
        origin  /home/edward/srcs/iotx-sdk-c/src/packages/Link-MQTT.git (fetch)
        origin  /home/edward/srcs/iotx-sdk-c/src/packages/Link-MQTT.git (push)
        upstream        git@gitlab.alibaba-inc.com:iot-middleware/Link-MQTT.git (fetch)
        upstream        git@gitlab.alibaba-inc.com:iot-middleware/Link-MQTT.git (push)
        edward @ ~/srcs/iotx-sdk-c.pkgs/Link-MQTT$ git push upstream
      
+ 然后提交到项目目录, 使用的命令是`make repo-update`, 和上文一样

        edward @ ~/srcs/iotx-sdk-c$ make repo-update
        [ Link-MQTT.git ] <= : git@gitlab.alibaba-inc.com:iot-middleware/Link-MQTT.git :: master
        + cd /home/edward/srcs/iotx-sdk-c/src/packages
        + rm -rf Link-MQTT.git
        + git clone -q --bare -b master --single-branch git@gitlab.alibaba-inc.com:iot-middleware/Link-MQTT.git Link-MQTT.git
        + rm -rf Link-MQTT.git/hooks/
        + mkdir -p Link-MQTT.git/hooks/
        + touch Link-MQTT.git/hooks/.gitkeep
        + touch Link-MQTT.git/refs/heads/.gitkeep Link-MQTT.git/refs/tags/.gitkeep
        + rm -rf /home/edward/srcs/iotx-sdk-c.pkgs/Link-MQTT
        + cd /home/edward/srcs/iotx-sdk-c
        + set +x
        edward @ ~/srcs/iotx-sdk-c$ git add src/packages/Link-MQTT.git/
        edward @ ~/srcs/iotx-sdk-c$ git commit
        edward @ ~/srcs/iotx-sdk-c$ git pull --rebase
        edward @ ~/srcs/iotx-sdk-c$ git push