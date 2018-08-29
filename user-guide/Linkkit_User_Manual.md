# <a name="目录">目录</a>
+ [第一章 物联网平台设备端C-SDK概述](#第一章 物联网平台设备端C-SDK概述)
    * [1.1 阿里云物联网平台](#1.1 阿里云物联网平台)
    * [1.2 名词解释](#1.2 名词解释)
    * [1.3 C-SDK组成部分](#1.3 C-SDK组成部分)
    * [1.4 C-SDK版本一览](#1.4 C-SDK版本一览)
+ [第二章 快速开始](#第二章 快速开始)
    * [2.1 准备本地开发环境](#2.1 准备本地开发环境)
    * [2.2 准备云端控制台环境](#2.2 准备云端控制台环境)
    * [2.3 体验基础版C-SDK](#2.3 体验基础版C-SDK)
    * [2.4 体验高级版C-SDK](#2.4 体验高级版C-SDK)
+ [第三章 移植指南](#第三章 移植指南)
    * [3.1 在Ubuntu上编译主机版本](#3.1 在Ubuntu上编译主机版本)
    * [3.2 交叉编译到嵌入式硬件平台](#3.2 交叉编译到嵌入式硬件平台)
    * [3.3 开发未适配平台的HAL层](#3.3 开发未适配平台的HAL层)
+ [第四章 构建配置系统](#第四章 构建配置系统)
    * [4.1 基于make的编译系统详解](#4.1 基于make的编译系统详解)
    * [4.2 config.xxx.yyy文件语法详解](#4.2 config.xxx.yyy文件语法详解)
    * [4.3 iot.mk文件语法详解](#4.3 iot.mk文件语法详解)
    * [4.4 基于make交叉编译到嵌入式平台](#4.4 基于make交叉编译到嵌入式平台)
    * [4.5 基于make编译到主机环境](#4.5 基于make编译到主机环境)
    * [4.6 构建配置输入文件](#4.6 构建配置输入文件)
    * [4.7 基于cmake的编译示例](#4.7 基于cmake的编译示例)
+ [第五章 HAL说明](#第五章 HAL说明)
    * [5.1 设备端C-SDK分层](#5.1 设备端C-SDK分层)
    * [5.2 HAL层接口列表](#5.2 HAL层接口列表)
    * [5.3 HAL层接口详解](#5.3 HAL层接口详解)
+ [第六章 API说明](#第六章 API说明)
    * [6.1 API接口清单](#6.1 API接口清单)
    * [6.2 API接口列表](#6.2 API接口列表)
    * [6.3 新版API接口详解](#6.3 新版API接口详解)
    * [6.4 高级版(旧版)单品API详解](#6.4 高级版(旧版)单品API详解)
    * [6.5 高级版(旧版)网关API详解](#6.5 高级版(旧版)网关API详解)
+ [第七章 典型场景示例](#第七章 典型场景示例)
    * [7.1 MQTT站点配置](#7.1 MQTT站点配置)
    * [7.2 TSL静态集成和动态拉取](#7.2 TSL静态集成和动态拉取)
    * [7.3 单品动态注册/一型一密](#7.3 单品动态注册/一型一密)
+ [附录A 功能分项说明](#附录A 功能分项说明)
    * [A.1 基础通信](#A.1 基础通信)
    * [A.2 设备管理](#A.2 设备管理)
    * [A.3 WiFi配网](#A.3 WiFi配网)
    * [A.4 本地通信](#A.4 本地通信)
+ [附录C 云端接口说明](#附录C 云端接口说明)
    * [C.1 设备身份注册](#C.1 设备身份注册)
    * [C.2 子设备与网关的拓扑关系](#C.2 子设备与网关的拓扑关系)
    * [C.3 设备属性、服务、事件](#C.3 设备属性、服务、事件)
    * [C.4 设备标签](#C.4 设备标签)


第一章目录
---
+ [第一章 物联网平台设备端C-SDK概述](#第一章 物联网平台设备端C-SDK概述)
    * [1.1 阿里云物联网平台](#1.1 阿里云物联网平台)
    * [1.2 名词解释](#1.2 名词解释)
        - [产品](#产品)
        - [设备](#设备)
        - [productKey](#productKey)
        - [productSecret](#productSecret)
        - [子设备](#子设备)
        - [网关](#网关)
        - [单品](#单品)
        - [deviceName](#deviceName)
        - [deviceSecret](#deviceSecret)
        - [Topic类](#Topic类)
        - [Topic](#Topic)
        - [发布](#发布)
        - [订阅](#订阅)
        - [授权](#授权)
        - [属性](#属性)
        - [服务](#服务)
        - [事件](#事件)
        - [RRPC](#RRPC)
        - [Alink协议](#Alink协议)
        - [数据解析脚本](#数据解析脚本)
        - [设备影子](#设备影子)
        - [规则引擎](#规则引擎)
    * [1.3 C-SDK组成部分](#1.3 C-SDK组成部分)
        - [架构框图](#架构框图)
        - [主要目录及文件一览](#主要目录及文件一览)
    * [1.4 C-SDK版本一览](#1.4 C-SDK版本一览)
        - [V2.2.1](#V2.2.1)
        - [V2.2.0](#V2.2.0)
        - [V2.1.0](#V2.1.0)
        - [V2.0.3](#V2.0.3)
        - [V2.0.2](#V2.0.2)
        - [V2.0.1](#V2.0.1)
        - [V2.0.0](#V2.0.0)
        - [V1.0.1](#V1.0.1)



*[回到目录](#目录)*
# <a name="第一章 物联网平台设备端C-SDK概述">第一章 物联网平台设备端C-SDK概述</a>

*[回到目录](#目录)*
## <a name="1.1 阿里云物联网平台">1.1 阿里云物联网平台</a>

> 最初, 物联网应用的开发从设备端到云端都是完全由技术人员自行完成, 没有相当强大的人员和资源支持很难完成, 导致小公司做不起来
>
> 而具有一定能力的公司真正实现了端到端之后, 又会发现产出非常低, 如果进行大规模投入, 经济效益会很差, 导致大公司做物联网应用不能产生很好的商业效果
>
> 这制约了物联网, 尤其是物联网领域的应用的发展
>
> 在这样的背景下, 一些企业发现物联网应用在安全性、可运营可管理等方面有很多共性的需求, 于是他们提供了这样一些能力, 组件, 工具和基础设施
>
> 后来, 随着“云化”、“平台”、“服务”的理念越来越普及, 又有企业想到以云服务的方式给开发者提供平台, 支撑开发者在平台上开发物联网应用. 渐渐的, 物联网平台就出现了

**阿里云物联网平台(LinkPlatform), 旧称"物联网套件", 则是阿里云专门为物联网领域的开发人员推出的一款物联网平台**

其目的是帮助开发者搭建安全且性能强大的数据通道, 方便终端(如传感器, 执行器, 嵌入式设备或智能家电等等)和云端的双向通信

具体来说, 阿里云物联网平台提供了如下的能力:
---

+ **嵌入式设备快速接入(设备端SDK)**
+ 设备管理
+ 设备和数据信息安全
+ 桥接到阿里云其他产品, 对设备数据存储/计算

它的使用场景可以这样表示:
---

![image](https://code.aliyun.com/edward.yangx/public-docs/raw/master/images/lp_comps.png)

**图中, 红框标识的物联网设备端C-SDK就是将嵌入式设备连接到阿里云的部分, 也是本手册将要详细介绍的**

*说明: 图中的黄色模块是高级版物联网平台所特有的功能.*

> 高级版包含基础版的所有功能, 并且提供Alink协议, 具备更加完整的设备全生命周期管理能力
>
> 支持设备模型定义、数据解析、在线调试和远程维护, 提供数据存储, 可实时获取设备运行状态或历史数据
>
> 同样支持规则引擎进行数据流转. 使开发者无需考虑设备的数据格式和存储等问题
>
> 进一步降低机器智能化周期和成本投入, 让开发者可以更加聚焦于垂直业务系统的搭建, 快速实现智能转型

*[回到目录](#目录)*
## <a name="1.2 名词解释">1.2 名词解释</a>
> 为介绍物联网平台, 和它的设备端C-SDK, 我们对相关名词做出以下约定性的概念定义

*[回到目录](#目录)*
### <a name="产品">产品</a>
也称品类, 是一类同样功能的设备的集合, 每个产品都有productKey加以区别, 产品下面可以有成千上万的设备, 并用设备证书区别每个设备

*[回到目录](#目录)*
### <a name="设备">设备</a>
归属于某个品类下的具体设备, 用设备证书区别每个设备, 设备可以直接连接物联网平台, 也可以作为子设备通过网关连接物联网平台

*[回到目录](#目录)*
### <a name="productKey">productKey</a>
productKey是物联网平台为产品颁发的唯一标识. 这个参数很重要, 需要用户保管好, 在设备认证以及通信中都会用到

*[回到目录](#目录)*
### <a name="productSecret">productSecret</a>
是由平台办法的产品密钥, 通常与ProductKey成对出现, 可用于一型一密的认证方案. 这个参数很重要, 需要用户保管好, 不能泄露

*[回到目录](#目录)*
### <a name="子设备">子设备</a>
也叫从设备, 它本质上仍是设备, 只不过表现形态是是通过另一台设备, 即所谓的网关, 连接到阿里云服务器, 而不是直接连到云端

> 一般这种设备不具有独立自主的因特网(公网)连接能力
>
>> 比如, 蓝牙耳机, Zigbee传感器等, 它们基于自身的蓝牙或Zigbee协议连接到蓝牙网关和Zigbee网关上, 后者建立和阿里云服务器之间的 TCP/IP 连接
>>
>> 当子设备有上报数据时, 数据是先通过阿里不做约束的各种本地网络通道比如蓝牙/Zigbee发送到网关, 再经过用户调用网关API的环节, 从网关和阿里云的通道代为发往云端
>>
>> 当云端有命令下发给子设备时, 命令也是先通过TCP/IP公网通道下发到网关设备, 再经过用户调用网关API的环节, 把命令代为接收和解析后, 通过蓝牙/Zigbee下发给子设备

*[回到目录](#目录)*
### <a name="网关">网关</a>
也叫主设备, 网关是一种直连云端的设备, 但区别于普通设备的地方在于网关设备具备对子设备的管理能力, 并且能够代替子设备连接云端

*[回到目录](#目录)*
### <a name="单品">单品</a>
这是相对于网关的概念来说的, 这样的设备不会将自己和阿里云服务器之间的TCP/IP通道分享给下联的其它设备(子设备)

> 单品设备有直接连云的能力并独占被建立的通道, 不需要为其它设备代为上报数据和接收指令

*[回到目录](#目录)*
### <a name="deviceName">deviceName</a>
用户注册设备时, 自定义生成的设备编号, 可以基于该编号与设备通信, 保证产品维度内的唯一性. 这个参数很重要, 需要用户保管好, 在设备认证以及通信中都会用到

*[回到目录](#目录)*
### <a name="deviceSecret">deviceSecret</a>
物联网平台为设备颁发的设备密钥, 和deviceName成对出现. 这个参数很重要, 需要用户保管好, 在设备认证会用到

*[回到目录](#目录)*
### <a name="Topic类">Topic类</a>
同一产品不同设备所共同定义的Topic集合, 一个Topic类对一个productKey下所有设备通用

*[回到目录](#目录)*
### <a name="Topic">Topic</a>
一个形如"/aaa/bbb/ccc"的字符串, 它在基于发布/订阅模型的通信系统中, 起到类似通信地址的功能, 是消息分发的依据

*[回到目录](#目录)*
### <a name="发布">发布</a>
对应Publish, 表达以某个Topic字符串作为目标地址的发送行为. 可能是设备向云端已定义的Topic做上行发布, 也可能是云端向设备已订阅的Topic做下行发布

*[回到目录](#目录)*
### <a name="订阅">订阅</a>
对应Subscribe, 表达设备向云端申请, 可以接受某个Topic字符串作为目标地址的下推报文的过程. 订阅只有可能是设备向云端做上行订阅

*[回到目录](#目录)*
### <a name="授权">授权</a>
对于设备端, 设备必须具有权限, 才可以往某个Topic发布或订阅消息, 这让用户可以完全控制Topic的消息转发, 帮助用户控制数据的安全性

*[回到目录](#目录)*
### <a name="属性">属性</a>
对应Property, 设备的物模型三要素之一, 一般用于描述设备运行时的状态, 如环境监测设备所读取的当前环境温度等

属性支持get和set, 应用系统可发起对属性的读取和设置请求

*[回到目录](#目录)*
### <a name="服务">服务</a>
对应Service, 设备的物模型三要素之一, 指可被外部调用的能力或方法, 包含输入参数和输出参数

相比于下发指令设置属性值, 服务可通过一条指令实现更加复杂的业务逻辑, 如执行某项特定的任务

*[回到目录](#目录)*
### <a name="事件">事件</a>
对应Event, 设备的物模型三要素之一, 相比于属性状态, 事件一般而言包含设备需要被外部感知和处理的通知信息

可包含多个输出参数, 如某项任务完成的信息或者设备发生故障/告警时的温度等, 事件可以被订阅和推送

*[回到目录](#目录)*
### <a name="RRPC">RRPC</a>
即Revert-RPC. 从字面上理解, 可以实现由服务端请求设备端, 并能够使设备端同步响应的功能

*[回到目录](#目录)*
### <a name="Alink协议">Alink协议</a>
阿里云定义的, 基于JSON格式的, 设备与云端之间的通信协议. 这份协议的完整描述存放在 [官网Alink协议说明](https://help.aliyun.com/document_detail/70046.html) 页面

*[回到目录](#目录)*
### <a name="数据解析脚本">数据解析脚本</a>
针对高级版中采用透传格式/自定义格式的设备, 需要在云端编写数据解析脚本, 将设备上报的二进制数据或者自定义的JSON数据, 转换为平台上的Alink JSON数据格式

*[回到目录](#目录)*
### <a name="设备影子">设备影子</a>
对应Device Shadow, 设备影子是一个JSON文档, 用于存储设备或者应用的当前状态信息

每个设备都会在云端有唯一的设备影子对应, 用户可以使用设备影子通过MQTT或HTTP获取和设置设备的状态, 无论该设备是否连接到Internet

*[回到目录](#目录)*
### <a name="规则引擎">规则引擎</a>
为用户提供类SQL语言的规则引擎, 帮助用户将Topic中的数据进行过滤处理, 并能够将处理后的数据发送到阿里云其他服务, 例如MNS,Table Store,DataHub等等

*[回到目录](#目录)*
## <a name="1.3 C-SDK组成部分">1.3 C-SDK组成部分</a>
*[回到目录](#目录)*
### <a name="架构框图">架构框图</a>

![image](https://code.aliyun.com/edward.yangx/public-docs/raw/master/images/sdk_arch.png)

*[回到目录](#目录)*
### <a name="主要目录及文件一览">主要目录及文件一览</a>

    .
    +-- build-rules                         : 编译构建系统, 基于GNU Make和bash脚本
    +-- project.mk                          : 编译系统配置, 指定SDK的目录排布等
    +-- LICENSE                             : 软件许可证, C-SDK使用的是Apache-2.0版本软件许可证
    +-- README.txt                          : 简要说明, 列出了C-SDK的功能模块和模块内容等
    +-- makefile                            : 基于GNU Make编译SDK的顶层Makefile
    +-- CMakeLists.txt                      : 基于cmake编译SDK的顶层CMakeLists.txt
    +-- make.settings                       : 功能裁剪配置, 可编辑该文件来裁剪SDK的内容
    |
    +-- examples                            : 例程目录, 演示SDK的使用
    |   +-- coap                            :     演示如何使用通信模块CoAP的API
    |   +-- device-shadow                   :     演示如何使用服务模块DeviceShadow的API
    |   +-- http                            :     演示如何使用通信模块HTTP的API
    |   +-- http2                           :     演示如何使用通信模块HTTP2的API
    |   +-- linkkit                         :     演示如何使用服务模块linkkit的API
    |   +-- mqtt                            :     演示如何使用通信模块MQTT的API
    |   +-- uOTA                            :     演示如何使用服务模块uOTA的API
    |
    +-- include                             : 头文件目录, 列出SDK依赖的HAL接口和向用户提供的API接口
    |   +-- iot_export.h                    :     列出所有API层函数的声明, 是SDK所提供的接口
    |   +-- iot_import.h                    :     列出所有HAL层函数的声明, 是SDK所依赖的接口
    |   +-- exports                         :     列出各功能点提供的API层接口
    |   +-- imports                         :     列出各功能点依赖的HAL层接口
    |
    +-- src
    |   +-- board                           : 跨平台适配目录, 一个目标平台对应一个config.xxx.yyy文件
    |   |   +-- config.macos.make           :     适配到OSX系统的编译配置文件
    |   |   +-- config.rhino.make           :     适配到AliOS Things系统的编译配置文件
    |   |   +-- config.ubuntu.x86           :     适配到Ubuntu系统的编译配置文件
    |   |   +-- config.win7.mingw32         :     适配到Win7/Win10系统的编译配置文件
    |   |
    |   +-- infra                           : SDK核心实现中的基础模块目录
    |   |   +-- log                         :     实现运行时SDK的日志
    |   |   +-- system                      :     实现全局信息保存, 如官方根证书, 设备标识ID等
    |   |   +-- utils                       :     实现工具函数, 如连接鉴权时的SHA1摘要计算等
    |   +-- protocol                        : SDK核心实现中的通信模块目录
    |   |   +-- alcs                        :     实现设备和手机app之间的本地加密通信
    |   |   +-- coap                        :     实现设备和阿里云之间的CoAP协议通信
    |   |   +-- http                        :     实现设备和阿里云之间的HTTP协议通信
    |   |   +-- http2                       :     实现设备和阿里云之间的HTTP2协议通信
    |   |   +-- mqtt                        :     实现设备和阿里云之间的MQTT协议通信
    |   +-- services                        : SDK核心实现中的服务模块目录
    |   |   +-- awss                        :     实现设备和手机app之间的WiFi配网服务
    |   |   +-- linkkit                     :     实现设备和阿里云之间的物模型管理服务
    |   |   +-- uOTA                        :     实现设备和阿里云之间的固件升级服务
    |   |
    |   +-- sdk-impl                        : API实现目录, iot_export.h中的API在此实现
    |   +-- ref-impl                        : 参考实现目录, 包括加解密库和HAL接口的参考实现
    |   |   +-- hal                         :     SDK所依赖的HAL接口的参考实现
    |   |   +-- tls                         :     加解密库的参考实现, 由开源软件mbedtls裁剪而成
    |   |
    |   +-- tools                           : 用于辅助build-rules编译系统的编译脚本文件
    |
    +-- prebuilt                            : 预编译二进制库存放目录, 分头文件和库文件的子目录
    |   +-- ubuntu
    |   |   +-- include                     :     Ubuntu平台预编译库对应的头文件
    |   |   +-- libs                        :     Ubuntu平台预编译库, 64位
    |   +-- win7
    |       +-- include                     :     Win7平台预编译库对应的头文件
    |       +-- libs                        :     Win7平台预编译库, 64位
    |       +-- 32bit-libs                  :     Win7平台预编译库, 32位
    |
    +-- tests                               : SDK的单元测试例目录

*[回到目录](#目录)*
## <a name="1.4 C-SDK版本一览">1.4 C-SDK版本一览</a>

*[回到目录](#目录)*
### <a name="V2.2.1">V2.2.1</a>
+ 发布日期: 2018/08/31
+ 下载链接: TBD
+ 更新内容:
    - **新增一型一密/动态注册功能:** 此功能可简化产线生产流程, 使所有设备烧录同样的固件
    - **新增离线倒计时例程:** 用于[飞燕平台](https://living.aliyun.com)的本地倒计时场景演示
    - **新增OTA使用iTLS下载固件:** 在使能iTLS/ID2的情况下, 不再需要HTTPS下载固件
    - **新增WiFi配网功能开源发布:** 在 `src/services/awss` 中, 首次源码发布WiFi配网模块, 用户可自行交叉编译
    - **新增本地通信功能开源发布:** 在 `src/protocol/alcs` 中, 首次源码发布本地通信模块, 用户可自行交叉编译

*[回到目录](#目录)*
### <a name="V2.2.0">V2.2.0</a>
+ 发布日期: 2018/08/06
+ 下载链接: [linkkit2.2.tar.gz](https://linkkit-sdk-download.oss-cn-shanghai.aliyuncs.com/linkkit2.2.tar.gz)
+ 更新内容:
    - **服务模块中新增:**
        * 离线Reset功能
        * 设备状态上报
        * 单品/网关统一处理
        * 简化版TSL支持
        * 设备禁用/使能功能
        * 高级版的上下行应答可配置
        * TSL数组中支持Objects类型
    - **通信模块中新增:**
        * MQTT海外多站点连接: 美西/新加坡/德国/日本(预发验证)
        * MQTT批量订阅用于物模型管理
        * HTTP2协议支持
        * iTLS(预置ID2)支持
        * CoAP基于PSK的认证和加密
        * CoAP的DTLS Session复用

*[回到目录](#目录)*
### <a name="V2.1.0">V2.1.0</a>
+ 发布日期: 2018/03/20
+ 下载链接: [RELEASED_V2_03_20180320.zip](https://github.com/aliyun/iotkit-embedded/archive/RELEASED_V2_1_20180320.zip)
+ 更新内容:
    - **新增cmake编译系统:** 可以在linux下或者windows下以cmake编译
    - **新增物模型支持:** 可以支持物模型抽象, 提供属性/服务/事件的相关接口, 可参考[相关公告](https://help.aliyun.com/document_detail/88019.html)

*[回到目录](#目录)*
### <a name="V2.0.3">V2.0.3</a>
+ 发布日期: 2018/01/31
+ 下载链接: [RELEASED_V2_03_20180131.zip](https://github.com/aliyun/iotkit-embedded/archive/RELEASED_V2_03_20180131.zip)
+ 更新内容:
    - **支持主子设备功能:** 设置`FEATURE_SUBDEVICE_ENABLED = y`, 可以支持子设备通过主设备(网关设备)进行数据交互
    - **升级HTTP通道:** 优化HTTP流程
    - **优化TLS:** 修复内存泄漏问题
    - **优化OTA的配置:** 可以更合理的开关OTA功能
    - **升级MQTT通道:** 支持topic更长, 更多的订阅请求; MQTT支持多线程

*[回到目录](#目录)*
### <a name="V2.0.2">V2.0.2</a>
+ 发布日期: 2017/11/30
+ 下载链接: [RELEASED_V2_02_20171130.zip](https://github.com/aliyun/iotkit-embedded/archive/RELEASED_V2_02_20171130.zip)
+ 更新内容:
    - **正式的多平台支持:** 使用`make reconfig`可弹出和选择`Ubuntu16.04`以外的已适配平台
    - **新增Windows版本:** 支持用mingw32编译`Win7`版本的库和例程, 详细可访问[第四章 构建配置系统](#第四章 构建配置系统)
    - **新增OpenSSL适配:** 新增了配合`openssl-0.9.x`+`Windows`版本的HAL参考实现
    - **优化HTTP接口:** HTTP通道方面接口优化, 支持发送报文而不断开TLS连接
    - **自包含的安全库:** 新增裁剪版本的安全库`mbedtls`, 目前已适配`Linux`和`Windows`平台

*[回到目录](#目录)*
### <a name="V2.0.1">V2.0.1</a>
+ 发布日期: 2017/10/10
+ 下载链接: [RELEASED_V2_01_20171010.zip](https://github.com/aliyun/iotkit-embedded/archive/RELEASED_V2_01_20171010.zip)
+ 更新内容:
    - **新增CoAP+OTA:** 允许配置成基于CoAP通知方式的OTA
    - **新增HTTP+TLS:** 在MQTT/CoAP之外, 新增HTTP的通道
    - **细化OTA状态:** 优化OTA部分代码, 使云端可以更细化的区分设备的OTA固件下载状态
    - **ArmCC支持:** 修正了SDK在ArmCC编译器编译时会出现的报错

*[回到目录](#目录)*
### <a name="V2.0.0">V2.0.0</a>
+ 发布日期: 2017/08/21
+ 下载链接: [RELEASED_V2_00_20170818.zip](https://github.com/aliyun/iotkit-embedded/archive/RELEASED_V2_00_20170818.zip)
+ 更新内容:
    - **新增MQTT直连:** 支持更快更轻的连接IoT套件, 去掉对HTTPS/HTTP的依赖, 可参看[相关公告](https://help.aliyun.com/document_detail/57164.html)
    - **新增CoAP通道:** 基于UDP, 在纯上报数据场景更节省资源, 可参看[相关公告](https://help.aliyun.com/document_detail/57566.html)
    - **新增OTA通道:** 提供一系列OTA相关的API, 可查询/触发/下载用户自主上传的固件
    - **升级构建系统:** 支持更灵活的组织和配置SDK

*[回到目录](#目录)*
### <a name="V1.0.1">V1.0.1</a>
+ 发布日期: 2017/06/29
+ 下载链接: [RELEASED_V1_0_1_20170629.zip](https://github.com/aliyun/iotkit-embedded/archive/RELEASED_V1_0_1_20170629.zip)
+ 更新内容:
    - **华东2站点:** 第一个正式配合华东2站点的设备端C-SDK, 全源码发布
    - **新增设备影子功能:** 具体可参看[设备影子介绍页面](https://help.aliyun.com/document_detail/53930.html)



第二章目录
---
+ [第二章 快速开始](#第二章 快速开始)
    * [2.1 准备本地开发环境](#2.1 准备本地开发环境)
        - [安装Ubuntu16.04](#安装Ubuntu16.04)
        - [安装必备软件](#安装必备软件)
    * [2.2 准备云端控制台环境](#2.2 准备云端控制台环境)
        - [注册/登录阿里云账号](#注册/登录阿里云账号)
        - [访问物联网套件控制台](#访问物联网套件控制台)
    * [2.3 体验基础版C-SDK](#2.3 体验基础版C-SDK)
        - [创建基础版产品和设备](#创建基础版产品和设备)
        - [编译运行基础版例程](#编译运行基础版例程)
            + [下载解压物联网平台设备端C-SDK](#下载解压物联网平台设备端C-SDK)
            + [填写设备三元组到例程中](#填写设备三元组到例程中)
            + [编译基础版例程](#编译基础版例程)
            + [运行基础版例程](#运行基础版例程)
        - [观察基础版例程](#观察基础版例程)
            + [观察消息上报](#观察消息上报)
            + [观察消息下推](#观察消息下推)
            + [观察控制台日志](#观察控制台日志)
    * [2.4 体验高级版C-SDK](#2.4 体验高级版C-SDK)
        - [创建高级版产品和设备](#创建高级版产品和设备)
        - [为高级版设备创建物模型](#为高级版设备创建物模型)
        - [编译运行高级版例程](#编译运行高级版例程)
            + [下载设备物模型](#下载设备物模型)
            + [填写设备物模型到例程中](#填写设备物模型到例程中)
            + [填写设备三元组到例程中](#填写设备三元组到例程中)
            + [编译高级版例程](#编译高级版例程)
            + [运行高级版例程](#运行高级版例程)
        - [观察高级版例程](#观察高级版例程)
            + [观察属性的上报](#观察属性的上报)
            + [观察属性的设置](#观察属性的设置)
            + [观察事件的上报](#观察事件的上报)
            + [观察服务的下发](#观察服务的下发)

*[回到目录](#目录)*
# <a name="第二章 快速开始">第二章 快速开始</a>

本章描述如何申请自己的设备, 并结合C-SDK快速体验该设备通过`MQTT`协议(基础版), 以及通过`Alink`协议(高级版), 连接到阿里云, 上报和接收业务报文.

*[回到目录](#目录)*
## <a name="2.1 准备本地开发环境">2.1 准备本地开发环境</a>

*[回到目录](#目录)*
### <a name="安装Ubuntu16.04">安装Ubuntu16.04</a>

本SDK的编译环境是**64位**主机上的`Ubuntu16.04`, 在其它Linux上尚未测试过, 所以推荐安装与阿里开发者一致的发行版

如果您使用`Windows`操作系统, 建议安装虚拟机软件`Virtualbox`, 下载地址: [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)

然后安装64位的Desktop版本的`Ubuntu 16.04.x LTS`, 下载地址: [http://releases.ubuntu.com/16.04](http://releases.ubuntu.com/16.04)

*[回到目录](#目录)*
### <a name="安装必备软件">安装必备软件</a>

本SDK的开发编译环境使用如下软件: `make-4.1`, `git-2.7.4`, `gcc-5.4.0`, `gcov-5.4.0`, `lcov-1.12`, `bash-4.3.48`, `tar-1.28`, `mingw-5.3.1`

可使用如下命令行安装必要的软件:

    apt-get install -y build-essential make git gcc

*[回到目录](#目录)*
## <a name="2.2 准备云端控制台环境">2.2 准备云端控制台环境</a>
*[回到目录](#目录)*
### <a name="注册/登录阿里云账号">注册/登录阿里云账号</a>

访问阿里云[登录页面](https://account.aliyun.com/login/login.htm), 点击[免费注册](https://account.aliyun.com/register/register.htm), 免费获得一个阿里云账号. 若您已有账号, 可直接登录

*[回到目录](#目录)*
### <a name="访问物联网套件控制台">访问物联网套件控制台</a>

登入之后, 鼠标悬停在**产品**上, 弹出层叠菜单, 并单击**物联网平台**

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/%E4%BA%A7%E5%93%81-%E7%89%A9%E8%81%94%E7%BD%91%E5%B9%B3%E5%8F%B0.png)

然后, 点击**立即开通**, 即可进入IoT控制台

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/%E4%BA%A7%E5%93%81-%E9%98%BF%E9%87%8C%E4%BA%91Link%20Platform-%E7%AB%8B%E5%8D%B3%E5%BC%80%E9%80%9A.png)

*[回到目录](#目录)*
## <a name="2.3 体验基础版C-SDK">2.3 体验基础版C-SDK</a>

> MQTT协议是IBM开发的一个即时通讯协议, 是为大量计算能力有限, 且工作在低带宽, 不可靠的网络的远程传感器和控制设备通讯而设计的协议
>
> 利用MQTT协议是一种基于二进制消息的发布/订阅编程模式的消息协议
>
> 本节将要介绍的例子程序先在阿里云IoT平台订阅(`Subscribe`)一个`Topic`成功, 然后自己向该`Topic`做发布(`Publish`)动作
>
> 阿里云IoT平台收到被发布的消息之后, 就会将该消息原样推送回给例子程序
>
> 因为该程序之前已经通过订阅(`Subscribe`)动作成为该`Topic`的一个接收者, 发布到这个`Topic`上的任何消息, 都会被推送到已订阅该`Topic`的所有终端上

*[回到目录](#目录)*
### <a name="创建基础版产品和设备">创建基础版产品和设备</a>

进入IoT控制台后, 点击页面左侧导航栏的**产品管理**, 再点击右侧的**创建产品**, 如下图所示: 

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E5%88%9B%E5%BB%BA%E4%BA%A7%E5%93%81.png)

在弹出的创建产品中, 选择基础版, 填写产品信息: 

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E5%88%9B%E5%BB%BA%E4%BA%A7%E5%93%81-%E8%AF%A6%E7%BB%86%E5%8F%82%E6%95%B0.png)

填写**产品名称**, 选择**节点类型**.

填写好产品信息后, 点击**确认**即可生成该产品: 

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E5%88%9B%E5%BB%BA%E4%BA%A7%E5%93%81-%E7%A1%AE%E8%AE%A4.png)

点击产品右侧的**查看**, 可跳转到产品详情页面: 

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E4%BA%A7%E5%93%81%E8%AF%A6%E6%83%85.png)

在该页面中, 有四个主要的选项卡: 

- 产品信息: 展示产品相关信息, 其中ProductKey用于标示产品的品类, 该产品下所有设备的ProductKey均一致
- 消息通信: 展示产品用于上下行数据的主要Topic
- 服务端订阅: 在这里可以选择设备消息类型并推送到MNS队列中, 用户服务端从队列里获得设备数据. 这样简化服务端订阅设备数据的流程, 让客户的服务端能够简单方便并高可靠的获得设备数据.
- 日志服务: 此处可浏览设备的历史上下行消息

产品创建好后, 接下来可以在该产品/品类下创建设备实例了

点击**产品详情**页面中**设备数**旁的**前往管理**, 即可看到当前产品下的设备列表, 目前为空: 

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E4%BA%A7%E5%93%81%E8%AF%A6%E6%83%85-%E8%AE%BE%E5%A4%87%E7%AE%A1%E7%90%86.png)

点击上图右侧的**添加设备**, 开始创建设备: 

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E4%BA%A7%E5%93%81%E8%AF%A6%E6%83%85-%E8%AE%BE%E5%A4%87%E7%AE%A1%E7%90%86-%E6%B7%BB%E5%8A%A0%E8%AE%BE%E5%A4%87.png)

在填写好**DeviceName**后, 点击确认即可创建该设备, 生成设备的**三元组**: 

- `ProductKey`: 标识产品的品类, 相同产品的所有设备ProductKey均相同
- `DeviceName`: 标识产品下的每个设备, 相同产品的所有设备DeviceName均不相同
- `DeviceSecret`: 设备密钥, 每个设备均不相同

*三元组用于标识阿里云上的每个设备, 用于连接阿里云服务器时完成设备认证*

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E4%BA%A7%E5%93%81%E8%AF%A6%E6%83%85-%E8%AE%BE%E5%A4%87%E7%AE%A1%E7%90%86-%E6%B7%BB%E5%8A%A0%E8%AE%BE%E5%A4%87-%E7%94%9F%E6%88%90%E4%B8%89%E5%85%83%E7%BB%84.png)

至此, 基础版产品与设备创建完成.

此外, 为了example演示, 我们在产品的消息通信中定义一个自定义的topic, 用于向服务端发送数据和从服务端接收数据: 

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-%E4%BA%A7%E5%93%81-%E6%B6%88%E6%81%AF%E9%80%9A%E4%BF%A1-%E8%87%AA%E5%AE%9A%E4%B9%89topic-data.png)

*[回到目录](#目录)*
### <a name="编译运行基础版例程">编译运行基础版例程</a>

#### <a name="下载解压物联网平台设备端C-SDK">下载解压物联网平台设备端C-SDK</a>

以获取 `V2.2.0` 版本SDK为例, 可用如下命令行获取:

    cd ${HOME}
    mkdir srcs
    cd srcs
    wget https://linkkit-sdk-download.oss-cn-shanghai.aliyuncs.com/linkkit2.2.tar.gz
    tar xvf linkkit2.2.tar.gz

如下则在 `~/srcs/iotx-sdk-c` 目录得到了解压之后的C-SDK

    cd iotx-sdk-c/
    ls
    aos.makefile  build-rules  CMakeLists.txt  doc  examples  include  LICENSE  makefile  make.settings  prebuilt  project.mk  README.md  src  tests

#### <a name="填写设备三元组到例程中">填写设备三元组到例程中</a>

打开文件 `iotx-sdk-c/examples/mqtt/mqtt-example.c`, 编辑如下代码段, 填入之前[创建基础版产品和设备](#创建基础版产品和设备)步骤中得到的**设备三元组**:

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-%E4%BB%A3%E7%A0%81%E7%A4%BA%E4%BE%8B-mqtt-example-%E4%B8%89%E5%85%83%E7%BB%84%E4%BF%AE%E6%94%B9.png)

#### <a name="编译基础版例程">编译基础版例程</a>

运行如下命令:

    cd ~/srcs/iotx-sdk-c
    make distclean
    make

编译成功完成后, 生成的样例程序在当前路径的 `output/release/bin` 目录下:

    $ tree output/release
    output/release/
    +-- bin
    ...
    ...
    |   +-- mqtt-example
    ...
    ...

#### <a name="运行基础版例程">运行基础版例程</a>

执行如下命令:

    cd ~/srcs/iotx-sdk-c
    ./output/release/bin/mqtt-example

得到运行结果如下:

    [inf] iotx_device_info_init(39): device_info created successfully!
    [dbg] iotx_device_info_set(49): start to set device info!
    [dbg] iotx_device_info_set(63): device_info set successfully!
    [inf] guider_print_dev_guider_info(279): ....................................................
    [inf] guider_print_dev_guider_info(280):           ProductKey : a1ExpAkj9Hi
    [inf] guider_print_dev_guider_info(281):           DeviceName : Example1
    [inf] guider_print_dev_guider_info(282):             DeviceID : a1ExpAkj9Hi.Example1
    [inf] guider_print_dev_guider_info(284): ....................................................
    [inf] guider_print_dev_guider_info(285):        PartnerID Buf : ,partner_id=example.demo.partner-id
    [inf] guider_print_dev_guider_info(286):         ModuleID Buf : ,module_id=example.demo.module-id
    [inf] guider_print_dev_guider_info(287):           Guider URL :
    [inf] guider_print_dev_guider_info(289):       Guider SecMode : 2 (TLS + Direct)
    [inf] guider_print_dev_guider_info(291):     Guider Timestamp : 2524608000000
    [inf] guider_print_dev_guider_info(292): ....................................................
    [inf] guider_print_dev_guider_info(298): ....................................................
    [inf] guider_print_conn_info(256): -----------------------------------------
    [inf] guider_print_conn_info(257):             Host : a1ExpAkj9Hi.iot-as-mqtt.cn-shanghai.aliyuncs.com
    [inf] guider_print_conn_info(258):             Port : 1883
    [inf] guider_print_conn_info(261):         ClientID : a1ExpAkj9Hi.Example1|securemode=2,timestamp=2524608000000,signmethod=hmacsha1 ...
    [inf] guider_print_conn_info(263):       TLS PubKey : 0x437636 ('-----BEGIN CERTI ...')
    [inf] guider_print_conn_info(266): -----------------------------------------
    [inf] IOT_MQTT_Construct(3005):      CONFIG_MQTT_SUBTOPIC_MAXNUM : 65535
    [dbg] IOT_MQTT_Construct(3007): sizeof(iotx_mc_client_t) = 1573144!
    [inf] iotx_mc_init(2098): MQTT init success!
    [inf] _ssl_client_init(142): Loading the CA root certificate ...
    [inf] _ssl_client_init(149):  ok (0 skipped)
    [inf] _TLSConnectNetwork(315): Connecting to /a1ExpAkj9Hi.iot-as-mqtt.cn-shanghai.aliyuncs.com/1883...
    [inf] mbedtls_net_connect_timeout(257): setsockopt SO_SNDTIMEO timeout: 10s
    [inf] mbedtls_net_connect_timeout(260): connecting IP_ADDRESS: 106.15.100.2
    [inf] _TLSConnectNetwork(328):  ok
    [inf] _TLSConnectNetwork(333):   . Setting up the SSL/TLS structure...
    [inf] _TLSConnectNetwork(343):  ok
    [inf] _TLSConnectNetwork(382): Performing the SSL/TLS handshake...
    [inf] _TLSConnectNetwork(390):  ok
    [inf] _TLSConnectNetwork(394):   . Verifying peer X.509 certificate..
    [inf] _real_confirm(90): certificate verification result: 0x00
    [wrn] MQTTConnect(204): NOT USING pre-malloced buf 0xc4a010, malloc per packet
    [dbg] MQTTConnect(204): ALLOC: curr buf = 0xc56890, curr buf_size = 320, required payload_len = 256
    [dbg] MQTTConnect(224): FREED: curr buf = (nil), curr buf_size = 0
    [inf] iotx_mc_connect(2449): mqtt connect success!
    ...
    ...
    mqtt_client|309 :: packet-id=7, publish topic msg={"attr_name":"temperature", "attr_value":"1"}
    [dbg] iotx_mc_cycle(1591): PUBACK
    event_handle|132 :: publish success, packet-id=7
    [dbg] iotx_mc_cycle(1608): PUBLISH
    [dbg] iotx_mc_handle_recv_PUBLISH(1412):         Packet Ident : 00035641
    [dbg] iotx_mc_handle_recv_PUBLISH(1413):         Topic Length : 26
    [dbg] iotx_mc_handle_recv_PUBLISH(1417):           Topic Name : /a1ExpAkj9Hi/Example1/data
    [dbg] iotx_mc_handle_recv_PUBLISH(1420):     Payload Len/Room : 45 / 992
    [dbg] iotx_mc_handle_recv_PUBLISH(1421):       Receive Buflen : 1024
    [dbg] iotx_mc_handle_recv_PUBLISH(1432): delivering msg ...
    [dbg] iotx_mc_deliver_message(1170): topic be matched
    _demo_message_arrive|166 :: ----
    _demo_message_arrive|167 :: packetId: 35641
    _demo_message_arrive|171 :: Topic: '/a1ExpAkj9Hi/Example1/data' (Length: 26)
    _demo_message_arrive|175 :: Payload: '{"attr_name":"temperature", "attr_value":"1"}' (Length: 45)
    _demo_message_arrive|176 :: ----
    ...
    ...
    main|361 :: out of sample!

*[回到目录](#目录)*
### <a name="观察基础版例程">观察基础版例程</a>

#### <a name="观察消息上报">观察消息上报</a>

如下日志信息显示样例程序正在通过`MQTT`的`Publish`类型消息, 上报业务数据到`/${prodcutKey}/${deviceName}/data`

    mqtt_client|309 :: packet-id=7, publish topic msg={"attr_name":"temperature", "attr_value":"1"}

#### <a name="观察消息下推">观察消息下推</a>

如下日志信息显示该消息因为是到达已被订阅的`Topic`, 所以又被服务器原样推送到基础版例程, 并进入相应的回调函数

    _demo_message_arrive|166 :: ----
    _demo_message_arrive|167 :: packetId: 35641
    _demo_message_arrive|171 :: Topic: '/a1ExpAkj9Hi/Example1/data' (Length: 26)
    _demo_message_arrive|175 :: Payload: '{"attr_name":"temperature", "attr_value":"1"}' (Length: 45)
    _demo_message_arrive|176 :: ----

#### <a name="观察控制台日志">观察控制台日志</a>

可以登录物联网套件控制台, 到**产品管理**, 找到刚才创建的产品, 点击**查看**, 选择**日志服务**选项卡, 可以看到刚才上报的消息(Message ID为7)

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E4%BA%A7%E5%93%81%E8%AF%A6%E6%83%85-%E6%97%A5%E5%BF%97%E6%9C%8D%E5%8A%A1.png)

*[回到目录](#目录)*
## <a name="2.4 体验高级版C-SDK">2.4 体验高级版C-SDK</a>

*[回到目录](#目录)*
### <a name="创建高级版产品和设备">创建高级版产品和设备</a>
进入IoT控制台后, 点击页面左侧导航栏的**产品管理**, 再点击右侧的**创建产品**, 如下图所示: 

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E5%88%9B%E5%BB%BA%E4%BA%A7%E5%93%81.png)

在弹出的创建产品中, 选择高级版, 填写产品信息: 

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-ADV-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E5%88%9B%E5%BB%BA%E4%BA%A7%E5%93%81-%E8%AF%A6%E7%BB%86%E5%8F%82%E6%95%B0.png)

在此需要填写以下信息: 

- 填写**产品名称**
- 选择**节点类型**
- 选择**设备类型**, 如果没有需要的设备类型, 可以填空, 稍后在产品**功能定义**时再定义具体的功能
- 选择**数据格式**, 目前有**Alink JSON**和**透传/自定义**两种格式可选(在这里我们使用Alink JSON格式作为示例)
    + **Alink JSON**: 使用标准的Alink协议格式来进行物联网套件与云端的服务、属性、事件的数据交换
    + **透传/自定义**: 使用物联网套件的透传接口来进行与云端的数据交换, 在云端需要用户完成透传数据与Alink格式的转换脚本
- **ID2**: 是否使用ID2认证

填写好产品信息后, 点击**确认**即可生成该产品: 

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-ADV-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E5%88%9B%E5%BB%BA%E4%BA%A7%E5%93%81-%E7%A1%AE%E8%AE%A4.png)

点击产品右侧的**查看**, 可跳转到产品详情页面: 

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-ADV-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E4%BA%A7%E5%93%81%E8%AF%A6%E6%83%85.png)

在该页面中, 有五个主要的选项卡: 

- 产品信息: 展示产品相关信息, 其中ProductKey用于标示产品的品类, 该产品下所有设备的ProductKey均一致
- 消息通信: 展示产品用于上下行数据的主要Topic
- 功能定义: 在这里可以定义设备的物模型, 定义物的服务、属性、事件
- 日志服务: 此处可浏览设备的历史上下行消息
- 在线调试: 此处可对该产品下的设备进行在线调试, 如下发服务到设备, 设置设备的属性, 观察设备的事件上报

产品创建好后, 接下来可以创建设备了, 点击**产品详情**页面中**设备数**旁的**前往管理**, 即可看到当前产品下的设备列表, 目前为空: 

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-ADV-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E4%BA%A7%E5%93%81%E8%AF%A6%E6%83%85-%E8%AE%BE%E5%A4%87%E7%AE%A1%E7%90%86.png)

点击上图右侧的**添加设备**, 开始创建设备: 

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-ADV-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E4%BA%A7%E5%93%81%E8%AF%A6%E6%83%85-%E8%AE%BE%E5%A4%87%E7%AE%A1%E7%90%86-%E6%B7%BB%E5%8A%A0%E8%AE%BE%E5%A4%87.png)

在填写好**DeviceName**后, 点击确认即可创建该设备, 生成设备的**三元组**: 

- `ProductKey`: 标识产品的品类, 相同产品的所有设备ProductKey均相同
- `DeviceName`: 标识产品下的每个设备, 相同产品的所有设备DeviceName均不相同
- `DeviceSecret`: 设备密钥, 每个设备均不相同

*三元组用于标识阿里云上的每个设备, 用于连接阿里云服务器时完成设备认证*

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-ADV-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E4%BA%A7%E5%93%81%E8%AF%A6%E6%83%85-%E8%AE%BE%E5%A4%87%E7%AE%A1%E7%90%86-%E6%B7%BB%E5%8A%A0%E8%AE%BE%E5%A4%87-%E7%94%9F%E6%88%90%E4%B8%89%E5%85%83%E7%BB%84.png)

至此, 高级版产品与设备创建完成.

*[回到目录](#目录)*
### <a name="为高级版设备创建物模型">为高级版设备创建物模型</a>

> 在高级版物联网平台中, 用户可以将物理空间中的实体, 例如各类传感器, 或者由传感器组成的设备等进行数字化, 并在云端构建该实体的数据模型
>
> 通过定义物模型(TSL)来对设备是什么, 能做什么, 可以对外提供哪些服务进行描述, 定义好物模型(TSL)也就定义好了产品功能

稍后会展示高级版服务、属性、事件的示例程序, 所以在这里我们回到**产品管理**, 选择我们刚才创建的产品, 进入**产品详情**页, 选择**功能定义**选项卡: 

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-ADV-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E4%BA%A7%E5%93%81%E8%AF%A6%E6%83%85-%E5%8A%9F%E8%83%BD%E5%AE%9A%E4%B9%89.png)

选择右侧的**新增**, 我们先创建一个属性: 

该属性是一个字符串属性, 最大长度为2048个字节

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-ADV-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E4%BA%A7%E5%93%81%E8%AF%A6%E6%83%85-%E5%8A%9F%E8%83%BD%E5%AE%9A%E4%B9%89-%E6%96%B0%E5%A2%9E%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%B1%9E%E6%80%A7.png)

点击**确认**, 这样一个属性就创建好了

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-ADV-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E4%BA%A7%E5%93%81%E8%AF%A6%E6%83%85-%E5%8A%9F%E8%83%BD%E5%AE%9A%E4%B9%89-%E6%96%B0%E5%A2%9E%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%B1%9E%E6%80%A7-%E5%88%9B%E5%BB%BA%E6%88%90%E5%8A%9F.png)

接下来我们创建一个服务: 

关于服务的创建要注意的是, 服务只能由服务端下发, 设备端被动接收, 每个服务有自己的输入参数和输出参数, 说明如下: 

- 服务的**输入参数**: 指的是当从云端下发服务时, 云端向设备端发送的数据内容
- 服务的**输出参数**: 指的是当设备端收到从云端下发的服务时, 如果有需要对该服务返回一些业务数据, 那么就是输出参数的内容

在这个服务中我们创建一个输入参数和一个输出参数

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-ADV-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E4%BA%A7%E5%93%81%E8%AF%A6%E6%83%85-%E5%8A%9F%E8%83%BD%E5%AE%9A%E4%B9%89-%E6%96%B0%E5%A2%9E%E6%9C%8D%E5%8A%A1.png)

最后, 再创建一个事件: 

> 关于事件要注意的是, 事件只能由设备端主动上报给服务端, 每个事件只有输出参数, 表示从设备端需要上报的数据内容

在这个事件中我们创建一个输出参数

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-ADV-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E4%BA%A7%E5%93%81%E8%AF%A6%E6%83%85-%E5%8A%9F%E8%83%BD%E5%AE%9A%E4%B9%89-%E6%96%B0%E5%A2%9E%E4%BA%8B%E4%BB%B6.png)

至此, 我们创建的产品中服务、属性、事件各有一个: 

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-ADV-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E4%BA%A7%E5%93%81%E8%AF%A6%E6%83%85-%E5%8A%9F%E8%83%BD%E5%AE%9A%E4%B9%89-%E5%AE%8C%E6%88%90.png)

*[回到目录](#目录)*
### <a name="编译运行高级版例程">编译运行高级版例程</a>

高级版以单品API为例演示设备服务/属性/事件的上报和下行

首先参考[下载解压物联网平台设备端C-SDK](#下载解压物联网平台设备端C-SDK)一节, 下载及解压得到C-SDK的源码, 然后再执行以下环节

#### <a name="下载设备物模型">下载设备物模型</a>
高级版单品的exmaple位于 `iotx-sdk-c/examples/linkkit/linkkit_deprecated_example_solo.c`, 该example用到的TSL位于同目录下的`example_tsl_solo.data`.

接下来我们需要将SDK默认的设备信息更换成我们在前一章节中创建的高级版设备, 需要替换设备的三元组以及设备的物模型描述(TSL)

进入**产品管理**, 选择我们刚才创建的设备`AdvUserExample`, 进入`产品详情`, 选择`功能定义`选项卡, 这里可以看到之前定义的服务、属性和事件.

点击右侧的`查看物模型`, 如下图所示: 

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E4%BA%A7%E5%93%81%E8%AF%A6%E6%83%85-%E7%89%A9%E6%A8%A1%E5%9E%8B.png)

点击`导出模型文件`将物模型导出, 则可以下载到JSON格式的, 设备的物模型(TSL)描述文件, **model.json**

#### <a name="填写设备物模型到例程中">填写设备物模型到例程中</a>

运行如下命令:

    make distclean
    make

在 `iotx-sdk-c/output/release/bin` 目录下会产生一个转义工具 `linkkit_tsl_convert`, 将刚才下载的物模型文件**model.json**拷贝到这里, 执行如下命令: 

    $ ./linkkit_tsl_convert -i model.json

命令执行成功后会在当前目录下产生一个`conv.txt`文件, 这个就是转义好的物模型字符串了. (转义工具网上很多, 也可自行寻找)

用`conv.txt`文件中的字符串替换`iotx-sdk-c/examples/linkkit/example_tsl_solo.data`中变量`TSL_STRING`的字符串即可.

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-ADV-%E5%8D%95%E5%93%81%E6%9B%BF%E6%8D%A2TSL.png)

#### <a name="填写设备三元组到例程中">填写设备三元组到例程中</a>
将 `iotx-sdk-c/examples/linkkit/linkkit_deprecated_example_solo.c` 中的三元组替换成刚才创建的设备

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-ADV-%E5%8D%95%E5%93%81%E6%9B%BF%E6%8D%A2%E4%B8%89%E5%85%83%E7%BB%84.png)

这样三元组和TSL都已换成我们刚才创建的设备.

之前我们在[为高级版设备创建物模型](#为高级版设备创建物模型)一节中, 定义了由一个服务, 一个属性和一个事件组成的物模型(TSL)

这些与当前 `linkkit_deprecated_example_solo.c` 中, 编写回调函数时对应的物模型(TSL)相同, 所以不需要对回调结构体 `linkkit_ops`, 及其成员回调函数做出修改.

#### <a name="编译高级版例程">编译高级版例程</a>

运行如下命令:

    $ make distclean
    $ make

编译成功完成后, 生成的高级版例子程序在当前路径的 `output/release/bin` 目录下, 名为`linkkit-example-solo`

#### <a name="运行高级版例程">运行高级版例程</a>
执行如下命令:

    cd ~/srcs/iotx-sdk-c
    ./output/release/bin/linkkit-example-solo

得到运行结果如下:

    linkkit_example|587 :: linkkit start
    [dbg] _dm_mgr_search_dev_by_devid(52): Device Not Found, devid: 0
    [inf] iotx_cm_init(77): cm verstion 0.3
    [inf] iotx_device_info_init(39): device_info created successfully!
    [dbg] iotx_device_info_set(49): start to set device info!
    [dbg] iotx_device_info_set(63): device_info set successfully!
    [inf] guider_print_dev_guider_info(279): ....................................................
    [inf] guider_print_dev_guider_info(280):           ProductKey : a1csED27mp7
    [inf] guider_print_dev_guider_info(281):           DeviceName : AdvExample1
    [inf] guider_print_dev_guider_info(282):             DeviceID : a1csED27mp7.AdvExample1
    [inf] guider_print_dev_guider_info(284): ....................................................
    [inf] guider_print_dev_guider_info(285):        PartnerID Buf : ,partner_id=example.demo.partner-id
    [inf] guider_print_dev_guider_info(286):         ModuleID Buf : ,module_id=example.demo.module-id
    [inf] guider_print_dev_guider_info(287):           Guider URL :
    [inf] guider_print_dev_guider_info(289):       Guider SecMode : 2 (TLS + Direct)
    [inf] guider_print_dev_guider_info(291):     Guider Timestamp : 2524608000000
    [inf] guider_print_dev_guider_info(292): ....................................................
    [inf] guider_print_dev_guider_info(298): ....................................................
    [inf] guider_print_conn_info(256): -----------------------------------------
    [inf] guider_print_conn_info(257):             Host : a1csED27mp7.iot-as-mqtt.cn-shanghai.aliyuncs.com
    [inf] guider_print_conn_info(258):             Port : 1883
    [inf] guider_print_conn_info(261):         ClientID : a1csED27mp7.AdvExample1|securemode=2,timestamp=2524608000000,signmethod=hmacsha1 ...
    [inf] guider_print_conn_info(263):       TLS PubKey : 0x47ffd6 ('-----BEGIN CERTI ...')
    [inf] guider_print_conn_info(266): -----------------------------------------
    [inf] iotx_cm_init(126): cm context initialize
    [inf] linked_list_insert(120): linked list(cm event_cb list) insert node@0x18a2760,size:1
    [inf] linked_list_insert(120): linked list(cm connectivity list) insert node@0x18a28c0,size:1
    [err] iotx_cm_add_connectivity(113): Add Connectivity Success, Type: 1
    [inf] iotx_cm_conn_mqtt_init(358):            CONFIG_MQTT_TX_MAXLEN : 1024
    [inf] iotx_cm_conn_mqtt_init(359):            CONFIG_MQTT_RX_MAXLEN : 5000
    [wrn] iotx_cm_conn_mqtt_init(369): WITH_MQTT_DYNBUF = 1, skipping malloc sendbuf of 1024 bytes
    [inf] IOT_MQTT_Construct(3005):      CONFIG_MQTT_SUBTOPIC_MAXNUM : 65535
    [dbg] IOT_MQTT_Construct(3007): sizeof(iotx_mc_client_t) = 1573144!
    [inf] iotx_mc_init(2098): MQTT init success!
    [inf] _ssl_client_init(142): Loading the CA root certificate ...
    [inf] _ssl_client_init(149):  ok (0 skipped)
    [inf] _TLSConnectNetwork(315): Connecting to /a1csED27mp7.iot-as-mqtt.cn-shanghai.aliyuncs.com/1883...
    [inf] mbedtls_net_connect_timeout(257): setsockopt SO_SNDTIMEO timeout: 10s
    [inf] mbedtls_net_connect_timeout(260): connecting IP_ADDRESS: 106.15.100.2
    [inf] _TLSConnectNetwork(328):  ok
    [inf] _TLSConnectNetwork(333):   . Setting up the SSL/TLS structure...
    [inf] _TLSConnectNetwork(343):  ok
    [inf] _TLSConnectNetwork(382): Performing the SSL/TLS handshake...
    [inf] _TLSConnectNetwork(390):  ok
    [inf] _TLSConnectNetwork(394):   . Verifying peer X.509 certificate..
    [inf] _real_confirm(90): certificate verification result: 0x00
    [dbg] MQTTConnect(204): ALLOC: curr buf = 0x18af8f0, curr buf_size = 320, required payload_len = 256
    [dbg] MQTTConnect(224): FREED: curr buf = (nil), curr buf_size = 0
    [inf] iotx_mc_connect(2449): mqtt connect success!
    ...
    ...

*[回到目录](#目录)*
### <a name="观察高级版例程">观察高级版例程</a>
#### <a name="观察属性的上报">观察属性的上报</a>
对于属性(Property), 示例程序会每隔30s上报(Post)一次所有属性, 因为我们定义了一个属性 `DeviceStatus`, 所以应该看到如下日志: 

    ```
    [dbg] iotx_dm_post_property_end(450): Current Property Post Payload, Length: 19, Payload: {"DeviceStatus":""}
    [dbg] _dm_mgr_search_dev_by_devid(46): Device Found, devid: 0
    [inf] dm_msg_request_all(265): DM Send Message, URI: /sys/a1csED27mp7/AdvExample1/thing/event/property/post, Payload: {"id":"1","version":"1.0","params":{"DeviceStatus":""},"method":"thing.event.property.post"}
    [inf] iotx_cm_conn_mqtt_publish(531): mqtt publish: topic=/sys/a1csED27mp7/AdvExample1/thing/event/property/post, topic_msg={"id":"4","version":"1.0","params":{"DeviceStatus":""},"method":"thing.event.property.post"}
    ```

可以看出, 由于我们尚未设置该属性的值, 所以默认该值为空字符串, 此时在物联网控制台上可以查询到刚才的上报记录: 

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-ADV-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E4%BA%A7%E5%93%81%E8%AF%A6%E6%83%85-%E6%97%A5%E5%BF%97%E6%9C%8D%E5%8A%A1-%E5%B1%9E%E6%80%A7%E4%B8%8A%E6%8A%A5.png)

从上图可以看出, 一条 `Property Post` 消息已上报至服务端

#### <a name="观察属性的设置">观察属性的设置</a>

在高级版中, 也可从服务端主动向这个属性(Property)设置(Set)一个值

打开`产品管理`->`产品详情`->`在线调试选项卡`, 选择我们要调试的设备: 

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-ADV-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E4%BA%A7%E5%93%81%E8%AF%A6%E6%83%85-%E8%AE%BE%E5%A4%87%E8%B0%83%E8%AF%95-%E8%AE%BE%E7%BD%AE%E5%B1%9E%E6%80%A7.png)

如上图所示, 选择 `DeviceStatus` 这个属性, 然后选择**设置**, 在下方的输入框中将"Hello World"填入该属性的值, 然后点击发送指令,

此时在设备端的日志中可以看到从服务端set下来的值: 

    [dbg] iotx_mc_cycle(1608): PUBLISH
    [dbg] iotx_mc_handle_recv_PUBLISH(1412):         Packet Ident : 00000000
    [dbg] iotx_mc_handle_recv_PUBLISH(1413):         Topic Length : 55
    [dbg] iotx_mc_handle_recv_PUBLISH(1417):           Topic Name : /sys/a1csED27mp7/AdvExample1/thing/service/property/set
    [dbg] iotx_mc_handle_recv_PUBLISH(1420):     Payload Len/Room : 112 / 4940
    [dbg] iotx_mc_handle_recv_PUBLISH(1421):       Receive Buflen : 5000
    [dbg] iotx_mc_handle_recv_PUBLISH(1432): delivering msg ...
    [dbg] iotx_mc_deliver_message(1170): topic be matched
    [inf] iotx_cloud_conn_mqtt_event_handle(180): event_type 12
    [inf] iotx_cloud_conn_mqtt_event_handle(325): mqtt received: topic=/sys/a1csED27mp7/AdvExample1/thing/service/property/set, topic_msg={"method":"thing.service.property.set","id":"65822254","params":{"DeviceStatus":"HelloWorld"},"version":"1.0.0"}

这样, 一条从服务端设置属性的命令就到达设备端了

又由于在例程里, 收到这条属性的设置指令之后, 会调用 `linkkit_post_property` 将该属性值上报给服务端, 所以有如下的日志打印: 

    [dbg] iotx_dm_post_property_end(450): Current Property Post Payload, Length: 29, Payload: {"DeviceStatus":"HelloWorld"}
    [dbg] _dm_mgr_search_dev_by_devid(46): Device Found, devid: 0
    [inf] dm_msg_request_all(265): DM Send Message, URI: /sys/a1csED27mp7/AdvExample1/thing/event/property/post, Payload: {"id":"34","version":"1.0","params":{"DeviceStatus":"HelloWorld"},"method":"thing.event.property.post"}
    [inf] iotx_cm_conn_mqtt_publish(531): mqtt publish: topic=/sys/a1csED27mp7/AdvExample1/thing/event/property/post, topic_msg={"id":"34","version":"1.0","params":{"DeviceStatus":"HelloWorld"},"method":"thing.event.property.post"}

> 注意: 只有当设备端主动上报某个属性的值到服务端后, 才能在服务端查询到该属性的值.

在**设备调试**选项卡中, 选择 `DeviceStatus` 这个属性, 然后选择**获取**, 点击**发送指令**, 稍后在下方的输入框中就可以看到刚才设置的属性了:

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-ADV-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E4%BA%A7%E5%93%81%E8%AF%A6%E6%83%85-%E8%AE%BE%E5%A4%87%E8%B0%83%E8%AF%95-%E8%8E%B7%E5%8F%96%E5%B1%9E%E6%80%A7.png)

#### <a name="观察事件的上报">观察事件的上报</a>

示例程序中 `Error` 事件(Event)是每45s上报一次, 日志如下: 

    [inf] dm_msg_request_all(265): DM Send Message, URI: /sys/a1csED27mp7/AdvExample1/thing/event/Error/post, Payload: {"id":"36","version":"1.0","params":{"ErrorCode":0},"method":"thing.event.Error.post"}
    [inf] iotx_cm_conn_mqtt_publish(531): mqtt publish: topic=/sys/a1csED27mp7/AdvExample1/thing/event/Error/post, topic_msg={"id":"36","version":"1.0","params":{"ErrorCode":0},"method":"thing.event.Error.post"}

相应地, 在物联网控制台的**日志服务**选项卡中可以看到对应的信息: 

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-ADV-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E4%BA%A7%E5%93%81%E8%AF%A6%E6%83%85-%E8%AE%BE%E5%A4%87%E8%B0%83%E8%AF%95-%E4%BA%8B%E4%BB%B6%E4%B8%8A%E6%8A%A5.png)

在物联网控制台的**在线调试**选项卡中可以看到对应的信息:

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-ADV-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E4%BA%A7%E5%93%81%E8%AF%A6%E6%83%85-%E8%AE%BE%E5%A4%87%E8%B0%83%E8%AF%95-%E8%8E%B7%E5%8F%96%E4%BA%8B%E4%BB%B6.png)

#### <a name="观察服务的下发">观察服务的下发</a>

在物联网控制台中打开**设备调试**选项卡, 选择我们创建的服务`Custom`

由于该服务的输入参数数据类型为int型, 标识为 `transparency`, 所以在下方的输入框中填入参数, 并点击**发送指令**: 

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-ADV-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E4%BA%A7%E5%93%81%E8%AF%A6%E6%83%85-%E8%AE%BE%E5%A4%87%E8%B0%83%E8%AF%95-%E6%9C%8D%E5%8A%A1%E4%B8%8B%E5%8F%91.png)

此时在设备端可以看到如下日志: 

    [dbg] iotx_mc_cycle(1608): PUBLISH
    [dbg] iotx_mc_handle_recv_PUBLISH(1412):         Packet Ident : 00000000
    [dbg] iotx_mc_handle_recv_PUBLISH(1413):         Topic Length : 49
    [dbg] iotx_mc_handle_recv_PUBLISH(1417):           Topic Name : /sys/a1csED27mp7/AdvExample1/thing/service/Custom
    [dbg] iotx_mc_handle_recv_PUBLISH(1420):     Payload Len/Room : 95 / 4946
    [dbg] iotx_mc_handle_recv_PUBLISH(1421):       Receive Buflen : 5000
    [dbg] iotx_mc_handle_recv_PUBLISH(1432): delivering msg ...

我们可以看到, 设备端example已经收到从云端下发的服务 `Custom`, 其中服务的输入参数 `transparency` 的值为5

    [dbg] iotx_mc_deliver_message(1170): topic be matched
    [inf] iotx_cloud_conn_mqtt_event_handle(180): event_type 12
    [inf] iotx_cloud_conn_mqtt_event_handle(325): mqtt received: topic=/sys/a1csED27mp7/AdvExample1/thing/service/Custom, topic_msg={"method":"thing.service.Custom","id":"65850626","params":{"transparency":5},"version":"1.0.0"}
    [inf] iotx_cm_cloud_conn_response_callback(121): rsp_type 11
    [inf] iotx_cm_cloud_conn_response_handler(525): URI = /sys/a1csED27mp7/AdvExample1/thing/service/Custom{"method":"thing.service.Custom","id":"65850626","params":{"transparency":5},"version":"1.0.0"}
    [inf] dm_disp_event_new_data_received_handler(1289): IOTX_CM_EVENT_NEW_DATA_RECEIVED

而在example中, 当收到服务 `Custom` 的请求后, 进入相应的处理回调函数 `handle_service_custom()` 中

接着会将该输入参数的值+1赋给输出参数 `Contrastratio`, 从上面的日志中可以看到`Contrastratio`的值被设成了6, 并被上报给服务端.

    [inf] dm_cmw_topic_callback(13): DMGR TOPIC CALLBACK
    [inf] dm_cmw_topic_callback(20): DMGR Receive Message: /sys/a1csED27mp7/AdvExample1/thing/service/Custom{"method":"thing.service.Custom","id":"65850626","params":{"transparency":5},"version":"1.0.0"}
    ...
    ...
    [dbg] iotx_dm_send_service_response(597): Current Service Response Payload, Length: 19, Payload: {"Contrastratio":6}
    [dbg] _dm_mgr_search_dev_by_devid(46): Device Found, devid: 0
    [dbg] dm_mgr_upstream_thing_service_response(2098): Current Service Name: thing/service/Custom_reply
    [dbg] dm_msg_response_with_data(384): Send URI: /sys/a1csED27mp7/AdvExample1/thing/service/Custom_reply, Payload: {"id":"65850626","code":200,"data":{"Contrastratio":6}}
    [inf] iotx_cm_conn_mqtt_publish(531): mqtt publish: topic=/sys/a1csED27mp7/AdvExample1/thing/service/Custom_reply, topic_msg={"id":"65850626","code":200,"data":{"Contrastratio":6}}

关于高级版单品例程中服务/属性/事件的说明就此结束



第三章目录
---
+ [第三章 移植指南](#第三章 移植指南)
    * [3.1 在Ubuntu上编译主机版本](#3.1 在Ubuntu上编译主机版本)
        - [正常的编译过程演示](#正常的编译过程演示)
        - [得到的编译产物说明](#得到的编译产物说明)
    * [3.2 交叉编译到嵌入式硬件平台](#3.2 交叉编译到嵌入式硬件平台)
        - [安装交叉编译工具链](#安装交叉编译工具链)
        - [添加配置文件](#添加配置文件)
        - [编辑配置文件](#编辑配置文件)
        - [选择配置文件](#选择配置文件)
        - [交叉编译产生库文件`libiot_sdk.a`](#交叉编译产生库文件`libiot_sdk.a`)
        - [获取交叉编译的产物, 包括静态库和头文件](#获取交叉编译的产物, 包括静态库和头文件)
    * [3.3 开发未适配平台的HAL层](#3.3 开发未适配平台的HAL层)
        - [复制一份HAL层实现代码](#复制一份HAL层实现代码)
        - [打开之前被关闭的编译开关](#打开之前被关闭的编译开关)
        - [尝试交叉编译被复制的HAL层代码](#尝试交叉编译被复制的HAL层代码)
        - [允许交叉编译样例程序](#允许交叉编译样例程序)
        - [重新载入配置文件, 交叉编译可执行程序](#重新载入配置文件, 交叉编译可执行程序)
        - [尝试运行样例程序](#尝试运行样例程序)

*[回到目录](#目录)*
# <a name="第三章 移植指南">第三章 移植指南</a>

> 物联网平台C-SDK以全开源的纯C语言编写而成, 旨在提供与目标平台硬件CPU体系架构无关, 与目标平台嵌入式OS操作系统无关的跨平台SDK

为此我们建议遵循如下的开发流程, 将C-SDK源码适配移植到您需要接入到阿里云物联网平台的嵌入式硬件上

+ **在Ubuntu上编译主机版本:** 这一步并不进行交叉编译, 而是希望您可以体验主机(X86)版本的SDK及其例程, 熟悉SDK的编译过程和产物
+ **交叉编译到嵌入式硬件平台:** 接着可以安装目标平台的编译工具链, 按本文说明, 交叉编译嵌入式体系架构的二进制库 `libiot_sdk.a`
+ **了解C-SDK的构建系统使用:** 在交叉编译环节, 您将接触到C-SDK的构建配置系统, 此时可阅读[第四章 构建配置系统](#第四章 构建配置系统), 全面了解其用法
+ **开发未适配平台的HAL层:** 要使用以上步骤中产生的 `libiot_sdk.a` 所提供的API, 您还需要为C-SDK提供[第五章 HAL接口说明](#第五章 HAL接口说明)中列出的HAL接口实现

---
本文为了快速走通以上流程, 简写了第3步和第4步, 以移植到 `arm-linux` 平台为例, 直接演示了一个完整的移植过程

*[回到目录](#目录)*
## <a name="3.1 在Ubuntu上编译主机版本">3.1 在Ubuntu上编译主机版本</a>

具体步骤是:

    $ make distclean
    $ make

即可得到`libiot_sdk.a`

*[回到目录](#目录)*
### <a name="正常的编译过程演示">正常的编译过程演示</a>

    $ make distclean
    $ make
    SELECTED CONFIGURATION:

    VENDOR :   ubuntu
    MODEL  :   x86


    CONFIGURE .............................. [examples]
    CONFIGURE .............................. [src/infra/log]
    CONFIGURE .............................. [src/infra/system]
    CONFIGURE .............................. [src/infra/utils]
    CONFIGURE .............................. [src/protocol/alcs]
    CONFIGURE .............................. [src/protocol/coap]
    CONFIGURE .............................. [src/protocol/http]
    CONFIGURE .............................. [src/protocol/http2]
    CONFIGURE .............................. [src/protocol/mqtt]
    CONFIGURE .............................. [src/ref-impl/hal]
    CONFIGURE .............................. [src/ref-impl/tls]
    CONFIGURE .............................. [src/sdk-impl]
    CONFIGURE .............................. [src/services/file_upload]
    CONFIGURE .............................. [src/services/linkkit/cm]
    CONFIGURE .............................. [src/services/linkkit/dm]
    CONFIGURE .............................. [src/services/shadow]
    CONFIGURE .............................. [src/services/subdev]
    CONFIGURE .............................. [src/services/uOTA]
    CONFIGURE .............................. [src/tools/linkkit_tsl_convert]
    CONFIGURE .............................. [tests]

    BUILDING WITH EXISTING CONFIGURATION:

    VENDOR :   ubuntu
    MODEL  :   x86

    Components: 

    . tests                                             
    . src/services/file_upload                                          
    . src/services/subdev                                          
    . src/services/uOTA                                          
    . src/services/shadow                                          
    . src/services/linkkit/dm                                          
    . src/services/linkkit/cm                                          
    . src/sdk-impl                                          
    . src/protocol/mqtt                                          
    . src/protocol/coap                                          
    . src/protocol/http2                                          
    . src/protocol/alcs                                          
    . src/protocol/http                                          
    . src/tools/linkkit_tsl_convert                                          
    . src/ref-impl/tls                                          
    . src/ref-impl/hal                                          
    . src/infra/log                                          
    . src/infra/system                                          
    . src/infra/utils                                          
    . examples                                          

    [CC] utils_list.o                       <=  ...
    [CC] json_parser.o                      <=  ...
    [CC] ota_service.o                      <=  ...
    [CC] sdk-impl.o                         <=  ...
    [CC] json_token.o                       <=  ...
    [CC] utils_event.o                      <=  ...
    [CC] utils_hmac.o                       <=  ...
    [CC] linked_list.o                      <=  ...
    [CC] string_utils.o                     <=  ...
    [CC] MQTTDeserializePublish.o           <=  ...
    [CC] utils_timer.o                      <=  ...
    [CC] MQTTUnsubscribeClient.o            <=  ...
    [CC] utils_sha256.o                     <=  ...
    [CC] mem_stats.o                        <=  ...
    [CC] iotx_log.o                         <=  ...
    [CC] lite-cjson.o                       <=  ...
    [CC] utils_net.o                        <=  ...
    [CC] utils_epoch_time.o                 <=  ...
    [CC] ota_download_coap.o                <=  ...
    [CC] lite_queue.o                       <=  ...
    [CC] MQTTSubscribeClient.o              <=  ...
    [CC] ota.o                              <=  ...
    [CC] ota_hal_os.o                       <=  ...
    [CC] ota_manifest.o                     <=  ...
    [CC] utils_md5.o                        <=  ...
    [CC] utils_httpc.o                      <=  ...
    [CC] ota_hal_plat.o                     <=  ...
    [CC] utils_sha1.o                       <=  ...
    [CC] mqtt_instance.o                    <=  ...
    [CC] MQTTPacket.o                       <=  ...
    [CC] guider.o                           <=  ...
    [CC] crc.o                              <=  ...
    [CC] utils_base64.o                     <=  ...
    [CC] alcs_client.o                      <=  ...
    [CC] MQTTConnectClient.o                <=  ...
    [CC] ota_socket.o                       <=  ...
    [CC] ota_hal_module.o                   <=  ...
    [CC] alcs_api.o                         <=  ...
    [CC] ca.o                               <=  ...
    [CC] CoAPSerialize.o                    <=  ...
    [CC] alcs_mqtt.o                        <=  ...
    [CC] ota_verify.o                       <=  ...
    [CC] mqtt_client.o                      <=  ...
    [CC] ota_transport_mqtt.o               <=  ...
    [CC] iotx_http_api.o                    <=  ...
    [CC] device.o                           <=  ...
    [CC] report.o                           <=  ...
    [CC] alcs_server.o                      <=  ...
    [CC] CoAPDeserialize.o                  <=  ...
    [CC] CoAPMessage.o                      <=  ...
    [CC] CoAPExport.o                       <=  ...
    [CC] CoAPNetwork.o                      <=  ...
    [CC] ota_version.o                      <=  ...
    [CC] CoAPServer.o                       <=  ...
    [CC] ota_util.o                         <=  ...
    [CC] ota_transport_coap.o               <=  ...
    [CC] CoAPPlatform.o                     <=  ...
    [CC] ota_download_http.o                <=  ...
    [CC] CoAPObserve.o                      <=  ...
    [CC] MQTTSerializePublish.o             <=  ...
    [CC] alcs_adapter.o                     <=  ...
    [CC] alcs_coap.o                        <=  ...
    [CC] CoAPResource.o                     <=  ...
    [CC] iotx_cm_api.o                      <=  ...
    [CC] iotx_cm_cloud_conn.o               <=  ...
    [CC] iotx_cm_common.o                   <=  ...
    [CC] iotx_cm_conn_coap.o                <=  ...
    [CC] iotx_cm_connectivity.o             <=  ...
    [CC] iotx_cm_conn_http.o                <=  ...
    [CC] iotx_cm_conn_mqtt.o                <=  ...
    [CC] dm_api.o                           <=  ...
    [CC] iotx_cm_log.o                      <=  ...
    [CC] iotx_cm_local_conn.o               <=  ...
    [CC] iotx_local_conn_alcs.o             <=  ...
    [CC] dm_cota.o                          <=  ...
    [CC] dm_cm_wrapper.o                    <=  ...
    [CC] dm_dispatch.o                      <=  ...
    [CC] dm_ipc.o                           <=  ...
    [CC] dm_fota.o                          <=  ...
    [CC] dm_conn.o                          <=  ...
    [CC] dm_message.o                       <=  ...
    [CC] dm_manager.o                       <=  ...
    [CC] dm_opt.o                           <=  ...
    [CC] dm_shadow.o                        <=  ...
    [CC] dm_subscribe.o                     <=  ...
    [CC] dm_message_cache.o                 <=  ...
    [CC] dm_ota.o                           <=  ...
    [CC] dm_utils.o                         <=  ...
    [CC] linkkit_gateway_legacy.o           <=  ...
    [CC] linkkit_solo_legacy.o              <=  ...
    [CC] dm_tsl_alink.o                     <=  ...
    [CC] base64.o                           <=  ...
    [CC] HAL_Crypt_Linux.o                  <=  ...
    [CC] HAL_OS_linux.o                     <=  ...
    [CC] HAL_TCP_linux.o                    <=  ...
    [CC] cJSON.o                            <=  ...
    [CC] HAL_DTLS_mbedtls.o                 <=  ...
    [CC] HAL_TLS_mbedtls.o                  <=  ...
    [CC] HAL_UDP_linux.o                    <=  ...
    [CC] pk_wrap.o                          <=  ...
    [CC] kv.o                               <=  ...
    [CC] platform.o                         <=  ...
    [CC] ctr_drbg.o                         <=  ...
    [CC] cipher_wrap.o                      <=  ...
    [CC] entropy.o                          <=  ...
    [CC] pk.o                               <=  ...
    [CC] error.o                            <=  ...
    [CC] sha1.o                             <=  ...
    [CC] oid.o                              <=  ...
    [CC] cipher.o                           <=  ...
    [CC] entropy_poll.o                     <=  ...
    [CC] pem.o                              <=  ...
    [CC] base64.o                           <=  ...
    [CC] asn1parse.o                        <=  ...
    [CC] rsa.o                              <=  ...
    [CC] md_wrap.o                          <=  ...
    [CC] ssl_tls.o                          <=  ...
    [CC] md.o                               <=  ...
    [CC] x509.o                             <=  ...
    [CC] x509_crt.o                         <=  ...
    [CC] sha256.o                           <=  ...
    [CC] net_sockets.o                      <=  ...
    [CC] ssl_ciphersuites.o                 <=  ...
    [CC] aes.o                              <=  ...
    [CC] ssl_cli.o                          <=  ...
    [CC] bignum.o                           <=  ...
    [CC] debug.o                            <=  ...
    [CC] pkparse.o                          <=  ...
    [CC] md5.o                              <=  ...
    [CC] timing.o                           <=  ...
    [CC] ssl_cookie.o                       <=  ...
    [CC] ssl.o                              <=  ...
    [CC] hash.o                             <=  ...
    [AR] libiot_hal.a                       <=  ...
    [AR] libiot_tls.a                       <=  ...
    [AR] libiot_sdk.a                       <=  ...
    [LD] linkkit-example-solo               <=  ...
    [LD] mqtt_multi_thread-example          <=  ...
    [LD] ota_mqtt-example                   <=  ...
    [LD] linkkit_tsl_convert                <=  ...
    [LD] mqtt-example                       <=  ...
    [LD] uota_app-example                   <=  ...
    [LD] sdk-testsuites                     <=  ...
    [LD] mqtt_rrpc-example                  <=  ...
    [LD] http-example                       <=  ...
    [LD] linkkit-example-sched              <=  ...

*[回到目录](#目录)*
### <a name="得到的编译产物说明">得到的编译产物说明</a>

SDK编译的产物在编译成功之后都存放在 `output` 目录下:

    $ tree -A output/
    output/
    +-- release
        +-- bin
        |   +-- http-example
        |   +-- linkkit-example-sched
        |   +-- linkkit-example-solo
        |   +-- linkkit_tsl_convert
        |   +-- mqtt-example
        |   +-- mqtt_multi_thread-example
        |   +-- mqtt_rrpc-example
        |   +-- ota_mqtt-example
        |   +-- sdk-testsuites
        |   +-- uota_app-example
        +-- include
        |   +-- exports
        |   |   +-- iot_export_alcs.h
        |   |   +-- iot_export_coap.h
        |   |   +-- iot_export_errno.h
        |   |   +-- iot_export_event.h
        |   |   +-- iot_export_file_uploader.h
        |   |   +-- iot_export_http2.h
        |   |   +-- iot_export_http.h
        |   |   +-- iot_export_mqtt.h
        |   |   +-- iot_export_ota.h
        |   |   +-- iot_export_shadow.h
        |   |   +-- iot_export_subdev.h
        |   |   +-- linkkit_export.h
        |   |   +-- linkkit_gateway_export.h
        |   +-- imports
        |   |   +-- iot_import_awss.h
        |   |   +-- iot_import_coap.h
        |   |   +-- iot_import_config.h
        |   |   +-- iot_import_crypt.h
        |   |   +-- iot_import_dtls.h
        |   |   +-- iot_import_product.h
        |   +-- iot_export.h
        |   +-- iot_import.h
        +-- lib
            +-- libalicrypto.a
            +-- libid2client.a
            +-- libiot_hal.a
            +-- libiot_sdk.a
            +-- libiot_tls.a
            +-- libitls.a
            +-- libkm.a
            +-- libmbedcrypto.a
            +-- libplat_gen.a

可做说明为:

| 产物                                                  | 说明
|-------------------------------------------------------|----------------------------------------------------------------------------------
| output/release/bin/*                                  | 例子程序, 在Ubuntu上运行, 并对照阅读 `examples/` 目录下的源代码, 以体验SDK的功能
| output/release/include/iot_export.h                   | 集中存放了所有SDK向外界提供的顶层用户接口声明, 命名方式为`IOT_XXX_YYY()`和`linkkit_mmm_nnn()`, 并且它也包含了所有`exports/`目录下的文件
| output/release/include/exports/*.h                    | 按不同的子功能分开列出各个子功能所提供的用户接口声明, 比如`iot_export_coap.h`就列出的是SDK提供CoAP相关功能时的可用接口
| output/release/include/imports/iot_import_config.h    | 配置头文件, 集中存放SDK的伸缩属性的可配置项, 比如`CONFIG_MQTT_TX_MAXLEN`表示给MQTT上行报文可以开辟的最大内存缓冲区长度等
| output/release/include/iot_import.h                   | 集中存放了所有SDK依赖外界提供的底层支撑接口声明, 命名方式为`HAL_XXX_YYY()`, 并且它也包含了所有`imports/`目录下的文件
| output/release/include/imports/*.h                    | 按不同的子功能分开列出各个子功能特殊引入的HAL接口的声明, 比如`iot_import_awss.h`就列出的是SDK因为提供配网功能而需要的HAL接口
| output/lib/libiot_sdk.a                               | SDK主库, 集中提供了所有用户接口的实现, 它的上层是用户业务逻辑, 下层是`libiot_hal.a`
| output/lib/libiot_hal.a                               | HAL主库, 集中提供了所有`HAL_XXX_YYY()`接口的实现, 它的上层是`libiot_sdk.a`, 下层是`libiot_tls.a`
| output/lib/libiot_tls.a                               | TLS主库, 集中提供了所有`mbedtls_xxx_yyy()`接口的实现, 它的上层是`libiot_hal.a`
| output/lib/*.a                                        | 其它分库, 它们是从SDK源码目录的`prebuilt/`目录移动过来的, 主要提供一些闭源发布的功能, 比如`ID2`等

*[回到目录](#目录)*
## <a name="3.2 交叉编译到嵌入式硬件平台">3.2 交叉编译到嵌入式硬件平台</a>

对于嵌入式硬件平台的情况, 对编译出目标平台的`libiot_sdk.a`, 需要经历如下几个步骤:

- 在`src/board/`目录下添加一个对应的配置文件, 文件名规范为`config.XXX.YYY`, 其中`XXX`部分就对应后面`src/ref-impl/hal/os/XXX`目录的HAL层代码
- 在配置文件中, 至少要指定:
    * 交叉编译器 `OVERRIDE_CC` 的路径
    * 交叉链接器 `OVERRIDE_LD` 的路径
    * 静态库压缩器 `OVERRIDE_AR` 的路径
    * 编译选项 `CONFIG_ENV_CFLAGS`, 用于C文件的编译
    * 链接选项 `CONFIG_ENV_LDFLAGS`, 用于可执行程序的链接

- 尝试编译SDK, 对可能出现的跨平台问题进行修正, 直到成功产生目标格式的`libiot_sdk.a`
- 最后, 您需要以任何编译方式, 产生目标架构的`libiot_hal.a`
- 若目标平台尚未被适配, 则`libiot_hal.a`对应的源代码在C-SDK中并未包含, 需要您根据[第五章 HAL接口说明](#第五章 HAL接口说明)自行实现`HAL_*()`接口

---
下面以某款目前未官方适配的 `arm-linux` 目标平台为例, 演示如何编译出该平台上可用的`libiot_sdk.a`

*[回到目录](#目录)*
### <a name="安装交叉编译工具链">安装交叉编译工具链</a>

> 仍以Ubuntu16.04开发环境为例

    $ sudo apt-get install -y gcc-arm-linux-gnueabihf
    $ arm-linux-gnueabihf-gcc --version

    arm-linux-gnueabihf-gcc (Ubuntu/Linaro 5.4.0-6ubuntu1~16.04.9) 5.4.0 20160609
    Copyright (C) 2015 Free Software Foundation, Inc.
    This is free software; see the source for copying conditions.  There is NO
    warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

*[回到目录](#目录)*
### <a name="添加配置文件">添加配置文件</a>

    $ touch src/board/config.arm-linux.demo
    $ ls src/board/
    config.arm-linux.demo  config.macos.make  config.rhino.make  config.ubuntu.x86  config.win7.mingw32

*[回到目录](#目录)*
### <a name="编辑配置文件">编辑配置文件</a>

在这一步, 需要设置编译选项和工具链, 以及跳过编译的目录

    $ vim src/board/config.arm-linux.demo

    CONFIG_ENV_CFLAGS = \
        -D_PLATFORM_IS_LINUX_ \
        -Wall

    OVERRIDE_CC = arm-linux-gnueabihf-gcc
    OVERRIDE_AR = arm-linux-gnueabihf-ar
    OVERRIDE_LD = arm-linux-gnueabihf-ld

    CONFIG_src/ref-impl/hal         :=
    CONFIG_examples                 :=
    CONFIG_tests                    :=
    CONFIG_src/tools/linkkit_tsl_convert :=

注意, 倒数4行表示对`src/ref-impl/hal`, `examples`, `tests`, `src/tools/linkkit_tsl_convert`这些目录跳过编译, 在编译未被适配平台的库时, 这在最初是必要的

*[回到目录](#目录)*
### <a name="选择配置文件">选择配置文件</a>

    $ make reconfig
    SELECT A CONFIGURATION:

    1) config.arm-linux.demo  3) config.rhino.make      5) config.win7.mingw32
    2) config.macos.make      4) config.ubuntu.x86
    #? 1

    SELECTED CONFIGURATION:

    VENDOR :   arm-linux
    MODEL  :   demo

    CONFIGURE .............................. [examples]
    CONFIGURE .............................. [src/infra/log]
    CONFIGURE .............................. [src/infra/system]
    CONFIGURE .............................. [src/infra/utils]
    CONFIGURE .............................. [src/protocol/alcs]
    CONFIGURE .............................. [src/protocol/coap]
    CONFIGURE .............................. [src/protocol/http]
    CONFIGURE .............................. [src/protocol/http2]
    CONFIGURE .............................. [src/protocol/mqtt]
    CONFIGURE .............................. [src/ref-impl/hal]
    CONFIGURE .............................. [src/ref-impl/tls]
    CONFIGURE .............................. [src/sdk-impl]
    CONFIGURE .............................. [src/services/file_upload]
    CONFIGURE .............................. [src/services/linkkit/cm]
    CONFIGURE .............................. [src/services/linkkit/dm]
    CONFIGURE .............................. [src/services/shadow]
    CONFIGURE .............................. [src/services/subdev]
    CONFIGURE .............................. [src/services/uOTA]
    CONFIGURE .............................. [src/tools/linkkit_tsl_convert]
    CONFIGURE .............................. [tests]

    BUILDING WITH EXISTING CONFIGURATION:

    VENDOR :   arm-linux
    MODEL  :   demo

    Components: 

    . tests                                             
    . src/services/file_upload                                          
    . src/services/subdev                                          
    . src/services/uOTA                                          
    . src/services/shadow                                          
    . src/services/linkkit/dm                                          
    . src/services/linkkit/cm                                          
    . src/sdk-impl                                          
    . src/protocol/mqtt                                          
    . src/protocol/coap                                          
    . src/protocol/http2                                          
    . src/protocol/alcs                                          
    . src/protocol/http                                          
    . src/tools/linkkit_tsl_convert                                          
    . src/ref-impl/tls                                          
    . src/ref-impl/hal                                          
    . src/infra/log                                          
    . src/infra/system                                          
    . src/infra/utils                                          
    . examples     

*[回到目录](#目录)*
### <a name="交叉编译产生库文件`libiot_sdk.a`">交叉编译产生库文件`libiot_sdk.a`</a>

    $ make
    BUILDING WITH EXISTING CONFIGURATION:

    VENDOR :   arm-linux
    MODEL  :   demo

    [CC] guider.o                           <=  ...
    [CC] utils_epoch_time.o                 <=  ...
    [CC] iotx_log.o                         <=  ...
    [CC] lite_queue.o                       <=  ...
    ...
    ...
    [AR] libiot_sdk.a                       <=  ...
    [AR] libiot_tls.a                       <=  ...

*[回到目录](#目录)*
### <a name="获取交叉编译的产物, 包括静态库和头文件">获取交叉编译的产物, 包括静态库和头文件</a>

    $ ls -1 output/release/lib/
    libiot_sdk.a
    libiot_tls.a

这里, `libiot_sdk.a`文件就是编译好的物联网套件SDK, 已经是`ELF 32-bit LSB relocatable, ARM, EABI5 version 1 (SYSV)`格式, 也就是`arm-linux`格式的交叉编译格式了

另外, `libiot_tls.a`是一个裁剪过的加解密库, 您可以选择使用它, 也可以选择使用平台自带的加解密库, 以减小最终固件的尺寸

    $ ls -1 output/release/include/
    exports
    imports
    iot_export.h
    iot_import.h

这里, `iot_import.h`和`iot_export.h`就是使用SDK需要包含的头文件, 它们按功能点又包含不同的子文件, 分别列在`imports/`目录下和`exports/`目录下

*[回到目录](#目录)*
## <a name="3.3 开发未适配平台的HAL层">3.3 开发未适配平台的HAL层</a>

对于实现平台抽象层接口 `HAL_XXX_YYY()` 的库 `libiot_hal.a`, 不限制其编译和产生的方式

但是如果你愿意的话, 当然仍然可以借助物联网套件设备端C-SDK的编译系统来开发和产生它

---
仍然以上一节中, 某款目前未适配的`arm-linux`目标平台为例, 假设这款平台和`Ubuntu`差别很小, 完全可以用`Ubuntu`上开发测试的HAL层代码作为开发的基础, 则可以这样做:

*[回到目录](#目录)*
### <a name="复制一份HAL层实现代码">复制一份HAL层实现代码</a>

    $ cd src/ref-impl/hal/os/
    $ ls
    macos  ubuntu  win7
    src/ref-impl/hal/os$ cp -rf ubuntu arm-linux
    src/ref-impl/hal/os$ ls
    arm-linux  macos  ubuntu  win7

    src/ref-impl/hal/os$ tree -A arm-linux/
    arm-linux/
    +-- base64.c
    +-- base64.h
    +-- cJSON.c
    +-- cJSON.h
    +-- HAL_Crypt_Linux.c
    +-- HAL_OS_linux.c
    +-- HAL_TCP_linux.c
    +-- HAL_UDP_linux.c
    +-- kv.c
    +-- kv.h

*[回到目录](#目录)*
### <a name="打开之前被关闭的编译开关">打开之前被关闭的编译开关</a>

    $ vim src/board/config.arm-linux.demo

    CONFIG_ENV_CFLAGS = \
        -D_PLATFORM_IS_LINUX_ \
        -Wall

    OVERRIDE_CC = arm-linux-gnueabihf-gcc
    OVERRIDE_AR = arm-linux-gnueabihf-ar
    OVERRIDE_LD = arm-linux-gnueabihf-ld

    # CONFIG_src/ref-impl/hal         :=
    CONFIG_examples                 :=
    CONFIG_tests                    :=
    CONFIG_src/tools/linkkit_tsl_convert :=

可以看到在`CONFIG_src/ref-impl/hal :=`这一行前添加了一个`#`符号, 代表这一行被注释掉了, 效果等同于被删掉, `src/ref-impl/hal`将会进入编译过程

*[回到目录](#目录)*
### <a name="尝试交叉编译被复制的HAL层代码">尝试交叉编译被复制的HAL层代码</a>

    $ make reconfig
    SELECT A CONFIGURATION:

    1) config.arm-linux.demo  3) config.rhino.make      5) config.win7.mingw32
    2) config.macos.make      4) config.ubuntu.x86
    #? 1

    SELECTED CONFIGURATION:

    VENDOR :   arm-linux
    MODEL  :   demo
    ...
    ...

    $ make
    [CC] utils_md5.o                        <=  ...
    [CC] utils_event.o                      <=  ...
    [CC] string_utils.o                     <=  ...
    ...
    ...
    [AR] libiot_sdk.a                       <=  ...
    [AR] libiot_hal.a                       <=  ...
    [AR] libiot_tls.a                       <=  ...

可以看到我们进展的十分顺利, 被复制的代码 `src/ref-impl/hal/os/arm-linux/*.c` 确实直接编译成功了, 产生了 `arm-linux` 格式的 `libiot_hal.a`

*[回到目录](#目录)*
### <a name="允许交叉编译样例程序">允许交叉编译样例程序</a>

这样有了`libiot_hal.a`, `libiot_tls.a`, 以及`libiot_sdk.a`, 其实已经可以尝试交叉编译样例的可执行程序, 并在目标嵌入式硬件开发板上运行一下试试了

方法和上一步一样, 打开`config.arm-linux.demo`里面的`CONFIG_example`开关, 使得`examples/`目录下的样例源码被编译出来

    $ vi src/board/config.arm-linux.demo

    CONFIG_ENV_CFLAGS = \
        -D_PLATFORM_IS_LINUX_ \
        -Wall

    CONFIG_ENV_LDFLAGS = \
        -lpthread -lrt

    OVERRIDE_CC = arm-linux-gnueabihf-gcc
    OVERRIDE_AR = arm-linux-gnueabihf-ar
    OVERRIDE_LD = arm-linux-gnueabihf-ld

    # CONFIG_src/ref-impl/hal         :=
    # CONFIG_examples                 :=
    CONFIG_tests                    :=
    CONFIG_src/tools/linkkit_tsl_convert :=

可以看到在`CONFIG_examples =`这一行前添加了一个`#`符号, 代表这一行被注释掉了, 效果等同于被删掉, `examples/` 目录也就是例子可执行程序进入了编译范围

另外一点需要注意的改动是增加了:

    CONFIG_ENV_LDFLAGS = \
        -lpthread -lrt

部分, 这是因为产生这些样例程序除了链接`libiot_hal.a`和`libiot_hal.a`之外, 还需要连接 `libpthread` 库和 `librt` 库

*[回到目录](#目录)*
### <a name="重新载入配置文件, 交叉编译可执行程序">重新载入配置文件, 交叉编译可执行程序</a>

    $ make reconfig
    SELECT A CONFIGURATION:

    1) config.arm-linux.demo  3) config.rhino.make      5) config.win7.mingw32
    2) config.macos.make      4) config.ubuntu.x86
    #? 1

    SELECTED CONFIGURATION:

    VENDOR :   arm-linux
    MODEL  :   demo
    ...

需要注意, 编译样例程序, 需要用:

    make all

命令, 而不再是上面的

    make

命令来产生编译产物, 比如:

    $ make all
    BUILDING WITH EXISTING CONFIGURATION:

    VENDOR :   arm-linux
    MODEL  :   demo

    [CC] base64.o                           <= base64.c                                    
    [CC] cJSON.o                            <= cJSON.c                                    
    [CC] HAL_UDP_linux.o                    <= HAL_UDP_linux.c          
    ...
    ...

如果有如下的编译输出, 则代表 `mqtt-example` 等一系列样例程序已经被成功的编译出来, 它们存放在 `output/release/bin` 目录下

    [LD] mqtt_rrpc-example                  <= mqtt_rrpc-example.o                                    
    [LD] uota_app-example                   <= uota_app-example.o                                    
    [LD] http-example                       <= http-example.o                                    
    [LD] mqtt-example                       <= mqtt-example.o                                    
    [LD] mqtt_multi_thread-example          <= mqtt_multi_thread-example.o                                    
    [LD] ota_mqtt-example                   <= ota_mqtt-example.o                                    
    [LD] linkkit-example-sched              <= linkkit_example_sched.o                                    
    [LD] linkkit-example-solo               <= linkkit_example_solo.o     

    $ cd output/release/bin/
    $ ls
    http-example  linkkit-example-sched  linkkit-example-solo  mqtt-example  mqtt_multi_thread-example  mqtt_rrpc-example  ota_mqtt-example  uota_app-example

    $ file *
    http-example:              ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-armhf.so.3, for GNU/Linux 3.2.0, ...
    linkkit-example-sched:     ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-armhf.so.3, for GNU/Linux 3.2.0, ...
    linkkit-example-solo:      ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-armhf.so.3, for GNU/Linux 3.2.0, ...
    mqtt-example:              ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-armhf.so.3, for GNU/Linux 3.2.0, ...
    mqtt_multi_thread-example: ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-armhf.so.3, for GNU/Linux 3.2.0, ...
    mqtt_rrpc-example:         ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-armhf.so.3, for GNU/Linux 3.2.0, ...
    ota_mqtt-example:          ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-armhf.so.3, for GNU/Linux 3.2.0, ...
    uota_app-example:          ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-armhf.so.3, for GNU/Linux 3.2.0, ...

可以用`file`命令验证, 这些可执行程序确实是交叉编译到 `arm-linux` 架构上的

*[回到目录](#目录)*
### <a name="尝试运行样例程序">尝试运行样例程序</a>

接下来, 您就可以把样例程序例如`mqtt-example`, 用`SCP`, `TFTP`或者其它方式, 拷贝下载到您的目标开发板上运行调试了

- 如果一切顺利, 样例程序和同样例程在 `Ubuntu` 上运行效果相同, 则证明 `src/ref-impl/hal/os/arm-linux` 部分的HAL层代码工作正常
- 如果样例程序运行起来, 和同样例程在 `Ubuntu` 上运行效果不同, 则需要再重点修改调试HAL实现
- 也就是指 `src/ref-impl/hal/os/arm-linux` 目录的HAL层代码, 因为这些代码是我们从 `Ubuntu` 主机部分复制的, 完全可能并不适合 `arm-linux`

如此反复直到确保 `libiot_hal.a` 的开发没问题为止



第四章目录
---
+ [第四章 构建配置系统](#第四章 构建配置系统)
    * [4.1 基于make的编译系统详解](#4.1 基于make的编译系统详解)
        - [常用命令](#常用命令)
        - [组成部分](#组成部分)
            + [make.settings](#make.settings)
            + [makefile](#makefile)
            + [build-rules](#build-rules)
            + [project.mk](#project.mk)
            + [src/board/config.xxx.yyy](#src/board/config.xxx.yyy)
            + [src/xxx/yyy/iot.mk](#src/xxx/yyy/iot.mk)
    * [4.2 config.xxx.yyy文件语法详解](#4.2 config.xxx.yyy文件语法详解)
        - [交叉编译相关](#交叉编译相关)
        - [目录文件相关](#目录文件相关)
    * [4.3 iot.mk文件语法详解](#4.3 iot.mk文件语法详解)
        - [输出相关](#输出相关)
        - [输入相关](#输入相关)
        - [模块组合相关](#模块组合相关)
        - [其它](#其它)
    * [4.4 基于make交叉编译到嵌入式平台](#4.4 基于make交叉编译到嵌入式平台)
        - [用make为arm-linux编译](#用make为arm-linux编译)
    * [4.5 基于make编译到主机环境](#4.5 基于make编译到主机环境)
        - [用make为64位Linux/OSX编译](#用make为64位Linux/OSX编译)
        - [用make为32位Linux/OSX编译](#用make为32位Linux/OSX编译)
        - [用make为Windows编译](#用make为Windows编译)
    * [4.6 构建配置输入文件](#4.6 构建配置输入文件)
        - [make.settings文件详解](#make.settings文件详解)
            + [FEATURE_ALCS_ENABLED](#FEATURE_ALCS_ENABLED)
            + [FEATURE_COAP_COMM_ENABLED](#FEATURE_COAP_COMM_ENABLED)
            + [FEATURE_COAP_DTLS_SUPPORT](#FEATURE_COAP_DTLS_SUPPORT)
            + [FEATURE_DEPRECATED_LINKKIT](#FEATURE_DEPRECATED_LINKKIT)
            + [FEATURE_ENHANCED_GATEWAY](#FEATURE_ENHANCED_GATEWAY)
            + [FEATURE_HTTP2_COMM_ENABLED](#FEATURE_HTTP2_COMM_ENABLED)
            + [FEATURE_HTTP_COMM_ENABLED](#FEATURE_HTTP_COMM_ENABLED)
            + [FEATURE_MQTT_COMM_ENABLED](#FEATURE_MQTT_COMM_ENABLED)
            + [FEATURE_MQTT_DIRECT](#FEATURE_MQTT_DIRECT)
            + [FEATURE_MQTT_SHADOW](#FEATURE_MQTT_SHADOW)
            + [FEATURE_OTA_ENABLED](#FEATURE_OTA_ENABLED)
            + [FEATURE_OTA_FETCH_CHANNEL](#FEATURE_OTA_FETCH_CHANNEL)
            + [FEATURE_OTA_SIGNAL_CHANNEL](#FEATURE_OTA_SIGNAL_CHANNEL)
            + [FEATURE_SDK_ENHANCE](#FEATURE_SDK_ENHANCE)
            + [FEATURE_SUBDEVICE_ENABLED](#FEATURE_SUBDEVICE_ENABLED)
            + [FEATURE_SUPPORT_ITLS](#FEATURE_SUPPORT_ITLS)
            + [FEATURE_SUPPORT_TLS](#FEATURE_SUPPORT_TLS)
            + [FEATURE_WIFI_AWSS_ENABLED](#FEATURE_WIFI_AWSS_ENABLED)
        - [iot_import_config.h文件详解](#iot_import_config.h文件详解)
            + [CONFIG_MBEDTLS_DEBUG_LEVEL](#CONFIG_MBEDTLS_DEBUG_LEVEL)
            + [CONFIG_MQTT_TX_MAXLEN](#CONFIG_MQTT_TX_MAXLEN)
            + [CONFIG_MQTT_RX_MAXLEN](#CONFIG_MQTT_RX_MAXLEN)
    * [4.7 基于cmake的编译示例](#4.7 基于cmake的编译示例)
        - [用cmake为64位Linux/OSX编译](#用cmake为64位Linux/OSX编译)
        - [用cmake为32位Linux/OSX编译](#用cmake为32位Linux/OSX编译)
        - [用cmake为arm-linux编译](#用cmake为arm-linux编译)
        - [用cmake为Windows编译](#用cmake为Windows编译)



*[回到目录](#目录)*
# <a name="第四章 构建配置系统">第四章 构建配置系统</a>

目前设备端C-SDK的构建配置系统支持以下的编译方式
---
+ 在`Linux`上或者`MacOS`上以`GNU Make` + `各种工具链`编译, 产生`各种目标架构`的SDK, 本节将演示
    * 以`GNU Make` + `gcc`, 产生适用于 `64位Linux/OSX` 的SDK
    * 以`GNU Make` + `gcc`, 产生适用于 `32位Linux/OSX` 的SDK
    * 以`GNU Make` + `arm-linux-gnueabihf-gcc`, 产生适用于 `arm-linux` 嵌入式平台的SDK
    * 以`GNU Make` + `i686-w64-mingw32-gcc`, 产生适用于 `Windows` 平台的SDK
+ 在`Linux`上或者`MacOS`上以`cmake` + `各种工具链`编译, 产生`各种目标架构`的SDK
    * 以`cmake` + `gcc`, 产生适用于 `64位Linux/OSX` 的SDK
    * 以`cmake` + `gcc`, 产生适用于 `32位Linux/OSX` 的SDK
    * 以`cmake` + `arm-linux-gnueabihf-gcc`, 产生适用于 `arm-linux` 嵌入式平台的SDK
    * 以`cmake` + `i686-w64-mingw32-gcc`, 产生适用于 `Windows` 平台的SDK

未来可能支持以下的编译方式
---
+ 在`Windows`上以`cmake` + `mingw32`编译, 产生适用于 `Windows` 平台的SDK
+ 在`Windows`上以`VS Code 2017`编译, 产生适用于 `Windows` 平台的SDK
+ 在`Windows`上以`QT Creator`编译, 产生适用于 `Windows` 平台的SDK
+ 在`Windows`上以`Visual Studio 2017`编译, 产生适用于 `Windows` 平台的SDK

目前设备端C-SDK的构建配置系统的配置/裁剪接口是由以下三者组合提供
---
+ 硬件平台维度: `src/board/config.xxx.yyy`
+ 功能模块维度: `make.settings`
+ 资源伸缩维度: `include/imports/iot_import_config.h`

> 本章先说明编译系统及其config文件的语法, 接着演示如何交叉编译到嵌入式目标平台和不交叉的编译Linux/OSX/Windows主机demo版本
>
> 再说明配置相关的文件及其用法, 介绍在平台选定的情况下再如何裁剪和配置功能
>
> 最后介绍了跨平台的cmake编译系统, 它同样是基于config文件/iot.mk/make.settings等输入的, 但可适用用更多的开发环境

*[回到目录](#目录)*
## <a name="4.1 基于make的编译系统详解">4.1 基于make的编译系统详解</a>

*[回到目录](#目录)*
### <a name="常用命令">常用命令</a>

*[回到目录](#目录)*
### <a name="组成部分">组成部分</a>

设备端C-SDK的构建配置系统, 有以下三个输入文件可接受用户的配置, 您可以通过编辑它们, 将配置输入到构建系统中
+ **功能配置文件:** 即顶层目录的 `make.settings` 文本文件
+ **平台配置文件:** 即目录 `src/board` 下的 `config.xxx.yyy` 系列文件, 也称config文件
+ **伸缩配置文件:** 即 `include/imports/iot_import_config.h` 文件

---
构建系统最终是依据 `config.xxx.yyy` 文件进行编译, 然而由于功能配置/裁剪更为常用, 我们将它额外抽取到了 `make.settings` 中

> config.xxx.yyy 更多关注于目标嵌入式硬件平台的工具链程序和编译/链接选项的指定, 用于跨平台移植
>
> make.settings 则是在已被确定的目标硬件平台上, 专注于C-SDK的功能模块裁剪或者配置, 用于裁剪功能模块
>
> iot_import_config.h 是接着对已被确定的功能, 进行"资源耗费"方面的伸缩性质的配置, 如时间长度/内存大小/重试次数/线程多少/日志简繁等

#### <a name="make.settings">make.settings</a>
#### <a name="makefile">makefile</a>
#### <a name="build-rules">build-rules</a>
#### <a name="project.mk">project.mk</a>
#### <a name="src/board/config.xxx.yyy">src/board/config.xxx.yyy</a>
#### <a name="src/xxx/yyy/iot.mk">src/xxx/yyy/iot.mk</a>

*[回到目录](#目录)*
## <a name="4.2 config.xxx.yyy文件语法详解">4.2 config.xxx.yyy文件语法详解</a>

*[回到目录](#目录)*
### <a name="交叉编译相关">交叉编译相关</a>
*[回到目录](#目录)*
### <a name="目录文件相关">目录文件相关</a>

*[回到目录](#目录)*
## <a name="4.3 iot.mk文件语法详解">4.3 iot.mk文件语法详解</a>

*[回到目录](#目录)*
### <a name="输出相关">输出相关</a>
*[回到目录](#目录)*
### <a name="输入相关">输入相关</a>
*[回到目录](#目录)*
### <a name="模块组合相关">模块组合相关</a>
*[回到目录](#目录)*
### <a name="其它">其它</a>

*[回到目录](#目录)*
## <a name="4.4 基于make交叉编译到嵌入式平台">4.4 基于make交叉编译到嵌入式平台</a>
*[回到目录](#目录)*
### <a name="用make为arm-linux编译">用make为arm-linux编译</a>

*[回到目录](#目录)*
## <a name="4.5 基于make编译到主机环境">4.5 基于make编译到主机环境</a>
> 本节的示例适用于开发者的开发环境是 Ubuntu16.04 的Linux主机, 或者是Apple公司的 OSX 的MacOS的情况

**这些例子都是在64位主机上的执行情况, 推荐您和阿里开发者一样, 安装64位的操作系统**

*[回到目录](#目录)*
### <a name="用make为64位Linux/OSX编译">用make为64位Linux/OSX编译</a>
> 希望编译产物适用于64位的Ubuntu或OSX的目标平台时
>
> C-SDK对其HAL和TLS都已有官方提供的参考实现, 因此可以完整编译出所有的库和例子程序

选择平台配置
---

    make reconfig
    SELECT A CONFIGURATION:

    1) config.macos.make    3) config.ubuntu.x86
    2) config.rhino.make    4) config.win7.mingw32
    #? 3

    SELECTED CONFIGURATION:

    VENDOR :   ubuntu
    MODEL  :   x86
    ...
    ...

*如果是为64位的OSX目标平台编译, 比如您希望例程直接在苹果电脑上执行, 这一步需选择 `config.macos.make`*

编译
---

    make

获取二进制库
---

    cd output/release/lib
    ls

其中有三个主要产物, **它们都是64位架构的**:

| 产物文件名 | 说明
|------------|------
| `libiot_hal.a`  | HAL接口层的参考实现, 提供了 `HAL_XXX()` 接口
| `libiot_sdk.a`  | SDK的主库, 提供了 `IOT_XXX` 接口和 `linkkit_xxx()` 接口
| `libiot_tls.a`  | 裁剪过的 `mbedtls`, 提供了 `mbedtls_xxx()` 接口, 支撑 `libiot_hal.a`

获取可执行程序
---

    cd output/release/lib
    ls

其中有两个主要产物, **它们都是64位架构的**:
    
| 产物文件名 | 说明
|------------|------
| `linkkit-example-solo`  | 高级版(旧版API)的例程, 可演示 `linkkit_xxx()` 接口的使用
| `mqtt-example`          | 基础版的例程, 可演示 `IOT_XXX()` 接口的使用

*[回到目录](#目录)*
### <a name="用make为32位Linux/OSX编译">用make为32位Linux/OSX编译</a>
> 如果您编译SDK是在一台安装了32位 Linux/OSX 的机器上, 那么直接重复上面 [用make为64位Linux/OSX编译](#用make为64位Linux/OSX编译) 的步骤, 即可得到32位的库和例程
>
> 如果您是在安装了64位 `Ubuntu16.04` 的机器上, 需要编译出32位的库, 请按照下文操作

安装32位工具链
---

    sudo apt-get install -y libc6:i386 libstdc++6:i386 gcc:i386

修改平台配置文件
---

    vim src/board/config.ubuntu.x86

增加如下一行

    OVERRIDE_CC         := gcc -m32

比如:

    cat src/board/config.ubuntu.x86

    CONFIG_ENV_CFLAGS   += \
        -Os -Wall \
        -g3 --coverage \
        -D_PLATFORM_IS_LINUX_ \
        -D__UBUNTU_SDK_DEMO__ \

    ...
    ...
    CONFIG_ENV_LDFLAGS  += -lpthread -lrt

    OVERRIDE_CC         := gcc -m32
    OVERRIDE_STRIP      := strip

选择平台配置
---

    make reconfig
    SELECT A CONFIGURATION:

    1) config.macos.make    3) config.ubuntu.x86
    2) config.rhino.make    4) config.win7.mingw32
    #? 3

编译
---

    make

获取二进制库
---

    cd output/release/lib
    ls

其中有三个主要产物, **它们都是32位架构的**:

| 产物文件名 | 说明
|------------|------
| `libiot_hal.a`  | HAL接口层的参考实现, 提供了 `HAL_XXX()` 接口
| `libiot_sdk.a`  | SDK的主库, 提供了 `IOT_XXX` 接口和 `linkkit_xxx()` 接口
| `libiot_tls.a`  | 裁剪过的 `mbedtls`, 提供了 `mbedtls_xxx()` 接口, 支撑 `libiot_hal.a`

*[回到目录](#目录)*
### <a name="用make为Windows编译">用make为Windows编译</a>

*[回到目录](#目录)*
## <a name="4.6 构建配置输入文件">4.6 构建配置输入文件</a>

*[回到目录](#目录)*
### <a name="make.settings文件详解">make.settings文件详解</a>

解压之后, 打开功能配置文件 `make.settings`, 根据需要编辑配置项，使用不同的编译配置，编译输出的SDK内容以及examples都有所不同

默认的配置状态为
---
    FEATURE_MQTT_COMM_ENABLED    = y          # 是否打开MQTT通道的总开关
    FEATURE_MQTT_DIRECT          = y          # 是否打开MQTT直连的分开关
    FEATURE_OTA_ENABLED          = y          # 是否打开固件升级功能的分开关
    FEATURE_SDK_ENHANCE          = y          # 是否打开高级版功能的总开关
    FEATURE_ENHANCED_GATEWAY     = n          # 是否产生高级版网关SDK的分开关
    FEATURE_WIFI_AWSS_ENABLED    = y          # 是否打开WiFi配网功能的开关
    FEATURE_SUPPORT_TLS          = y          # 选择TLS安全连接的开关, 此开关与iTLS开关互斥
    FEATURE_SUPPORT_ITLS         = n          # 选择iTLS安全连接的开关, 此开关与TLS开关互斥, 使能ID2时需打开此开关

除此以外, 即使并未出现在默认 `make.settings` 文件中的选项, 只要在以下的列表中, 也仍然可以被添加到 `make.settings` 文件

#### <a name="FEATURE_ALCS_ENABLED">FEATURE_ALCS_ENABLED</a>
+ 本地通信功能开关, 所谓本地通信是指搭载了C-SDK的嵌入式设备和手机app之间的局域网直接通信, 而不经过因特网和阿里云服务器中转
+ 可取的值是 `y` 或者 `n`
+ 它不依赖其它的 `FEATURE_XXX`
+ 它不是其它 `FEATURE_XXX` 的依赖之一
+ 对应的源码目录是 `src/protocol/alcs`

#### <a name="FEATURE_COAP_COMM_ENABLED">FEATURE_COAP_COMM_ENABLED</a>
+ CoAP上云功能开关, 所谓CoAP上云是指搭载了C-SDK的嵌入式设备和阿里云服务器之间使用 `CoAP` 协议进行连接和交互
+ 可取的值是 `y` 或者 `n`
+ 它不依赖其它的 `FEATURE_XXX`
+ 它不是其它 `FEATURE_XXX` 的依赖之一
+ 对应的源码目录是 `src/protocol/coap`

#### <a name="FEATURE_COAP_DTLS_SUPPORT">FEATURE_COAP_DTLS_SUPPORT</a>
+ CoAP上云DTLS开关, 配置设备和阿里云服务器之间使用 `CoAP` 协议进行连接和交互时, 是否要以 `DTLS` 方式进行身份认证和报文加解密
+ 可取的值是 `y` 或者 `n`
+ 它依赖于 `FEATURE_COAP_COMM_ENABLED` 的值是 `y`
+ 它不是其它 `FEATURE_XXX` 的依赖之一
+ 对应的源码目录是 `src/protocol/coap`

#### <a name="FEATURE_DEPRECATED_LINKKIT">FEATURE_DEPRECATED_LINKKIT</a>
+ 高级版接口风格的开关, 配置进行高级版物模型相关的编程时, C-SDK是提供 `linkkit_xxx_yyy()` 风格的旧版接口, 还是提供 `IOT_Linkkit_XXX()` 风格的新版接口
+ 可取的值是 `y` 或者 `n`
+ 它依赖于 `FEATURE_SDK_ENHANCE` 的值是 `y`
+ 它不是其它 `FEATURE_XXX` 的依赖之一
+ 对应的源码目录是 `src/sdk-impl`

#### <a name="FEATURE_ENHANCED_GATEWAY">FEATURE_ENHANCED_GATEWAY</a>
+ 高级版网关能力的开关, 配置进行高级版物模型相关的编程时, C-SDK是提供 `linkkit_xxx_yyy()` 风格的单品接口, 还是提供 `linkkit_gateway_xxx_yyy()` 风格的网关接口
+ 可取的值是 `y` 或者 `n`
+ 它依赖于 `FEATURE_SDK_ENHANCE` 的值是 `y`
+ 它不是其它 `FEATURE_XXX` 的依赖之一
+ 对应的源码目录是 `src/services/linkkit/dm`

#### <a name="FEATURE_HTTP2_COMM_ENABLED">FEATURE_HTTP2_COMM_ENABLED</a>
+ HTTP2上云功能开关, 所谓HTTP2上云是指搭载了C-SDK的嵌入式设备和阿里云服务器之间使用 `HTTP2` 协议进行连接和交互
+ 可取的值是 `y` 或者 `n`
+ 它不依赖其它的 `FEATURE_XXX`
+ 它不是其它 `FEATURE_XXX` 的依赖之一
+ 对应的源码目录是 `src/protocol/http2`

#### <a name="FEATURE_HTTP_COMM_ENABLED">FEATURE_HTTP_COMM_ENABLED</a>
+ HTTP/S上云功能开关, 所谓HTTP/S上云是指搭载了C-SDK的嵌入式设备和阿里云服务器之间使用 `HTTP` 协议或 `HTTPS` 协议进行连接和交互
+ 可取的值是 `y` 或者 `n`
+ 它不依赖其它的 `FEATURE_XXX`
+ 它不是其它 `FEATURE_XXX` 的依赖之一
+ 对应的源码目录是 `src/protocol/http`

#### <a name="FEATURE_MQTT_COMM_ENABLED">FEATURE_MQTT_COMM_ENABLED</a>
+ MQTT上云功能开关, 所谓MQTT上云是指搭载了C-SDK的嵌入式设备和阿里云服务器之间使用 `MQTT` 协议进行连接和交互
+ 可取的值是 `y` 或者 `n`
+ 它不依赖其它的 `FEATURE_XXX`
+ 它被以下 `FEATURE_XXX` 所依赖, 是它们必需的前提
    * FEATURE_MQTT_DIRECT
    * FEATURE_MQTT_SHADOW
    * FEATURE_SUBDEVICE_ENABLED
    * FEATURE_WIFI_AWSS_ENABLED
    * FEATURE_SDK_ENHANCE
+ 对应的源码目录是 `src/protocol/mqtt`

#### <a name="FEATURE_MQTT_DIRECT">FEATURE_MQTT_DIRECT</a>
+ MQTT直连功能开关, 所谓MQTT直连是指设备和阿里云服务器之间使用 `MQTT` 协议进行连接, 而不会前置基于 `HTTP` 协议认证的交互过程
+ 可取的值是 `y` 或者 `n`
+ 它依赖于 `FEATURE_MQTT_COMM_ENABLED` 的值是 `y`
+ 它不是其它 `FEATURE_XXX` 的依赖之一
+ 对应的源码目录是 `src/infra/system`

#### <a name="FEATURE_MQTT_SHADOW">FEATURE_MQTT_SHADOW</a>
+ MQTT设备影子开关, 所谓MQTT设备影子是指设备在阿里云服务器之间上以JSON文档保留一份设备数据的镜像
+ 可取的值是 `y` 或者 `n`
+ 它依赖于 `FEATURE_MQTT_COMM_ENABLED` 的值是 `y`
+ 它不是其它 `FEATURE_XXX` 的依赖之一
+ 对应的源码目录是 `src/services/shadow`

#### <a name="FEATURE_OTA_ENABLED">FEATURE_OTA_ENABLED</a>
+ 固件升级功能开关, 所谓固件升级是指设备从阿里云服务器上下载用户在IoT控制台中上传的固件文件功能
+ 可取的值是 `y` 或者 `n`
+ 它依赖于 `FEATURE_MQTT_COMM_ENABLED` 或者 `FEATURE_COAP_COMM_ENABLED` 的值是 `y`
+ 它被以下 `FEATURE_XXX` 的依赖
    * FEATURE_OTA_FETCH_CHANNEL
    * FEATURE_OTA_SIGNAL_CHANNEL
+ 对应的源码目录是 `src/services/uOTA`

#### <a name="FEATURE_OTA_FETCH_CHANNEL">FEATURE_OTA_FETCH_CHANNEL</a>
+ 固件升级中文件下载的通道配置, 所谓文件下载通道是指设备从阿里云服务器下载固件文件的数据通道
+ 可取的值是 `HTTP` 或者 `COAP`
+ 它在被配置为 `COAP` 的时候, 依赖于 `FEATURE_COAP_COMM_ENABLED` 的值是 `y`
+ 它不是其它 `FEATURE_XXX` 的依赖之一
+ 对应的源码目录是 `src/services/uOTA`

#### <a name="FEATURE_OTA_SIGNAL_CHANNEL">FEATURE_OTA_SIGNAL_CHANNEL</a>
+ 固件升级中推送消息的通道配置, 所谓推送消息通道是指设备从阿里云服务器接收到有新固件需要下载的控制通道
+ 可取的值是 `MQTT` 或者 `COAP`
+ 它依赖于 `FEATURE_MQTT_COMM_ENABLED` 或者 `FEATURE_COAP_COMM_ENABLED` 的值是 `y`
+ 它不是其它 `FEATURE_XXX` 的依赖之一
+ 对应的源码目录是 `src/services/uOTA`

#### <a name="FEATURE_SDK_ENHANCE">FEATURE_SDK_ENHANCE</a>
+ 高级版物模型能力的功能开关, 所谓高级版物模型能力是指设备可使用基于服务/属性/事件三要素的Alink协议和服务端通信
+ 可取的值是 `y` 或者 `n`
+ 它依赖于 `FEATURE_MQTT_COMM_ENABLED` 的值是 `y`
+ 它被 `FEATURE_ENHANCED_GATEWAY` 所依赖
+ 对应的源码目录是 `src/services/linkkit`

#### <a name="FEATURE_SUBDEVICE_ENABLED">FEATURE_SUBDEVICE_ENABLED</a>
+ 旧版子设备管理能力的功能开关, 所谓子设备管理能力是指设备可代理其它不具有直接连云通道的设备, 上报和接收MQTT消息
+ 可取的值是 `y` 或者 `n`
+ 它依赖于 `FEATURE_MQTT_COMM_ENABLED` 的值是 `y`
+ 它不是其它 `FEATURE_XXX` 的依赖之一
+ 对应的源码目录是 `src/services/subdev`

#### <a name="FEATURE_SUPPORT_ITLS">FEATURE_SUPPORT_ITLS</a>
#### <a name="FEATURE_SUPPORT_TLS">FEATURE_SUPPORT_TLS</a>
+ 在TLS层是否使用iTLS的功能开关, 所谓iTLS是阿里巴巴基于ID2商业产品提供的特有闭源安全连接协议
+ 可取的值是 `y` 或者 `n`, `FEATURE_SUPPORT_ITLS` 和 `FEATURE_SUPPORT_TLS` 的取值**必须不同**
+ 它不依赖其它的 `FEATURE_XXX`
+ 它不是其它 `FEATURE_XXX` 的依赖之一
+ 对应的源码目录是 `src/ref-impl/hal`

#### <a name="FEATURE_WIFI_AWSS_ENABLED">FEATURE_WIFI_AWSS_ENABLED</a>
+ WiFi配网的功能开关, 所谓WiFi配网是阿里巴巴自研的一种从手机app发送WiFi网络的SSID和密码给设备端的通信协议
+ 可取的值是 `y` 或者 `n`
+ 它依赖于 `FEATURE_ALCS_ENABLED` 的值是 `y`
+ 它不是其它 `FEATURE_XXX` 的依赖之一
+ 对应的源码目录是 `src/services/awss`

*[回到目录](#目录)*
### <a name="iot_import_config.h文件详解">iot_import_config.h文件详解</a>

相比 `make.settings` 和 `src/board/config.xxx.yyy` 文件, 摆置在 `include/imports/iot_import_config.h` 的这个文件"内部"的多, 也"细节"的多

它的定位是在目标硬件平台确定, 继而C-SDK内容功能确定之后, 进行"资源耗费"方面的伸缩性质的配置, 如时间长度/内存大小/重试次数/线程多少/日志简繁等

**需要注意的是: 我们并不建议用户直接修改这个文件!**

---
之所以会对该文件进行说明, 是因为可用的配置项都在这个文件中出现, 对这些配置项正确的调整方式是写在 `config` 文件中, 例如

`src/board/config.ubuntu.x86` 文件中的如下段落:

    CONFIG_ENV_CFLAGS   += \
        -DCONFIG_HTTP_AUTH_TIMEOUT=500 \
        -DCONFIG_MID_HTTP_TIMEOUT=500 \
        -DCONFIG_GUIDER_AUTH_TIMEOUT=500 \
        -DCONFIG_MQTT_RX_MAXLEN=5000 \
        -DCONFIG_MQTT_SUBTOPIC_MAXNUM=65535 \
        -DCONFIG_MBEDTLS_DEBUG_LEVEL=0 \

#### <a name="CONFIG_MBEDTLS_DEBUG_LEVEL">CONFIG_MBEDTLS_DEBUG_LEVEL</a>
+ 调整在TLS连接过程中, 调试日志打印的详细程度
+ 可配置的值从 `0` 到 `10`, 数字越大, 打印越详细, 数字为 `0` 表示不打印调试信息
+ 起作用的源码目录在 `src/ref-impl/tls`

#### <a name="CONFIG_MQTT_TX_MAXLEN">CONFIG_MQTT_TX_MAXLEN</a>
+ 调整在高级版中, 通信模块为MQTT上行报文所开辟的常驻内存缓冲区长度
+ 可配置的值为整数, 建议`512`到`2048`, 过小会导致上行报文不够空间组装, 过大会导致设备上RAM资源消耗增大
+ 起作用的源码目录在 `src/services/linkkit`

#### <a name="CONFIG_MQTT_RX_MAXLEN">CONFIG_MQTT_RX_MAXLEN</a>
+ 调整在高级版中, 通信模块为MQTT上行报文所开辟的常驻内存缓冲区长度
+ 可配置的值为整数, 建议`512`到`2048`, 过小会导致上行报文不够空间组装, 过大会导致设备上RAM资源消耗增大
+ 起作用的源码目录在 `src/services/linkkit`

*[回到目录](#目录)*
## <a name="4.7 基于cmake的编译示例">4.7 基于cmake的编译示例</a>
> 本节的示例适用于开发者的开发环境是 Ubuntu16.04 的Linux主机, 或者是Apple公司的 OSX 的MacOS的情况

**这些例子都是在64位主机上的执行情况, 推荐您和阿里开发者一样, 安装64位的操作系统**

*[回到目录](#目录)*
### <a name="用cmake为64位Linux/OSX编译">用cmake为64位Linux/OSX编译</a>

*[回到目录](#目录)*
### <a name="用cmake为32位Linux/OSX编译">用cmake为32位Linux/OSX编译</a>

*[回到目录](#目录)*
### <a name="用cmake为arm-linux编译">用cmake为arm-linux编译</a>

*[回到目录](#目录)*
### <a name="用cmake为Windows编译">用cmake为Windows编译</a>





第五章目录
---
+ [第五章 HAL说明](#第五章 HAL说明)
    * [5.1 设备端C-SDK分层](#5.1 设备端C-SDK分层)
    * [5.2 HAL层接口列表](#5.2 HAL层接口列表)
        - [全部接口列表](#全部接口列表)
        - [基础相关](#基础相关)
        - [多线程相关](#多线程相关)
        - [MQTT和HTTP通道功能相关](#MQTT和HTTP通道功能相关)
        - [CoAP通道功能相关](#CoAP通道功能相关)
        - [本地通信功能相关](#本地通信功能相关)
        - [设备信息设置与获取相关](#设备信息设置与获取相关)
        - [OTA功能相关](#OTA功能相关)
        - [本地定时任务功能相关](#本地定时任务功能相关)
        - [WIFI配网功能相关](#WIFI配网功能相关)
    * [5.3 HAL层接口详解](#5.3 HAL层接口详解)
        - [HAL_Aes128_Cbc_Decrypt](#HAL_Aes128_Cbc_Decrypt)
        - [HAL_Aes128_Cbc_Encrypt](#HAL_Aes128_Cbc_Encrypt)
        - [HAL_Aes128_Cfb_Decrypt](#HAL_Aes128_Cfb_Decrypt)
        - [HAL_Aes128_Cfb_Encrypt](#HAL_Aes128_Cfb_Encrypt)
        - [HAL_Aes128_Destroy](#HAL_Aes128_Destroy)
        - [HAL_Aes128_Init](#HAL_Aes128_Init)
        - [HAL_Awss_Close_Monitor](#HAL_Awss_Close_Monitor)
        - [HAL_Awss_Connect_Ap](#HAL_Awss_Connect_Ap)
        - [HAL_Awss_Get_Channelscan_Interval_Ms](#HAL_Awss_Get_Channelscan_Interval_Ms)
        - [HAL_Awss_Get_Connect_Default_Ssid_Timeout_Interval_Ms](#HAL_Awss_Get_Connect_Default_Ssid_Timeout_Interval_Ms)
        - [HAL_Awss_Get_Conn_Encrypt_Type](#HAL_Awss_Get_Conn_Encrypt_Type)
        - [HAL_Awss_Get_Encrypt_Type](#HAL_Awss_Get_Encrypt_Type)
        - [HAL_Awss_Get_Timeout_Interval_Ms](#HAL_Awss_Get_Timeout_Interval_Ms)
        - [HAL_Awss_Open_Monitor](#HAL_Awss_Open_Monitor)
        - [HAL_Awss_Switch_Channel](#HAL_Awss_Switch_Channel)
        - [HAL_Config_Read](#HAL_Config_Read)
        - [HAL_Config_Write](#HAL_Config_Write)
        - [HAL_DTLSSession_create](#HAL_DTLSSession_create)
        - [HAL_DTLSSession_free](#HAL_DTLSSession_free)
        - [HAL_DTLSSession_read](#HAL_DTLSSession_read)
        - [HAL_DTLSSession_write](#HAL_DTLSSession_write)
        - [HAL_Firmware_Persistence_Start](#HAL_Firmware_Persistence_Start)
        - [HAL_Firmware_Persistence_Stop](#HAL_Firmware_Persistence_Stop)
        - [HAL_Firmware_Persistence_Write](#HAL_Firmware_Persistence_Write)
        - [HAL_Free](#HAL_Free)
        - [HAL_GetChipID](#HAL_GetChipID)
        - [HAL_GetDeviceID](#HAL_GetDeviceID)
        - [HAL_GetDeviceName](#HAL_GetDeviceName)
        - [HAL_GetDeviceSecret](#HAL_GetDeviceSecret)
        - [HAL_GetFirmwareVesion](#HAL_GetFirmwareVesion)
        - [HAL_GetModuleID](#HAL_GetModuleID)
        - [HAL_GetPartnerID](#HAL_GetPartnerID)
        - [HAL_GetProductKey](#HAL_GetProductKey)
        - [HAL_GetProductSecret](#HAL_GetProductSecret)
        - [HAL_GetTimeStr](#HAL_GetTimeStr)
        - [HAL_Kv_Del](#HAL_Kv_Del)
        - [HAL_Kv_Erase_All](#HAL_Kv_Erase_All)
        - [HAL_Kv_Get](#HAL_Kv_Get)
        - [HAL_Kv_Set](#HAL_Kv_Set)
        - [HAL_Malloc](#HAL_Malloc)
        - [HAL_MutexCreate](#HAL_MutexCreate)
        - [HAL_MutexDestroy](#HAL_MutexDestroy)
        - [HAL_MutexLock](#HAL_MutexLock)
        - [HAL_MutexUnlock](#HAL_MutexUnlock)
        - [HAL_Printf](#HAL_Printf)
        - [HAL_Random](#HAL_Random)
        - [HAL_Reboot](#HAL_Reboot)
        - [HAL_RF433_Get_Rssi_Dbm](#HAL_RF433_Get_Rssi_Dbm)
        - [HAL_SemaphoreCreate](#HAL_SemaphoreCreate)
        - [HAL_SemaphoreDestroy](#HAL_SemaphoreDestroy)
        - [HAL_SemaphorePost](#HAL_SemaphorePost)
        - [HAL_SemaphoreWait](#HAL_SemaphoreWait)
        - [HAL_SetDeviceName](#HAL_SetDeviceName)
        - [HAL_SetDeviceSecret](#HAL_SetDeviceSecret)
        - [HAL_SetProductKey](#HAL_SetProductKey)
        - [HAL_SetProductSecret](#HAL_SetProductSecret)
        - [HAL_SleepMs](#HAL_SleepMs)
        - [HAL_Snprintf](#HAL_Snprintf)
        - [HAL_Srandom](#HAL_Srandom)
        - [HAL_SSL_Destroy](#HAL_SSL_Destroy)
        - [HAL_SSL_Establish](#HAL_SSL_Establish)
        - [HAL_SSL_Read](#HAL_SSL_Read)
        - [HAL_SSL_Write](#HAL_SSL_Write)
        - [HAL_Sys_Net_Is_Ready](#HAL_Sys_Net_Is_Ready)
        - [HAL_Sys_reboot](#HAL_Sys_reboot)
        - [HAL_TCP_Destroy](#HAL_TCP_Destroy)
        - [HAL_TCP_Establish](#HAL_TCP_Establish)
        - [HAL_TCP_Read](#HAL_TCP_Read)
        - [HAL_TCP_Write](#HAL_TCP_Write)
        - [HAL_ThreadCreate](#HAL_ThreadCreate)
        - [HAL_ThreadDelete](#HAL_ThreadDelete)
        - [HAL_ThreadDetach](#HAL_ThreadDetach)
        - [HAL_Timer_Create](#HAL_Timer_Create)
        - [HAL_Timer_Delete](#HAL_Timer_Delete)
        - [HAL_Timer_Start](#HAL_Timer_Start)
        - [HAL_Timer_Stop](#HAL_Timer_Stop)
        - [HAL_UDP_bindtodevice](#HAL_UDP_bindtodevice)
        - [HAL_UDP_close](#HAL_UDP_close)
        - [HAL_UDP_close_without_connect](#HAL_UDP_close_without_connect)
        - [HAL_UDP_connect](#HAL_UDP_connect)
        - [HAL_UDP_create](#HAL_UDP_create)
        - [HAL_UDP_create_without_connect](#HAL_UDP_create_without_connect)
        - [HAL_UDP_joinmulticast](#HAL_UDP_joinmulticast)
        - [HAL_UDP_read](#HAL_UDP_read)
        - [HAL_UDP_readTimeout](#HAL_UDP_readTimeout)
        - [HAL_UDP_recv](#HAL_UDP_recv)
        - [HAL_UDP_recvfrom](#HAL_UDP_recvfrom)
        - [HAL_UDP_send](#HAL_UDP_send)
        - [HAL_UDP_sendto](#HAL_UDP_sendto)
        - [HAL_UDP_write](#HAL_UDP_write)
        - [HAL_UptimeMs](#HAL_UptimeMs)
        - [HAL_UTC_Get](#HAL_UTC_Get)
        - [HAL_UTC_Set](#HAL_UTC_Set)
        - [HAL_Vsnprintf](#HAL_Vsnprintf)
        - [HAL_Wifi_Enable_Mgmt_Frame_Filter](#HAL_Wifi_Enable_Mgmt_Frame_Filter)
        - [HAL_Wifi_Get_Ap_Info](#HAL_Wifi_Get_Ap_Info)
        - [HAL_Wifi_Get_IP](#HAL_Wifi_Get_IP)
        - [HAL_Wifi_Get_Mac](#HAL_Wifi_Get_Mac)
        - [HAL_Wifi_Get_Os_Version](#HAL_Wifi_Get_Os_Version)
        - [HAL_Wifi_Get_Rssi_Dbm](#HAL_Wifi_Get_Rssi_Dbm)
        - [HAL_Wifi_Low_Power](#HAL_Wifi_Low_Power)
        - [HAL_Wifi_Scan](#HAL_Wifi_Scan)
        - [HAL_Wifi_Send_80211_Raw_Frame](#HAL_Wifi_Send_80211_Raw_Frame)



*[回到目录](#目录)*
# <a name="第五章 HAL说明">第五章 HAL说明</a>

*[回到目录](#目录)*
## <a name="5.1 设备端C-SDK分层">5.1 设备端C-SDK分层</a>

        +---------------------------+
        |                           |
        |   C-SDK Example Program   |
        |                           |
        +---------------------------+
        |                           |
        |   C-SDK Interface Layer   |
        |                           |
        |       IOT_XXX_YYY() APIs  |
        |   linkkit_mmm_nnn() APIs  |
        |                           |
        +---------------------------+
        |                           |
        |   C-SDK Core Implements   |
        |   : =>                    |
        |   : You SHOULD NOT Focus  |
        |   : on this unless        |
        |   : you're debugging SDK  |
        |                           |
        +---------------------------+
        |                           |
        |  Hardware Abstract Layer  |
        |                           |
        |     HAL_XXX_YYY() APIs    |
        |                           |
        |  : You MUST Implement     |
        |  : this part for your     |
        |  : target device first    |
        +---------------------------+

设备端C-SDK是分为三层的, 如上图

+ 最底层称为"硬件平台抽象层", 也简称`HAL层`, 对应上图的 `Hardware Abstract Layer`
    - 这里是抽象不同的嵌入式目标板上操作系统对我们SDK的支撑函数
    - 举例来说, 包括网络收发, TLS/DTLS通道建立和读写, 内存申请释放, 互斥量加锁解锁等
    - **注意: 在任何跨平台移植时, 实现这部分是需要完成的第一步工作**
    - **注意: 阿里的SDK里, 并不含有多平台的HAL层实现, 但我们提供了Linux桌面OS(Ubuntu16.04)上的参考实现, 移植时可以作为参考**

+ 中间层称为"SDK内核实现层", 对应上图的 `Link Kit Core Implements`
    - 这里是C-SDK的核心实现部分, 它基于HAL层接口完成了MQTT/CoAP通道等的功能封装
    - 举例来说, 包括MQTT的连接建立, 报文收发, CoAP的连接建立, 报文收发, OTA的固件状态查询, OTA的固件下载等
    - **注意: 如果HAL层实现的好, 这一层在跨平台移植时, 理想情况下不需要做任何修改**

+ 最上层称为"SDK接口声明层", 对应上图的 `Link Kit Interface Layer`
    - 这里没有实现, 只有一系列C函数的原型声明, 也就是SDK跨平台移植完成之后, 可以用于编写业务逻辑, 和阿里云服务器通信的API
    - 举例来说, 怎么去使用这些API做业务逻辑, 我们在`examples`目录提供了丰富的示例程序, 并且只要填入了设备信息, 就可以在Linux主机上运行体验

以下按照从下到上的顺序, 逐个对每个层次做更加详细的说明

+ 所有HAL层函数的声明都在 `include/iot_import.h` 这个头文件中列出
+ 各功能点引入的HAL层接口依赖在`include/imports/iot_import_*.h`中列出
+ 这些`include/imports`目录下的子文件, 都被`include/iot_import.h`包含
+ 这个部分的函数声明, 对应在`src/ref-impl/hal/os/ubuntu/`有提供一份`Ubuntu`桌面系统上的参考实现, 会被编译成`output/release/lib/libiot_hal.a`

*[回到目录](#目录)*
## <a name="5.2 HAL层接口列表">5.2 HAL层接口列表</a>
*[回到目录](#目录)*
### <a name="全部接口列表">全部接口列表</a>
以下命令可以列出所有跨平台移植时需要实现的HAL层接口

    include$ grep -ro "HAL_[_A-Za-z0-9]*(" *|awk -F':' '{ print $NF }'|awk '{ print $NF }'|sed 's!(!!g'|sort -u|awk '{ printf("%03d  %s\n", NR, $0); }'

    001  HAL_Aes128_Cbc_Decrypt
    002  HAL_Aes128_Cbc_Encrypt
    003  HAL_Aes128_Cfb_Decrypt
    004  HAL_Aes128_Cfb_Encrypt
    005  HAL_Aes128_Destroy
    006  HAL_Aes128_Init
    007  HAL_Awss_Close_Monitor
    008  HAL_Awss_Connect_Ap
    009  HAL_Awss_Get_Channelscan_Interval_Ms
    010  HAL_Awss_Get_Connect_Default_Ssid_Timeout_Interval_Ms
    011  HAL_Awss_Get_Conn_Encrypt_Type
    012  HAL_Awss_Get_Encrypt_Type
    013  HAL_Awss_Get_Timeout_Interval_Ms
    014  HAL_Awss_Open_Monitor
    015  HAL_Awss_Switch_Channel
    016  HAL_Config_Read
    017  HAL_Config_Write
    018  HAL_DTLSSession_create
    019  HAL_DTLSSession_free
    020  HAL_DTLSSession_read
    021  HAL_DTLSSession_write
    022  HAL_Firmware_Persistence_Start
    023  HAL_Firmware_Persistence_Stop
    024  HAL_Firmware_Persistence_Write
    025  HAL_Free
    026  HAL_GetChipID
    027  HAL_GetDeviceID
    028  HAL_GetDeviceName
    029  HAL_GetDeviceSecret
    030  HAL_GetFirmwareVesion
    031  HAL_GetModuleID
    032  HAL_GetPartnerID
    033  HAL_GetProductKey
    034  HAL_GetProductSecret
    035  HAL_GetTimeStr
    036  HAL_Kv_Del
    037  HAL_Kv_Erase_All
    038  HAL_Kv_Get
    039  HAL_Kv_Set
    040  HAL_Malloc
    041  HAL_MutexCreate
    042  HAL_MutexDestroy
    043  HAL_MutexLock
    044  HAL_MutexUnlock
    045  HAL_Printf
    046  HAL_Random
    047  HAL_Reboot
    048  HAL_RF433_Get_Rssi_Dbm
    049  HAL_SemaphoreCreate
    050  HAL_SemaphoreDestroy
    051  HAL_SemaphorePost
    052  HAL_SemaphoreWait
    053  HAL_SetDeviceName
    054  HAL_SetDeviceSecret
    055  HAL_SetProductKey
    056  HAL_SetProductSecret
    057  HAL_SleepMs
    058  HAL_Snprintf
    059  HAL_Srandom
    060  HAL_SSL_Destroy
    061  HAL_SSL_Establish
    062  HAL_SSL_Read
    063  HAL_SSL_Write
    064  HAL_Sys_Net_Is_Ready
    065  HAL_Sys_reboot
    066  HAL_TCP_Destroy
    067  HAL_TCP_Establish
    068  HAL_TCP_Read
    069  HAL_TCP_Write
    070  HAL_ThreadCreate
    071  HAL_ThreadDelete
    072  HAL_ThreadDetach
    073  HAL_Timer_Create
    074  HAL_Timer_Delete
    075  HAL_Timer_Start
    076  HAL_Timer_Stop
    077  HAL_UDP_bindtodevice
    078  HAL_UDP_close
    079  HAL_UDP_close_without_connect
    080  HAL_UDP_connect
    081  HAL_UDP_create
    082  HAL_UDP_create_without_connect
    083  HAL_UDP_joinmulticast
    084  HAL_UDP_read
    085  HAL_UDP_readTimeout
    086  HAL_UDP_recv
    087  HAL_UDP_recvfrom
    088  HAL_UDP_send
    089  HAL_UDP_sendto
    090  HAL_UDP_write
    091  HAL_UptimeMs
    092  HAL_UTC_Get
    093  HAL_UTC_Set
    094  HAL_Vsnprintf
    095  HAL_Wifi_Enable_Mgmt_Frame_Filter
    096  HAL_Wifi_Get_Ap_Info
    097  HAL_Wifi_Get_IP
    098  HAL_Wifi_Get_Mac
    099  HAL_Wifi_Get_Os_Version
    100  HAL_Wifi_Get_Rssi_Dbm
    101  HAL_Wifi_Low_Power
    102  HAL_Wifi_Scan
    103  HAL_Wifi_Send_80211_Raw_Frame

对这些函数做实现的时候, 可以参考`src/ref-impl/hal/os/ubuntu`下和`src/ref-impl/hal/os/win7`下已经写好的示例, 这些示例在`Ubuntu16.04`主机和`Win7`主机上被完善的编写和测试过

    src/ref-impl/hal$ tree -A
    .
    +-- CMakeLists.txt
    +-- iot.mk
    +-- iotx_hal_internal.h
    +-- os
    |   +-- macos
    |   |   +-- HAL_Crypt_MacOS.c
    |   |   +-- HAL_OS_MacOS.c
    |   |   +-- HAL_TCP_MacOS.c
    |   |   +-- HAL_UDP_MacOS.c
    |   +-- ubuntu
    |   |   +-- base64.c
    |   |   +-- base64.h
    |   |   +-- cJSON.c
    |   |   +-- cJSON.h
    |   |   +-- HAL_Crypt_Linux.c
    |   |   +-- HAL_OS_linux.c
    |   |   +-- HAL_TCP_linux.c
    |   |   +-- HAL_UDP_linux.c
    |   |   +-- kv.c
    |   |   +-- kv.h
    |   +-- win7
    |       +-- HAL_OS_win7.c
    |       +-- HAL_TCP_win7.c
    +-- ssl
        +-- itls
        |   +-- HAL_TLS_itls.c
        +-- mbedtls
        |   +-- HAL_DTLS_mbedtls.c
        |   +-- HAL_TLS_mbedtls.c
        +-- openssl
            +-- HAL_TLS_openssl.c

以下是这些函数的一些简要说明表格, 若需要获取更多函数的详细信息, 请访问章节[HAL层接口详解](#HAL层接口详解), 或者查阅代码中的注释

*[回到目录](#目录)*
### <a name="基础相关">基础相关</a>

| 函数名                                | 说明
|---------------------------------------|-------------------------------------------------------------------------
| [HAL_Malloc](#HAL_Malloc)           | 申请一片堆上内存
| [HAL_Free](#HAL_Free)               | 释放一片堆上内存
| [HAL_SleepMs](#HAL_SleepMs)         | 睡眠函数, 使当前执行线程睡眠指定的毫秒数
| [HAL_Snprintf](#HAL_Snprintf)       | 打印函数, 向内存缓冲区格式化构建一个字符串, 参考C99标准库函数`snprintf`
| [HAL_Printf](#HAL_Printf)           | 打印函数, 用于向串口或其它标准输出打印日志或调试信息
| [HAL_Vsnprintf](#HAL_Vsnprintf)     | 字符串打印函数, 将`va_list`类型的变量, 打印到指定目标字符串
| [HAL_UptimeMs](#HAL_UptimeMs)       | 时钟函数, 获取本设备从加电以来到目前时间点已经过去的毫秒数

*[回到目录](#目录)*
### <a name="多线程相关">多线程相关</a>

| 函数名                                        | 说明
|-----------------------------------------------|--------------------------------------------------------------------------------
| [HAL_MutexCreate](#HAL_MutexCreate)             | 创建一个互斥量, 用于同步控制, 对于仅支持单线程应用, 可实现为空函数
| [HAL_MutexDestroy](#HAL_MutexDestroy)           | 销毁一个互斥量, 用于同步控制, 对于仅支持单线程应用, 可实现为空函数
| [HAL_MutexLock](#HAL_MutexLock)                 | 加锁一个互斥量, 用于同步控制, 对于仅支持单线程应用, 可实现为空函数
| [HAL_MutexUnlock](#HAL_MutexUnlock)             | 解锁一个互斥量, 用于同步控制, 对于仅支持单线程应用, 可实现为空函数
| [HAL_ThreadCreate](#HAL_ThreadCreate)           | 按照指定入参创建一个线程, 对于仅支持单线程应用, 可实现为空函数
| [HAL_ThreadDelete](#HAL_ThreadDelete)           | 杀死指定的线程, 对于仅支持单线程应用, 可实现为空函数
| [HAL_ThreadDetach](#HAL_ThreadDetach)           | 设置指定的线程为`Detach`状态, 对于仅支持单线程应用, 可实现为空函数
| [HAL_SemaphoreCreate](#HAL_SemaphoreCreate)     | 创建一个计数信号量, 对于仅支持单线程应用, 可实现为空函数
| [HAL_SemaphoreDestroy](#HAL_SemaphoreDestroy)   | 销毁一个计数信号量, 对于仅支持单线程应用, 可实现为空函数
| [HAL_SemaphorePost](#HAL_SemaphorePost)         | 在指定的计数信号量上做自减操作并等待, 对于仅支持单线程应用, 可实现为空函数
| [HAL_SemaphoreWait](#HAL_SemaphoreWait)         | 在指定的计数信号量上做自增操作, 解除其它线程的等待, 对于仅支持单线程应用, 可实现为空函数

*[回到目录](#目录)*
### <a name="MQTT和HTTP通道功能相关">MQTT和HTTP通道功能相关</a>

| 函数名                                    | 说明
|-------------------------------------------|-------------------------------------------------------------------------
| [HAL_SSL_Destroy](#HAL_SSL_Destroy)         | 销毁一个TLS连接, 用于MQTT功能, HTTPS功能
| [HAL_SSL_Establish](#HAL_SSL_Establish)     | 建立一个TLS连接, 用于MQTT功能, HTTPS功能
| [HAL_SSL_Read](#HAL_SSL_Read)               | 从一个TLS连接中读数据, 用于MQTT功能, HTTPS功能
| [HAL_SSL_Write](#HAL_SSL_Write)             | 向一个TLS连接中写数据, 用于MQTT功能, HTTPS功能
| [HAL_TCP_Destroy](#HAL_TCP_Destroy)         | 销毁一个TLS连接, 用于MQTT功能, HTTPS功能
| [HAL_TCP_Establish](#HAL_TCP_Establish)     | 建立一个TCP连接, 包含了域名解析的动作和TCP连接的建立
| [HAL_TCP_Read](#HAL_TCP_Read)               | 在指定时间内, 从TCP连接读取流数据, 并返回读到的字节数
| [HAL_TCP_Write](#HAL_TCP_Write)             | 在指定时间内, 向TCP连接发送流数据, 并返回发送的字节数
| [HAL_Random](#HAL_Random)                   | 随机数函数, 接受一个无符号数作为范围, 返回0到该数值范围内的随机无符号数
| [HAL_Srandom](#HAL_Srandom)                 | 随机数播种函数, 使 [HAL_Random](#HAL_Random) 的返回值每个执行序列各不相同, 类似`srand`

*[回到目录](#目录)*
### <a name="CoAP通道功能相关">CoAP通道功能相关</a>

| 函数名                                                | 说明
|-------------------------------------------------------|-------------------------------------------------------------------------
| [HAL_DTLSSession_create](#HAL_DTLSSession_create)   | 初始化DTLS资源并建立一个DTLS会话, 用于CoAP功能
| [HAL_DTLSSession_free](#HAL_DTLSSession_free)       | 销毁一个DTLS会话并释放DTLS资源, 用于CoAP功能
| [HAL_DTLSSession_read](#HAL_DTLSSession_read)       | 从DTLS连接中读数据, 用于CoAP功能
| [HAL_DTLSSession_write](#HAL_DTLSSession_write)     | 向DTLS连接中写数据, 用于CoAP功能
| [HAL_Aes128_Cbc_Decrypt](#HAL_Aes128_Cbc_Decrypt)   | AES128解密, CBC模式, 用于CoAP报文加解密
| [HAL_Aes128_Cbc_Encrypt](#HAL_Aes128_Cbc_Encrypt)   | AES128加密, CBC模式, 用于CoAP报文加解密
| [HAL_Aes128_Cfb_Decrypt](#HAL_Aes128_Cfb_Decrypt)   | AES128解密, CFB模式, 用于CoAP报文加解密
| [HAL_Aes128_Cfb_Encrypt](#HAL_Aes128_Cfb_Encrypt)   | AES128加密, CFB模式, 用于CoAP报文加解密
| [HAL_Aes128_Destroy](#HAL_Aes128_Destroy)           | AES128反初始化
| [HAL_Aes128_Init](#HAL_Aes128_Init)                 | AES128初始化
| [HAL_UDP_close](#HAL_UDP_close)                     | 关闭一个UDP socket
| [HAL_UDP_create](#HAL_UDP_create)                   | 创建一个UDP socket
| [HAL_UDP_read](#HAL_UDP_read)                       | 阻塞的从一个UDP socket中读取数据包, 并返回读到的字节数
| [HAL_UDP_readTimeout](#HAL_UDP_readTimeout)         | 在指定时间内, 从一个UDP socket中读取数据包, 返回读到的字节数
| [HAL_UDP_write](#HAL_UDP_write)                     | 阻塞的向一个UDP socket中发送数据包, 并返回发送的字节数

*[回到目录](#目录)*
### <a name="本地通信功能相关">本地通信功能相关</a>

| 函数名                                                            | 说明
|-------------------------------------------------------------------|-----------------------------------------------------------------
| [HAL_UDP_create_without_connect](#HAL_UDP_create_without_connect)   | 创建一个本地UDP socket, 但不发起任何网络交互
| [HAL_UDP_close_without_connect](#HAL_UDP_close_without_connect)     | 销毁指定的UDP socket, 回收资源
| [HAL_UDP_joinmulticast](#HAL_UDP_joinmulticast)                     | 在指定的UDP socket上发送加入组播组的请求
| [HAL_UDP_sendto](#HAL_UDP_sendto)                                   | 在指定的UDP socket上发送指定缓冲区的指定长度, 阻塞时间不超过指定时长, 且指定长度若发送完需提前返回
| [HAL_UDP_recvfrom](#HAL_UDP_recvfrom)                               | 从指定的UDP句柄接收指定长度数据到缓冲区, 阻塞时间不超过指定时长, 且指定长度若接收完需提前返回, 源地址保存在出参中
| [HAL_Kv_Set](#HAL_Kv_Set)                                           | 写入指定KV数据
| [HAL_Kv_Get](#HAL_Kv_Get)                                           | 读取指定KV数据
| [HAL_Kv_Del](#HAL_Kv_Del)                                           | 删除指定KV数据
| [HAL_Kv_Erase_All](#HAL_Kv_Erase_All)                               | 擦除所有的KV数据

*[回到目录](#目录)*
### <a name="设备信息设置与获取相关">设备信息设置与获取相关</a>

| 函数名                                            | 说明
|---------------------------------------------------|-------------------------------------------------------------------------
| [HAL_GetChipID](#HAL_GetChipID)                     | 获取芯片ID
| [HAL_GetDeviceID](#HAL_GetDeviceID)                 | 获取设备ID
| [HAL_GetFirmwareVesion](#HAL_GetFirmwareVesion)     | 获取固件版本号, 必须实现
| [HAL_GetModuleID](#HAL_GetModuleID)                 | 获取模组ID, 用于紧密合作伙伴, 一般客户只需要在此可实现为空函数
| [HAL_GetPartnerID](#HAL_GetPartnerID)               | 获取合作伙伴ID, 用于紧密合作伙伴, 一般客户只需要在此可实现为空函数
| [HAL_GetDeviceName](#HAL_GetDeviceName)             | 获取DeviceName, 三元组获取函数之一, 必须实现
| [HAL_GetDeviceSecret](#HAL_GetDeviceSecret)         | 获取DeviceSecret, 三元组获取函数之一, 必须实现
| [HAL_GetProductKey](#HAL_GetProductKey)             | 获取ProductKey, 三元组获取函数之一, 必须实现
| [HAL_GetProductSecret](#HAL_GetProductSecret)       | 获取ProductSecret, 三元组获取函数之一, 必须实现
| [HAL_SetDeviceName](#HAL_SetDeviceName)             | 设置DeviceName, 三元组配置函数之一, 必须实现
| [HAL_SetDeviceSecret](#HAL_SetDeviceSecret)         | 设置DeviceSecret, 三元组配置函数之一, 必须实现
| [HAL_SetProductKey](#HAL_SetProductKey)             | 设置ProductKey, 三元组配置函数之一, 必须实现
| [HAL_SetProductSecret](#HAL_SetProductSecret)       | 设置ProductSecret, 三元组配置函数之一, 必须实现

*[回到目录](#目录)*
### <a name="OTA功能相关">OTA功能相关</a>

| 函数名                            | 说明
|-----------------------------------|-------------------------------------------------------------
| [HAL_Firmware_Persistence_Start](#HAL_Firmware_Persistence_Start)   | 固件持久化开始, 包含OTA功能时必须实现
| [HAL_Firmware_Persistence_Stop](#HAL_Firmware_Persistence_Stop)     | 固件持久化结束, 包含OTA功能时必须实现
| [HAL_Firmware_Persistence_Write](#HAL_Firmware_Persistence_Write)   | 固件持久化写入, 包含OTA功能时必须实现

*[回到目录](#目录)*
### <a name="本地定时任务功能相关">本地定时任务功能相关</a>

| 函数名                            | 说明
|-----------------------------------|--------------------------------------------------------------
| [HAL_UTC_Set](#HAL_UTC_Set)     | 设置UTC时间, 单位ms
| [HAL_UTC_Get](#HAL_UTC_Get)     | 获取UTC时间, 单位ms
|                                 |

*[回到目录](#目录)*
### <a name="WIFI配网功能相关">WIFI配网功能相关</a>
请查看`include/imports/iot_import_awss.h`


*[回到目录](#目录)*
## <a name="5.3 HAL层接口详解">5.3 HAL层接口详解</a>
*[回到目录](#目录)*
### <a name="HAL_Aes128_Cbc_Decrypt">HAL_Aes128_Cbc_Decrypt</a>

原型
---
```
typedef void *p_HAL_Aes128_t;

int HAL_Aes128_Cbc_Decrypt(
            _IN_ p_HAL_Aes128_t aes,
            _IN_ const void *src,
            _IN_ size_t blockNum,
            _OU_ void *dst);
```

接口说明
---
以`AES-CBC-128`的加解密模式, 用`HAL_Aes128_Init()`被调用时传入的密钥, 解密从`src`位置起长度为`blockNum`块数的密文, 并把明文结果存放到`dst`起始的内存缓冲区中

参数说明
---
| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------
| aes         | void *          | 输入      | 这是一个句柄, 是用户调用`HAL_Aes128_Init()`成功时所得到的返回值, 之后需要作为必选的入参传给所有AES加解密相关的HAL接口
| src         | const void *    | 输入      | 指定被解密的源数据的缓冲区首地址, 也就是AES密文的起始地址
| blockNum    | size_t          | 输入      | 指定被解密的源数据的缓冲区长度, 以16字节为一个Block, 此参数表达密文总长度是多少个Block, 或者说是16字节的多少倍
| dst         | void *          | 输出      | 指定被解密的目的数据的缓冲区首地址, 也就是AES解密后, 存放明文的首地址

返回值说明
---
0表示解密成功, -1表示解密失败

-----

*[回到目录](#目录)*
### <a name="HAL_Aes128_Cbc_Encrypt">HAL_Aes128_Cbc_Encrypt</a>

原型
---
```
int HAL_Aes128_Cbc_Encrypt(
            _IN_ p_HAL_Aes128_t aes,
            _IN_ const void *src,
            _IN_ size_t blockNum,
            _OU_ void *dst);
```

接口说明
---
以`AES-CBC-128`的加解密模式, 用`HAL_Aes128_Init()`被调用时传入的密钥, 加密从`src`位置起长度为`blockNum`块数的明文, 并把密文结果存放到`dst`起始的内存缓冲区中

参数说明
---
| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------
| aes         | void *          | 输入      | 这是一个句柄, 是用户调用`HAL_Aes128_Init()`成功时所得到的返回值, 之后需要作为必选的入参传给所有AES加解密相关的HAL接口
| src         | const void *    | 输入      | 指定被加密的源数据的缓冲区首地址, 也就是明文的起始地址
| blockNum    | size_t          | 输入      | 指定被加密的源数据的缓冲区长度, 以16字节为一个Block, 此参数表达明文总长度是多少个Block, 或者说是16字节的多少倍
| dst         | void *          | 输出      | 指定被加密的目的数据的缓冲区首地址, 也就是AES加密后, 存放AES密文的首地址

返回值说明
---
0表示加密成功, -1表示加密失败

-----

*[回到目录](#目录)*
### <a name="HAL_Aes128_Cfb_Decrypt">HAL_Aes128_Cfb_Decrypt</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Aes128_Cfb_Encrypt">HAL_Aes128_Cfb_Encrypt</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Aes128_Destroy">HAL_Aes128_Destroy</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Aes128_Init">HAL_Aes128_Init</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Awss_Close_Monitor">HAL_Awss_Close_Monitor</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Awss_Connect_Ap">HAL_Awss_Connect_Ap</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Awss_Get_Channelscan_Interval_Ms">HAL_Awss_Get_Channelscan_Interval_Ms</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Awss_Get_Connect_Default_Ssid_Timeout_Interval_Ms">HAL_Awss_Get_Connect_Default_Ssid_Timeout_Interval_Ms</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Awss_Get_Conn_Encrypt_Type">HAL_Awss_Get_Conn_Encrypt_Type</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Awss_Get_Encrypt_Type">HAL_Awss_Get_Encrypt_Type</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Awss_Get_Timeout_Interval_Ms">HAL_Awss_Get_Timeout_Interval_Ms</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Awss_Open_Monitor">HAL_Awss_Open_Monitor</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Awss_Switch_Channel">HAL_Awss_Switch_Channel</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Config_Read">HAL_Config_Read</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Config_Write">HAL_Config_Write</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_DTLSSession_create">HAL_DTLSSession_create</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_DTLSSession_free">HAL_DTLSSession_free</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_DTLSSession_read">HAL_DTLSSession_read</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_DTLSSession_write">HAL_DTLSSession_write</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Firmware_Persistence_Start">HAL_Firmware_Persistence_Start</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Firmware_Persistence_Stop">HAL_Firmware_Persistence_Stop</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Firmware_Persistence_Write">HAL_Firmware_Persistence_Write</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Free">HAL_Free</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_GetChipID">HAL_GetChipID</a>

原型
---
```
char *HAL_GetChipID(_OU_ char cid_str[HAL_CID_LEN]);
```

接口说明
---
获取唯一的芯片ID字符串

参数说明
---
| 参数         | 数据类型      | 方向  |说明
|--------------|--------------|-------|-------------------
| cid_str     | char    | 输出      | 存放芯片ID的字符串缓冲区

返回值说明
---
指向字符串缓冲区的指针

-----

*[回到目录](#目录)*
### <a name="HAL_GetDeviceID">HAL_GetDeviceID</a>

原型
---
```
int HAL_GetDeviceID(_OU_ char device_id[DEVICE_ID_LEN]);
```

接口说明
---
获取设备的`DeviceID`, 唯一标识设备的身份标识

参数说明
---
| 参数         | 数据类型      | 方向  |说明
|--------------|--------------|-------|-------------------
| device_id   | char    | 输出      | 存放`DeviceID`的字符串缓冲区

返回值说明
---
写入到`DeviceID`数组中的字符串长度

-----

*[回到目录](#目录)*
### <a name="HAL_GetDeviceName">HAL_GetDeviceName</a>

原型
---
```
int HAL_GetDeviceName(_OU_ char device_name[DEVICE_NAME_LEN]);
```

接口说明
---
获取设备的`DeviceName`, 用于唯一标识设备的名字, 三元组之一

参数说明
---
| 参数         | 数据类型      | 方向  |说明
|--------------|--------------|-------|-------------------
| device_name     | char    | 输出      | 存放`DeviceName`的字符串缓冲区

返回值说明
---
写入到`device_name`数组中的字符串长度

-----

*[回到目录](#目录)*
### <a name="HAL_GetDeviceSecret">HAL_GetDeviceSecret</a>

原型
---
```
int HAL_GetDeviceSecret(_OU_ char device_secret[DEVICE_SECRET_LEN]);
```

接口说明
---

参数说明
---
| 参数         | 数据类型      | 方向  |说明
|--------------|--------------|-------|-------------------
| device_secret   | char    | 输出      | 存放`DeviceName`的字符串缓冲区

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_GetFirmwareVesion">HAL_GetFirmwareVesion</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_GetModuleID">HAL_GetModuleID</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_GetPartnerID">HAL_GetPartnerID</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_GetProductKey">HAL_GetProductKey</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_GetProductSecret">HAL_GetProductSecret</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_GetTimeStr">HAL_GetTimeStr</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Kv_Del">HAL_Kv_Del</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Kv_Erase_All">HAL_Kv_Erase_All</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Kv_Get">HAL_Kv_Get</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Kv_Set">HAL_Kv_Set</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Malloc">HAL_Malloc</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_MutexCreate">HAL_MutexCreate</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_MutexDestroy">HAL_MutexDestroy</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_MutexLock">HAL_MutexLock</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_MutexUnlock">HAL_MutexUnlock</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Printf">HAL_Printf</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Random">HAL_Random</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Reboot">HAL_Reboot</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_RF433_Get_Rssi_Dbm">HAL_RF433_Get_Rssi_Dbm</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_SemaphoreCreate">HAL_SemaphoreCreate</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_SemaphoreDestroy">HAL_SemaphoreDestroy</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_SemaphorePost">HAL_SemaphorePost</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_SemaphoreWait">HAL_SemaphoreWait</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_SetDeviceName">HAL_SetDeviceName</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_SetDeviceSecret">HAL_SetDeviceSecret</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_SetProductKey">HAL_SetProductKey</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_SetProductSecret">HAL_SetProductSecret</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_SleepMs">HAL_SleepMs</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Snprintf">HAL_Snprintf</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Srandom">HAL_Srandom</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_SSL_Destroy">HAL_SSL_Destroy</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_SSL_Establish">HAL_SSL_Establish</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_SSL_Read">HAL_SSL_Read</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_SSL_Write">HAL_SSL_Write</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Sys_Net_Is_Ready">HAL_Sys_Net_Is_Ready</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Sys_reboot">HAL_Sys_reboot</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_TCP_Destroy">HAL_TCP_Destroy</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_TCP_Establish">HAL_TCP_Establish</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_TCP_Read">HAL_TCP_Read</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_TCP_Write">HAL_TCP_Write</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_ThreadCreate">HAL_ThreadCreate</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_ThreadDelete">HAL_ThreadDelete</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_ThreadDetach">HAL_ThreadDetach</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Timer_Create">HAL_Timer_Create</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Timer_Delete">HAL_Timer_Delete</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Timer_Start">HAL_Timer_Start</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Timer_Stop">HAL_Timer_Stop</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_UDP_bindtodevice">HAL_UDP_bindtodevice</a>

原型
---
```
int HAL_UDP_bindtodevice(_IN_ intptr_t fd,
                         _IN_ const char *ifname);
```

接口说明
---
绑定UDP socket到指定接口, 只接收来自该接口的数据包

参数说明
---
| 参数         | 数据类型      | 方向  |说明
|--------------|--------------|-------|-------------------
| fd      | intptr_t        | 输入      | 指定用来绑定的UDP socket
| ifname  | const char *    | 输入      | 指定用来绑定socket的网络接口名字

返回值说明
---
| 值       | 说明
|----------|-----------------
| < 0     | 绑定异常或失败
| = 0     | 绑定成功

-----

*[回到目录](#目录)*
### <a name="HAL_UDP_close">HAL_UDP_close</a>

原型
---
```
void HAL_UDP_close(_IN_ intptr_t p_socket);
```

接口说明
---
摧毁指定的UDP连接, 释放资源

参数说明
---
| 参数         | 数据类型      | 方向  |说明
|--------------|--------------|-------|-------------------
| p_socket    | intptr_t    | 输入      | UDP socket句柄

返回值说明
---
void

-----

*[回到目录](#目录)*
### <a name="HAL_UDP_close_without_connect">HAL_UDP_close_without_connect</a>

原型
---
```
int HAL_UDP_close_without_connect(_IN_ intptr_t sockfd);
```

接口说明
---
摧毁指定的UDP连接, 释放资源

参数说明
---
| 参数         | 数据类型      | 方向  |说明
|--------------|--------------|-------|-------------------
| sockfd  | intptr_t    | 输入      | UDP socket句柄

返回值说明
---
| 值       | 说明
|----------|-----------------
| < 0     | 操作失败
| = 0     | 操作成功

-----

*[回到目录](#目录)*
### <a name="HAL_UDP_connect">HAL_UDP_connect</a>

原型
---
```
int HAL_UDP_connect(_IN_ intptr_t sockfd,
                    _IN_ const char *host,
                    _IN_ unsigned short port);
```

接口说明
---
设置UDP socket的目的地址和目的端口

参数说明
---
| 参数         | 数据类型      | 方向  |说明
|--------------|--------------|-------|-------------------
| sockfd  | intptr_t        | 输入      | UDP socket句柄
| host    | const char *    | 输入      | UDP目的地址
| port    | unsigned short  | 输入      | UDP目的端口

返回值说明
---
| 值       | 说明
|----------|-----------------
| < 0     | 操作失败
| = 0     | 操作成功

-----

*[回到目录](#目录)*
### <a name="HAL_UDP_create">HAL_UDP_create</a>

原型
---
```
intptr_t HAL_UDP_create(_IN_ char *host, _IN_ unsigned short port);
```

接口说明
---
建立指定目的地址和目的端口的UDP连接

参数说明
---
| 参数         | 数据类型      | 方向  |说明
|--------------|--------------|-------|-------------------
| host    | const char *    | 输入      | UDP目的地址
| port    | unsigned short  | 输入      | UDP目的端口

返回值说明
---
| 值       | 说明
|----------|-----------------
| < 0     | 操作失败
| >= 0    | 操作成功, 返回值为UDP socket句柄

-----

*[回到目录](#目录)*
### <a name="HAL_UDP_create_without_connect">HAL_UDP_create_without_connect</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_UDP_joinmulticast">HAL_UDP_joinmulticast</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_UDP_read">HAL_UDP_read</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_UDP_readTimeout">HAL_UDP_readTimeout</a>

原型
---
```
int HAL_UDP_readTimeout(
            _IN_ intptr_t p_socket,
            _OU_ unsigned char *p_data,
            _IN_ unsigned int datalen,
            _IN_ unsigned int timeout_ms);
```

接口说明
---
从指定的UDP句柄读取指定长度数据到缓冲区, 阻塞时间不超过指定时长, 若读取到指定长度数据完需立即返回, 调用该接口之前需要调用HAL_UDP_connect()设置好源地址和端口


参数说明
---


返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_UDP_recv">HAL_UDP_recv</a>

原型
---
```
int HAL_UDP_recv(_IN_ intptr_t sockfd,
                 _OU_ unsigned char *p_data,
                 _IN_ unsigned int datalen,
                 _IN_ unsigned int timeout_ms);
```

接口说明
---
从指定的UDP句柄接收指定长度数据到缓冲区, 阻塞时间不超过指定时长, 且指定长度若接收完需提前返回, 调用该接口之前需要调用HAL_UDP_connect()设置好源地址和端口

参数说明
---
| 参数         | 数据类型      | 方向  |说明
|--------------|--------------|-------|-------------------
| sockfd      | intptr_t            | 输入      | UDP socket句柄
| p_data      | unsigned char *     | 输出      | 指向数据接收缓冲区的指针
| datalen     | unsigned int        | 输入      | 接收缓冲区的字节大小
| timeout_ms  | unsigned int        | 输入      | 阻塞的超时时间, 单位ms

返回值说明
---
| 值       | 说明
|----------|-----------------
| < 0     | 接收过程中出现错误或异常
| = 0     | 在指定的`timeout_ms`时间内, 没有接收到任何数据
| > 0     | 在指定的`timeout_ms`时间内, 实际接收到的数据字节数

-----

*[回到目录](#目录)*
### <a name="HAL_UDP_recvfrom">HAL_UDP_recvfrom</a>

原型
---
```
int HAL_UDP_recvfrom(_IN_ intptr_t sockfd,
                     _OU_ NetworkAddr *p_remote,
                     _OU_ unsigned char *p_data,
                     _IN_ unsigned int datalen,
                     _IN_ unsigned int timeout_ms);
```

接口说明
---
从指定的UDP句柄接收指定长度数据到缓冲区, 阻塞时间不超过指定时长, 且指定长度若接收完需提前返回, 源地址保存在`p_remote`参数中

参数说明
---
| 参数         | 数据类型      | 方向  |说明
|--------------|--------------|-------|-------------------
| sockfd      | intptr_t            | 输入      | UDP socket句柄
| p_remote    | NetworkAddr *       | 输出      | 指向存放源网络地址的指针
| p_data      | unsigned char *     | 输出      | 指向数据接收缓冲区的指针
| datalen     | unsigned int        | 输入      | 接收缓冲区的字节大小
| timeout_ms  | unsigned int        | 输入      | 阻塞的超时时间, 单位ms

返回值说明
---
| 值       | 说明
|----------|-----------------
| < 0     | 接收过程中出现错误或异常
| = 0     | 在指定的`timeout_ms`时间内, 没有接收到任何数据
| > 0     | 在指定的`timeout_ms`时间内, 实际接收到的数据字节数

-----

*[回到目录](#目录)*
### <a name="HAL_UDP_send">HAL_UDP_send</a>

原型
---
```
int HAL_UDP_sendto(_IN_ intptr_t          sockfd,
                   _IN_ const NetworkAddr *p_remote,
                   _IN_ const unsigned char *p_data,
                   _IN_ unsigned int datalen,
                   _IN_ unsigned int timeout_ms);
```

接口说明
---
向指定的UDP句柄发送指定缓冲区的指定长度, 阻塞时间不超过指定时长, 且指定长度若发送完需提前返回, 调用该接口之前需要调用HAL_UDP_connect()设置好目的地址和端口

参数说明
---
| 参数         | 数据类型      | 方向  |说明
|--------------|--------------|-------|-------------------
| sockfd      | intptr_t                | 输入      | UDP socket句柄
| p_data      | const unsigned char *   | 输入      | 指向待发送数据缓冲区的指针
| datalen     | unsigned int            | 输入      | 待发送数据的字节长度
| timeout_ms  | unsigned int            | 输入      | 阻塞的超时时间, 单位ms

返回值说明
---
| 值       | 说明
|----------|-----------------
| < 0     | 发送过程中出现错误或异常
| = 0     | 在指定的`timeout_ms`时间内, 没有任何数据发送成功
| > 0     | 在指定的`timeout_ms`时间内, 实际发送的数据字节数

-----

*[回到目录](#目录)*
### <a name="HAL_UDP_sendto">HAL_UDP_sendto</a>

原型
---
```
int HAL_UDP_sendto(_IN_ intptr_t          sockfd,
                   _IN_ const NetworkAddr *p_remote,
                   _IN_ const unsigned char *p_data,
                   _IN_ unsigned int datalen,
                   _IN_ unsigned int timeout_ms);
```

接口说明
---
向指定UDP句柄发送指定长度的数据, 阻塞时间不超过指定时长, 且指定长度若发送完需提前返回

参数说明
---
| 参数         | 数据类型      | 方向  |说明
|--------------|--------------|-------|-------------------
| sockfd      | intptr_t                | 输入      | UDP socket句柄
| p_remote    | const NetworkAddr *     | 输入      | 指向目标网络地址的指针
| p_data      | const unsigned char *   | 输入      | 指向待发送数据缓冲区的指针
| datalen     | unsigned int            | 输入      | 待发送数据的字节长度
| timeout_ms  | unsigned int            | 输入      | 阻塞的超时时间, 单位ms

返回值说明
---
| 值       | 说明
|----------|-----------------
| < 0     | 发送过程中出现错误或异常
| = 0     | 在指定的`timeout_ms`时间内, 没有任何数据发送成功
| > 0     | 在指定的`timeout_ms`时间内, 实际发送的数据字节数

-----

*[回到目录](#目录)*
### <a name="HAL_UDP_write">HAL_UDP_write</a>

原型
---
```
int HAL_UDP_write(
            _IN_ intptr_t p_socket,
            _IN_ const unsigned char *p_data,
            _IN_ unsigned int datalen);
```

接口说明
---
向指定UDP句柄写入指定字节长度的数据

参数说明
---
| 参数         | 数据类型      | 方向  |说明
|--------------|--------------|-------|-------------------
| p_socket    | intptr_t                | 输入      | 用于标识连接的描述符
| p_data      | const unsigned char *   | 输入      | 指向待写入数据buffer的指针
| datalen     | unsigned int            | 输入      | 待写入数据的字节长度

返回值说明
---
| 值       | 说明
|----------|-----------------
| < 0     | UDP连接发生错误
| = 0     | EOF, 文件已结束
| > 0     | 实际写入的字节数

-----

*[回到目录](#目录)*
### <a name="HAL_UptimeMs">HAL_UptimeMs</a>

原型
---
```
uint64_t HAL_UptimeMs(void);
```

接口说明
---
获取设备从上电到当前时刻所经过的毫秒数

参数说明
---
void

返回值说明
---
设备从上电到当前时刻所经过的毫秒数

-----

*[回到目录](#目录)*
### <a name="HAL_UTC_Get">HAL_UTC_Get</a>

原型
---
```
long long HAL_UTC_Get(void);
```

接口说明
---
获取UTC时间, 单位为ms

参数说明
---
void

返回值说明
---
单位为ms的UTC时间

-----

*[回到目录](#目录)*
### <a name="HAL_UTC_Set">HAL_UTC_Set</a>

原型
---
```
void HAL_UTC_Set(long long ms);
```

接口说明
---
设置UTC时间, 单位为ms

参数说明
---
| 参数         | 数据类型      | 方向  |说明
|--------------|--------------|-------|-------------------
| ms  | long long   | 输入      | 单位为ms的UTC时间

返回值说明
---
void

-----

*[回到目录](#目录)*
### <a name="HAL_Vsnprintf">HAL_Vsnprintf</a>

原型
---
```
int HAL_Vsnprintf(_OU_ char *str, _IN_ const int len, _IN_ const char *fmt, _IN_ va_list ap);
```

接口说明
---
格式化输出字符串到指定buffer中

参数说明
---
| 参数         | 数据类型      | 方向  |说明
|--------------|--------------|-------|-------------------
| str     | char *      | 输出      | 用于存放写入字符串的buffer
| len     | const int   | 输入      | 允许写入的最大字符串长度
| fmt     | const char  | 输入      | 格式化字符串
| ap      | va_list     | 输入      | 可变参数列表

返回值说明
---
成功写入的字符串长

-----

*[回到目录](#目录)*
### <a name="HAL_Wifi_Enable_Mgmt_Frame_Filter">HAL_Wifi_Enable_Mgmt_Frame_Filter</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Wifi_Get_Ap_Info">HAL_Wifi_Get_Ap_Info</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Wifi_Get_IP">HAL_Wifi_Get_IP</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Wifi_Get_Mac">HAL_Wifi_Get_Mac</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Wifi_Get_Os_Version">HAL_Wifi_Get_Os_Version</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Wifi_Get_Rssi_Dbm">HAL_Wifi_Get_Rssi_Dbm</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Wifi_Low_Power">HAL_Wifi_Low_Power</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Wifi_Scan">HAL_Wifi_Scan</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="HAL_Wifi_Send_80211_Raw_Frame">HAL_Wifi_Send_80211_Raw_Frame</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

第六章 目录
---
+ [第六章 API说明](#第六章 API说明)
    * [6.1 API接口清单](#6.1 API接口清单)
        - [基础版API清单](#基础版API清单)
        - [高级版单品场景API清单](#高级版单品场景API清单)
        - [高级版网关场景API清单](#高级版网关场景API清单)
    * [6.2 API接口列表](#6.2 API接口列表)
        - [基础相关](#基础相关)
        - [CoAP功能相关](#CoAP功能相关)
        - [MQTT功能相关](#MQTT功能相关)
        - [OTA功能相关](#OTA功能相关)
        - [HTTP功能相关](#HTTP功能相关)
        - [设备影子相关](#设备影子相关)
        - [主子设备相关(老版本接口, 不推荐使用)](#主子设备相关(老版本接口, 不推荐使用))
        - [linkkit单品相关](#linkkit单品相关)
        - [linkkit_gateway网关相关](#linkkit_gateway网关相关)
    * [6.3 新版API接口详解](#6.3 新版API接口详解)
        - [IOT_OpenLog](#IOT_OpenLog)
        - [IOT_CloseLog](#IOT_CloseLog)
        - [IOT_SetLogLevel](#IOT_SetLogLevel)
        - [IOT_DumpMemoryStats](#IOT_DumpMemoryStats)
        - [IOT_SetupConnInfo](#IOT_SetupConnInfo)
        - [IOT_Ioctl](#IOT_Ioctl)
        - [IOT_CoAP_Init](#IOT_CoAP_Init)
        - [IOT_CoAP_Deinit](#IOT_CoAP_Deinit)
        - [IOT_CoAP_DeviceNameAuth](#IOT_CoAP_DeviceNameAuth)
        - [IOT_CoAP_Yield](#IOT_CoAP_Yield)
        - [IOT_CoAP_SendMessage](#IOT_CoAP_SendMessage)
        - [IOT_CoAP_GetMessagePayload](#IOT_CoAP_GetMessagePayload)
        - [IOT_CoAP_GetMessageCode](#IOT_CoAP_GetMessageCode)
        - [IOT_HTTP_Init](#IOT_HTTP_Init)
        - [IOT_HTTP_DeInit](#IOT_HTTP_DeInit)
        - [IOT_HTTP_DeviceNameAuth](#IOT_HTTP_DeviceNameAuth)
        - [IOT_HTTP_SendMessage](#IOT_HTTP_SendMessage)
        - [IOT_HTTP_Disconnect](#IOT_HTTP_Disconnect)
        - [IOT_Linkkit_Open](#IOT_Linkkit_Open)
        - [IOT_Linkkit_Ioctl](#IOT_Linkkit_Ioctl)
        - [IOT_Linkkit_Connect](#IOT_Linkkit_Connect)
        - [IOT_Linkkit_Yield](#IOT_Linkkit_Yield)
        - [IOT_Linkkit_Close](#IOT_Linkkit_Close)
        - [IOT_Linkkit_Post](#IOT_Linkkit_Post)
        - [IOT_Linkkit_TriggerEvent](#IOT_Linkkit_TriggerEvent)
        - [IOT_MQTT_Construct](#IOT_MQTT_Construct)
        - [IOT_MQTT_Destroy](#IOT_MQTT_Destroy)
        - [IOT_MQTT_Yield](#IOT_MQTT_Yield)
        - [IOT_MQTT_LogPost](#IOT_MQTT_LogPost)
        - [IOT_MQTT_CheckStateNormal](#IOT_MQTT_CheckStateNormal)
        - [IOT_MQTT_Subscribe](#IOT_MQTT_Subscribe)
        - [IOT_MQTT_Subscribe_Sync](#IOT_MQTT_Subscribe_Sync)
        - [IOT_MQTT_Unsubscribe](#IOT_MQTT_Unsubscribe)
        - [IOT_MQTT_Publish](#IOT_MQTT_Publish)
        - [IOT_OTA_Init](#IOT_OTA_Init)
        - [IOT_OTA_Deinit](#IOT_OTA_Deinit)
        - [IOT_OTA_ReportVersion](#IOT_OTA_ReportVersion)
        - [IOT_OTA_RequestImage](#IOT_OTA_RequestImage)
        - [IOT_OTA_ReportProgress](#IOT_OTA_ReportProgress)
        - [IOT_OTA_GetConfig](#IOT_OTA_GetConfig)
        - [IOT_OTA_IsFetching](#IOT_OTA_IsFetching)
        - [IOT_OTA_IsFetchFinish](#IOT_OTA_IsFetchFinish)
        - [IOT_OTA_FetchYield](#IOT_OTA_FetchYield)
        - [IOT_OTA_Ioctl](#IOT_OTA_Ioctl)
        - [IOT_OTA_GetLastError](#IOT_OTA_GetLastError)
        - [IOT_Shadow_Construct](#IOT_Shadow_Construct)
        - [IOT_Shadow_Destroy](#IOT_Shadow_Destroy)
        - [IOT_Shadow_Yield](#IOT_Shadow_Yield)
        - [IOT_Shadow_RegisterAttribute](#IOT_Shadow_RegisterAttribute)
        - [IOT_Shadow_DeleteAttribute](#IOT_Shadow_DeleteAttribute)
        - [IOT_Shadow_PushFormat_Init](#IOT_Shadow_PushFormat_Init)
        - [IOT_Shadow_PushFormat_Add](#IOT_Shadow_PushFormat_Add)
        - [IOT_Shadow_PushFormat_Finalize](#IOT_Shadow_PushFormat_Finalize)
        - [IOT_Shadow_Push](#IOT_Shadow_Push)
        - [IOT_Shadow_Push_Async](#IOT_Shadow_Push_Async)
        - [IOT_Shadow_Pull](#IOT_Shadow_Pull)
        - [IOT_Gateway_Generate_Message_ID](#IOT_Gateway_Generate_Message_ID)
        - [IOT_Gateway_Construct](#IOT_Gateway_Construct)
        - [IOT_Gateway_Destroy](#IOT_Gateway_Destroy)
        - [IOT_Subdevice_Register](#IOT_Subdevice_Register)
        - [IOT_Subdevice_Unregister](#IOT_Subdevice_Unregister)
        - [IOT_Subdevice_Login](#IOT_Subdevice_Login)
        - [IOT_Subdevice_Logout](#IOT_Subdevice_Logout)
        - [IOT_Gateway_Get_TOPO](#IOT_Gateway_Get_TOPO)
        - [IOT_Gateway_Get_Config](#IOT_Gateway_Get_Config)
        - [IOT_Gateway_Publish_Found_List](#IOT_Gateway_Publish_Found_List)
        - [IOT_Gateway_Yield](#IOT_Gateway_Yield)
        - [IOT_Gateway_Subscribe](#IOT_Gateway_Subscribe)
        - [IOT_Gateway_Unsubscribe](#IOT_Gateway_Unsubscribe)
        - [IOT_Gateway_Publish](#IOT_Gateway_Publish)
        - [IOT_Gateway_RRPC_Register](#IOT_Gateway_RRPC_Register)
        - [IOT_Gateway_RRPC_Response](#IOT_Gateway_RRPC_Response)
    * [6.4 高级版(旧版)单品API详解](#6.4 高级版(旧版)单品API详解)
        - [linkkit_answer_service](#linkkit_answer_service)
        - [linkkit_cota_init](#linkkit_cota_init)
        - [linkkit_dispatch](#linkkit_dispatch)
        - [linkkit_end](#linkkit_end)
        - [linkkit_fota_init](#linkkit_fota_init)
        - [linkkit_get_value](#linkkit_get_value)
        - [linkkit_invoke_cota_get_config](#linkkit_invoke_cota_get_config)
        - [linkkit_invoke_cota_service](#linkkit_invoke_cota_service)
        - [linkkit_invoke_fota_service](#linkkit_invoke_fota_service)
        - [linkkit_invoke_raw_service](#linkkit_invoke_raw_service)
        - [linkkit_is_end](#linkkit_is_end)
        - [linkkit_is_try_leave](#linkkit_is_try_leave)
        - [linkkit_post_property](#linkkit_post_property)
        - [linkkit_set_opt](#linkkit_set_opt)
        - [linkkit_set_tsl](#linkkit_set_tsl)
        - [linkkit_set_value](#linkkit_set_value)
        - [linkkit_start](#linkkit_start)
        - [linkkit_trigger_event](#linkkit_trigger_event)
        - [linkkit_trigger_extended_info_operate](#linkkit_trigger_extended_info_operate)
        - [linkkit_try_leave](#linkkit_try_leave)
        - [linkkit_yield](#linkkit_yield)
    * [6.5 高级版(旧版)网关API详解](#6.5 高级版(旧版)网关API详解)
        - [linkkit_gateway_delete_extinfos](#linkkit_gateway_delete_extinfos)
        - [linkkit_gateway_exit](#linkkit_gateway_exit)
        - [linkkit_gateway_fota_init](#linkkit_gateway_fota_init)
        - [linkkit_gateway_get_default_params](#linkkit_gateway_get_default_params)
        - [linkkit_gateway_get_devinfo](#linkkit_gateway_get_devinfo)
        - [linkkit_gateway_get_num_devices](#linkkit_gateway_get_num_devices)
        - [linkkit_gateway_init](#linkkit_gateway_init)
        - [linkkit_gateway_invoke_fota_service](#linkkit_gateway_invoke_fota_service)
        - [linkkit_gateway_post_extinfos](#linkkit_gateway_post_extinfos)
        - [linkkit_gateway_post_property_json](#linkkit_gateway_post_property_json)
        - [linkkit_gateway_post_property_json_sync](#linkkit_gateway_post_property_json_sync)
        - [linkkit_gateway_post_rawdata](#linkkit_gateway_post_rawdata)
        - [linkkit_gateway_set_event_callback](#linkkit_gateway_set_event_callback)
        - [linkkit_gateway_setopt](#linkkit_gateway_setopt)
        - [linkkit_gateway_start](#linkkit_gateway_start)
        - [linkkit_gateway_stop](#linkkit_gateway_stop)
        - [linkkit_gateway_subdev_create](#linkkit_gateway_subdev_create)
        - [linkkit_gateway_subdev_destroy](#linkkit_gateway_subdev_destroy)
        - [linkkit_gateway_subdev_login](#linkkit_gateway_subdev_login)
        - [linkkit_gateway_subdev_logout](#linkkit_gateway_subdev_logout)
        - [linkkit_gateway_subdev_register](#linkkit_gateway_subdev_register)
        - [linkkit_gateway_subdev_unregister](#linkkit_gateway_subdev_unregister)
        - [linkkit_gateway_trigger_event_json](#linkkit_gateway_trigger_event_json)
        - [linkkit_gateway_trigger_event_json_sync](#linkkit_gateway_trigger_event_json_sync)




*[回到目录](#目录)*
# <a name="第六章 API说明">第六章 API说明</a>

> 本节说明 C-SDK 向客户提供的功能和对应的API, 用于让客户编写业务逻辑. 另外模组商/芯片商封装AT命令时, 也是基于这些API的
>
>> 更加准确详细和权威的描述, 以代码 `include/iot_export.h`, 以及 `include/exports/*.h` 中的注释为准
>>
>> 如何使用这些API编写应用逻辑, 以代码 `examples/*/*.c` 的示例程序为准

*[回到目录](#目录)*
## <a name="6.1 API接口清单">6.1 API接口清单</a>
如下列出当前SDK代码提供的所有面向用户的API函数:

*[回到目录](#目录)*
### <a name="基础版API清单">基础版API清单</a>

    $ cd include
    $ grep -o "IOT_[A-Z][_a-zA-Z]*[^_]\> *(" iot_export.h exports/*.h|sed 's!.*:\(.*\)(!\1!'|cat -n

     1  IOT_OpenLog
     2  IOT_CloseLog
     3  IOT_SetLogLevel
     4  IOT_DumpMemoryStats
     5  IOT_SetupConnInfo
     6  IOT_Ioctl
     7  IOT_CoAP_Init
     8  IOT_CoAP_Deinit
     9  IOT_CoAP_DeviceNameAuth
    10  IOT_CoAP_Yield
    11  IOT_CoAP_SendMessage
    12  IOT_CoAP_GetMessagePayload
    13  IOT_CoAP_GetMessageCode
    14  IOT_HTTP_Init
    15  IOT_HTTP_DeInit
    16  IOT_HTTP_DeviceNameAuth
    17  IOT_HTTP_SendMessage
    18  IOT_HTTP_Disconnect
    19  IOT_Linkkit_Open
    20  IOT_Linkkit_Ioctl
    21  IOT_Linkkit_Connect
    22  IOT_Linkkit_Yield
    23  IOT_Linkkit_Close
    24  IOT_Linkkit_Post
    25  IOT_Linkkit_TriggerEvent
    26  IOT_MQTT_Construct
    27  IOT_MQTT_Destroy
    28  IOT_MQTT_Yield
    29  IOT_MQTT_LogPost
    30  IOT_MQTT_CheckStateNormal
    31  IOT_MQTT_Subscribe
    32  IOT_MQTT_Subscribe_Sync
    33  IOT_MQTT_Unsubscribe
    34  IOT_MQTT_Publish
    35  IOT_OTA_Init
    36  IOT_OTA_Deinit
    37  IOT_OTA_ReportVersion
    38  IOT_OTA_RequestImage
    39  IOT_OTA_ReportProgress
    40  IOT_OTA_GetConfig
    41  IOT_OTA_IsFetching
    42  IOT_OTA_IsFetchFinish
    43  IOT_OTA_FetchYield
    44  IOT_OTA_Ioctl
    45  IOT_OTA_GetLastError
    46  IOT_Shadow_Construct
    47  IOT_Shadow_Destroy
    48  IOT_Shadow_Yield
    49  IOT_Shadow_RegisterAttribute
    50  IOT_Shadow_DeleteAttribute
    51  IOT_Shadow_PushFormat_Init
    52  IOT_Shadow_PushFormat_Add
    53  IOT_Shadow_PushFormat_Finalize
    54  IOT_Shadow_Push
    55  IOT_Shadow_Push_Async
    56  IOT_Shadow_Pull
    57  IOT_Gateway_Generate_Message_ID
    58  IOT_Gateway_Construct
    59  IOT_Gateway_Destroy
    60  IOT_Subdevice_Register
    61  IOT_Subdevice_Unregister
    62  IOT_Subdevice_Login
    63  IOT_Subdevice_Logout
    64  IOT_Gateway_Get_TOPO
    65  IOT_Gateway_Get_Config
    66  IOT_Gateway_Publish_Found_List
    67  IOT_Gateway_Yield
    68  IOT_Gateway_Subscribe
    69  IOT_Gateway_Unsubscribe
    70  IOT_Gateway_Publish
    71  IOT_Gateway_RRPC_Register
    72  IOT_Gateway_RRPC_Response

*[回到目录](#目录)*
### <a name="高级版单品场景API清单">高级版单品场景API清单</a>

    $ cd include
    $ grep -o "linkkit_[_a-z]* *(" iot_export.h exports/*.h|sed 's!.*:\(.*\)(!\1!'|sort -u|grep -v gateway|cat -n

     1  linkkit_answer_service
     2  linkkit_cota_init
     3  linkkit_dispatch
     4  linkkit_end
     5  linkkit_fota_init
     6  linkkit_get_value
     7  linkkit_invoke_cota_get_config
     8  linkkit_invoke_cota_service
     9  linkkit_invoke_fota_service
    10  linkkit_invoke_raw_service
    11  linkkit_is_end
    12  linkkit_is_try_leave
    13  linkkit_post_property
    14  linkkit_set_opt
    15  linkkit_set_tsl
    16  linkkit_set_value
    17  linkkit_start
    18  linkkit_trigger_event
    19  linkkit_trigger_extended_info_operate
    20  linkkit_try_leave
    21  linkkit_yield

*[回到目录](#目录)*
### <a name="高级版网关场景API清单">高级版网关场景API清单</a>

    $ cd include
    $ grep -o "linkkit_gateway_[_a-z]* *(" iot_export.h exports/*.h|sed 's!.*:\(.*\)(!\1!'|sort -u|cat -n

     1  linkkit_gateway_delete_extinfos
     2  linkkit_gateway_exit
     3  linkkit_gateway_fota_init
     4  linkkit_gateway_get_default_params
     5  linkkit_gateway_get_devinfo
     6  linkkit_gateway_get_num_devices
     7  linkkit_gateway_init
     8  linkkit_gateway_invoke_fota_service
     9  linkkit_gateway_post_extinfos
    10  linkkit_gateway_post_property_json
    11  linkkit_gateway_post_property_json_sync
    12  linkkit_gateway_post_rawdata
    13  linkkit_gateway_set_event_callback
    14  linkkit_gateway_setopt
    15  linkkit_gateway_start
    16  linkkit_gateway_stop
    17  linkkit_gateway_subdev_create
    18  linkkit_gateway_subdev_destroy
    19  linkkit_gateway_subdev_login
    20  linkkit_gateway_subdev_logout
    21  linkkit_gateway_subdev_register
    22  linkkit_gateway_subdev_unregister
    23  linkkit_gateway_trigger_event_json
    24  linkkit_gateway_trigger_event_json_sync

*[回到目录](#目录)*
## <a name="6.2 API接口列表">6.2 API接口列表</a>

*[回到目录](#目录)*
### <a name="基础相关">基础相关</a>

| 函数名                       | 说明
|------------------------------|--------------------------------------------------------------------
| [IOT_OpenLog](#IOT_OpenLog)                     | 开始打印日志信息(log), 接受一个const char *为入参, 表示模块名字
| [IOT_CloseLog](#IOT_CloseLog)                   | 停止打印日志信息(log), 入参为空
| [IOT_SetLogLevel](#IOT_SetLogLevel)             | 设置打印的日志等级, 接受入参从1到5, 数字越大, 打印越详细
| [IOT_DumpMemoryStats](#IOT_DumpMemoryStats)     | 调试函数, 打印内存的使用统计情况, 入参为1-5, 数字越大, 打印越详细
| [IOT_Ioctl](#IOT_Ioctl)                         | 设置SDK运行时可配置选项, 详情见API注释

*[回到目录](#目录)*
### <a name="CoAP功能相关">CoAP功能相关</a>

| 函数名                       | 说明
|------------------------------|--------------------------------------------------------------------------------
| [IOT_CoAP_Init](#IOT_CoAP_Init)                             | CoAP实例的构造函数, 入参为`iotx_coap_config_t`结构体, 返回创建的CoAP会话句柄
| [IOT_CoAP_Deinit](#IOT_CoAP_Deinit)                         | CoAP实例的摧毁函数, 入参为 [IOT_CoAP_Init](#IOT_CoAP_Init) 所创建的句柄
| [IOT_CoAP_DeviceNameAuth](#IOT_CoAP_DeviceNameAuth)         | 基于控制台申请的`DeviceName`, `DeviceSecret`, `ProductKey`做设备认证
| [IOT_CoAP_GetMessageCode](#IOT_CoAP_GetMessageCode)         | CoAP会话阶段, 从服务器的`CoAP Response`报文中获取`Respond Code`
| [IOT_CoAP_GetMessagePayload](#IOT_CoAP_GetMessagePayload)   | CoAP会话阶段, 从服务器的`CoAP Response`报文中获取报文负载
| [IOT_CoAP_SendMessage](#IOT_CoAP_SendMessage)               | CoAP会话阶段, 连接已成功建立后调用, 组织一个完整的CoAP报文向服务器发送
| [IOT_CoAP_Yield](#IOT_CoAP_Yield)                           | CoAP会话阶段, 连接已成功建立后调用, 检查和收取服务器对`CoAP Request`的回复报文

*[回到目录](#目录)*
### <a name="MQTT功能相关">MQTT功能相关</a>

| 函数名                       | 说明
|------------------------------|--------------------------------------------------------------------------------
| [IOT_SetupConnInfo](#IOT_SetupConnInfo)                     | MQTT连接前的准备, 基于`DeviceName + DeviceSecret + ProductKey`产生MQTT连接的用户名和密码等
| [IOT_MQTT_CheckStateNormal](#IOT_MQTT_CheckStateNormal)     | MQTT连接后, 调用此函数检查长连接是否正常
| [IOT_MQTT_Construct](#IOT_MQTT_Construct)                   | MQTT实例的构造函数, 入参为`iotx_mqtt_param_t`结构体, 连接MQTT服务器, 并返回被创建句柄
| [IOT_MQTT_Destroy](#IOT_MQTT_Destroy)                       | MQTT实例的摧毁函数, 入参为 [IOT_MQTT_Construct](#IOT_MQTT_Construct) 创建的句柄
| [IOT_MQTT_Publish](#IOT_MQTT_Publish)                       | MQTT会话阶段, 组织一个完整的`MQTT Publish`报文, 向服务端发送消息发布报文
| [IOT_MQTT_Subscribe](#IOT_MQTT_Subscribe)                   | MQTT会话阶段, 组织一个完整的`MQTT Subscribe`报文, 向服务端发送订阅请求
| [IOT_MQTT_Unsubscribe](#IOT_MQTT_Unsubscribe)               | MQTT会话阶段, 组织一个完整的`MQTT UnSubscribe`报文, 向服务端发送取消订阅请求
| [IOT_MQTT_Yield](#IOT_MQTT_Yield)                           | MQTT会话阶段, MQTT主循环函数, 内含了心跳的维持, 服务器下行报文的收取等

*[回到目录](#目录)*
### <a name="OTA功能相关">OTA功能相关</a>

| 函数名                       | 说明
|------------------------------|----------------------------------------------------------------------------------------
| [IOT_OTA_Init](#IOT_OTA_Init)                       | OTA实例的构造函数, 创建一个OTA会话的句柄并返回
| [IOT_OTA_Deinit](#IOT_OTA_Deinit)                   | OTA实例的摧毁函数, 销毁所有相关的数据结构
| [IOT_OTA_Ioctl](#IOT_OTA_Ioctl)                     | OTA实例的输入输出函数, 根据不同的命令字可以设置OTA会话的属性, 或者获取OTA会话的状态
| [IOT_OTA_GetLastError](#IOT_OTA_GetLastError)       | OTA会话阶段, 若某个 IOT_OTA_XXX() 函数返回错误, 调用此接口可获得最近一次的详细错误码
| [IOT_OTA_ReportVersion](#IOT_OTA_ReportVersion)     | OTA会话阶段, 向服务端汇报当前的固件版本号
| [IOT_OTA_FetchYield](#IOT_OTA_FetchYield)           | OTA下载阶段, 在指定的`timeout`时间内, 从固件服务器下载一段固件内容, 保存在入参buffer中
| [IOT_OTA_IsFetchFinish](#IOT_OTA_IsFetchFinish)     | OTA下载阶段, 判断迭代调用 [IOT_OTA_FetchYield](#IOT_OTA_FetchYield) 是否已经下载完所有的固件内容
| [IOT_OTA_IsFetching](#IOT_OTA_IsFetching)           | OTA下载阶段, 判断固件下载是否仍在进行中, 尚未完成全部固件内容的下载
| [IOT_OTA_ReportProgress](#IOT_OTA_ReportProgress)   | 可选API, OTA下载阶段, 调用此函数向服务端汇报已经下载了全部固件内容的百分之多少
| [IOT_OTA_RequestImage](#IOT_OTA_RequestImage)       | 可选API, 向服务端请求固件下载
| [IOT_OTA_GetConfig](#IOT_OTA_GetConfig)             | 可选API, 向服务端请求远程配置

*[回到目录](#目录)*
### <a name="HTTP功能相关">HTTP功能相关</a>

| 函数名                       | 说明
|------------------------------|----------------------------------------------------------------------------------------
| [IOT_HTTP_Init](#IOT_HTTP_Init)                         | Https实例的构造函数, 创建一个HTTP会话的句柄并返回
| [IOT_HTTP_DeInit](#IOT_HTTP_DeInit)                     | Https实例的摧毁函数, 销毁所有相关的数据结构
| [IOT_HTTP_DeviceNameAuth](#IOT_HTTP_DeviceNameAuth)     | 基于控制台申请的`DeviceName`, `DeviceSecret`, `ProductKey`做设备认证
| [IOT_HTTP_SendMessage](#IOT_HTTP_SendMessage)           | Https会话阶段, 组织一个完整的HTTP报文向服务器发送,并同步获取HTTP回复报文
| [IOT_HTTP_Disconnect](#IOT_HTTP_Disconnect)             | Https会话阶段, 关闭HTTP层面的连接, 但是仍然保持TLS层面的连接

*[回到目录](#目录)*
### <a name="设备影子相关">设备影子相关</a>

| 函数名                          | 说明
|---------------------------------|---------------------------------------------------------------------------------
| [IOT_Shadow_Construct](#IOT_Shadow_Construct)                       | 建立一个设备影子的MQTT连接, 并返回被创建的会话句柄
| [IOT_Shadow_Destroy](#IOT_Shadow_Destroy)                           | 摧毁一个设备影子的MQTT连接, 销毁所有相关的数据结构, 释放内存, 断开连接
| [IOT_Shadow_Pull](#IOT_Shadow_Pull)                                 | 把服务器端被缓存的JSON数据下拉到本地, 更新本地的数据属性
| [IOT_Shadow_Push](#IOT_Shadow_Push)                                 | 把本地的数据属性上推到服务器缓存的JSON数据, 更新服务端的数据属性
| [IOT_Shadow_Push_Async](#IOT_Shadow_Push_Async)                     | 和 [IOT_Shadow_Push](#IOT_Shadow_Push) 接口类似, 但是异步的, 上推后便返回, 不等待服务端回应
| [IOT_Shadow_PushFormat_Add](#IOT_Shadow_PushFormat_Add)             | 向已创建的数据类型格式中增添成员属性
| [IOT_Shadow_PushFormat_Finalize](#IOT_Shadow_PushFormat_Finalize)   | 完成一个数据类型格式的构造过程
| [IOT_Shadow_PushFormat_Init](#IOT_Shadow_PushFormat_Init)           | 开始一个数据类型格式的构造过程
| [IOT_Shadow_RegisterAttribute](#IOT_Shadow_RegisterAttribute)       | 创建一个数据类型注册到服务端, 注册时需要`*PushFormat*()`接口创建的数据类型格式
| [IOT_Shadow_DeleteAttribute](#IOT_Shadow_DeleteAttribute)           | 删除一个已被成功注册的数据属性
| [IOT_Shadow_Yield](#IOT_Shadow_Yield)                               | MQTT的主循环函数, 调用后接受服务端的下推消息, 更新本地的数据属性

*[回到目录](#目录)*
### <a name="主子设备相关(老版本接口, 不推荐使用)">主子设备相关(老版本接口, 不推荐使用)</a>

| 函数名                          | 说明
|---------------------------------|---------------------------------------------------------------------------------
| [IOT_Gateway_Construct](#IOT_Gateway_Construct)                         | 建立一个主设备, 建立MQTT连接, 并返回被创建的会话句柄
| [IOT_Gateway_Destroy](#IOT_Gateway_Destroy)                             | 摧毁一个主设备的MQTT连接, 销毁所有相关的数据结构, 释放内存, 断开连接
| [IOT_Subdevice_Login](#IOT_Subdevice_Login)                             | 子设备上线, 通知云端建立子设备session
| [IOT_Subdevice_Logout](#IOT_Subdevice_Logout)                           | 子设备下线, 销毁云端建立子设备session及所有相关的数据结构, 释放内存
| [IOT_Gateway_Yield](#IOT_Gateway_Yield)                                 | MQTT的主循环函数, 调用后接受服务端的下推消息
| [IOT_Gateway_Subscribe](#IOT_Gateway_Subscribe)                         | 通过MQTT连接向服务端发送订阅请求
| [IOT_Gateway_Unsubscribe](#IOT_Gateway_Unsubscribe)                     | 通过MQTT连接向服务端发送取消订阅请求
| [IOT_Gateway_Publish](#IOT_Gateway_Publish)                             | 通过MQTT连接服务端发送消息发布报文
| [IOT_Gateway_RRPC_Register](#IOT_Gateway_RRPC_Register)                 | 注册设备的RRPC回调函数, 接收云端发起的RRPC请求
| [IOT_Gateway_RRPC_Response](#IOT_Gateway_RRPC_Response)                 | 对云端的RRPC请求进行应答
| [IOT_Gateway_Generate_Message_ID](#IOT_Gateway_Generate_Message_ID)     | 生成消息id
| [IOT_Gateway_Get_TOPO](#IOT_Gateway_Get_TOPO)                           | 向topo/get topic发送包并等待回复(TOPIC_GET_REPLY 回复)
| [IOT_Gateway_Get_Config](#IOT_Gateway_Get_Config)                       | 向conifg/get topic发送包并等待回复(TOPIC_CONFIG_REPLY 回复)
| [IOT_Gateway_Publish_Found_List](#IOT_Gateway_Publish_Found_List)       | 发现设备列表上报

*[回到目录](#目录)*
### <a name="linkkit单品相关">linkkit单品相关</a>

| 函数名                          | 说明
|---------------------------------|---------------------------------------------------------------------------------
| [linkkit_start](#linkkit_start)                                                     | 启动 linkkit 服务, 与云端建立连接并安装回调函数
| [linkkit_end](#linkkit_end)                                                         | 停止 linkkit 服务, 与云端断开连接并回收资源
| [linkkit_dispatch](#linkkit_dispatch)                                               | 事件分发函数, 触发 [linkkit_start](#linkkit_start) 安装的回调
| [linkkit_yield](#linkkit_yield)                                                     | linkkit 主循环函数, 内含了心跳的维持, 服务器下行报文的收取等; 如果允许多线程, 请不要调用此函数
| [linkkit_set_value](#linkkit_set_value)                                             | 根据identifier设置物对象的 TSL 属性
| [linkkit_get_value](#linkkit_get_value)                                             | 根据identifier获取物对象的 TSL 属性
| [linkkit_set_tsl](#linkkit_set_tsl)                                                 | 从本地读取 TSL 文件,生成物的对象并添加到 linkkit 中
| [linkkit_answer_service](#linkkit_answer_service)                                   | 对云端服务请求进行回应
| [linkkit_invoke_raw_service](#linkkit_invoke_raw_service)                           | 向云端发送裸数据
| [linkkit_trigger_event](#linkkit_trigger_event)                                     | 上报设备事件到云端
| [linkkit_fota_init](#linkkit_fota_init)                                             | 初始化 OTA-fota 服务, 并安装回调函数(需编译设置宏 OTA_ENABLED)
| [linkkit_invoke_fota_service](#linkkit_invoke_fota_service)                         | 执行fota服务
| [linkkit_cota_init](#linkkit_cota_init)                                             | 初始化 OTA-cota 服务, 并安装回调函数(需编译设置宏 OTA_ENABLED)
| [linkkit_invoke_cota_get_config](#linkkit_invoke_cota_get_config)                   | 设备请求远程配置
| [linkkit_invoke_cota_service](#linkkit_invoke_cota_service)                         | 执行cota服务
| [linkkit_post_property](#linkkit_post_property)                                     | 上报设备属性到云端
| [linkkit_set_opt](#linkkit_set_opt)                                                 | 设置设备属性和服务上报参数
| [linkkit_try_leave](#linkkit_try_leave)                                             | 设置linkkit离开标志
| [linkkit_is_try_leave](#linkkit_is_try_leave)                                       | 获取linkkit离开标志
| [linkkit_is_end](#linkkit_is_end)                                                   | 获取linkkit结束标志
| [linkkit_trigger_extended_info_operate](#linkkit_trigger_extended_info_operate)     | 设备拓展信息上报或删除

*[回到目录](#目录)*
### <a name="linkkit_gateway网关相关">linkkit_gateway网关相关</a>
| 函数名                          | 说明
|---------------------------------|---------------------------------------------------------------------------------
| [linkkit_gateway_get_default_params](#linkkit_gateway_get_default_params)               | 获取默认的网关配置参数
| [linkkit_gateway_setopt](#linkkit_gateway_setopt)                                       | 修改网关配置参数
| [linkkit_gateway_set_event_callback](#linkkit_gateway_set_event_callback)               | 注册网关事件回调函数, 加载用户数据
| [linkkit_gateway_init](#linkkit_gateway_init)                                           | 网关初始化,
| [linkkit_gateway_exit](#linkkit_gateway_exit)                                           | 网关反初始化
| [linkkit_gateway_start](#linkkit_gateway_start)                                         | 启动网关服务, 与云端服务器建立连接
| [linkkit_gateway_stop](#linkkit_gateway_stop)                                           | 停止网关服务, 与云端服务器断开连接
| [linkkit_gateway_subdev_register](#linkkit_gateway_subdev_register)                     | 向云端注册productKey/deviceName指定的子设备, 并将子设备加入网关的拓扑关系
| [linkkit_gateway_subdev_unregister](#linkkit_gateway_subdev_unregister)                 | 向云端注销productKey/deviceName指定的子设备, 并将子设备从网关的拓扑关系移除
| [linkkit_gateway_subdev_create](#linkkit_gateway_subdev_create)                         | 创建子设备, 并注册用户回调函数, 载入用户数据
| [linkkit_gateway_subdev_destroy](#linkkit_gateway_subdev_destroy)                       | 删除子设备, 回收资源
| [linkkit_gateway_subdev_login](#linkkit_gateway_subdev_login)                           | 子设备上线, 云端将可以访问子设备
| [linkkit_gateway_subdev_logout](#linkkit_gateway_subdev_logout)                         | 子设备下线, 云端将无法访问子设备
| [linkkit_gateway_get_devinfo](#linkkit_gateway_get_devinfo)                             | 获取设备信息
| [linkkit_gateway_trigger_event_json_sync](#linkkit_gateway_trigger_event_json_sync)     | 上报网关或子设备事件, 同步接口
| [linkkit_gateway_trigger_event_json](#linkkit_gateway_trigger_event_json)               | 上报网关或子设备事件, 异步接口, 上报处理结束将调用用户回调函数
| [linkkit_gateway_post_property_json_sync](#linkkit_gateway_post_property_json_sync)     | 上报网关或子设备属性, 同步接口
| [linkkit_gateway_post_property_json](#linkkit_gateway_post_property_json)               | 上报网关或子设备属性, 异步接口, 上报处理结束将调用用户回调函数
| [linkkit_gateway_post_rawdata](#linkkit_gateway_post_rawdata)                           | 上报网关或子设备的裸数据
| [linkkit_gateway_fota_init](#linkkit_gateway_fota_init)                                 | FOTA服务初始化, 并注册回调函数
| [linkkit_gateway_invoke_fota_service](#linkkit_gateway_invoke_fota_service)             | 执行FOTA服务
| [linkkit_gateway_post_extinfos](#linkkit_gateway_post_extinfos)                         | 上报拓展信息, 拓展信息在控制台上以标签信息呈现
| [linkkit_gateway_delete_extinfos](#linkkit_gateway_delete_extinfos)                     | 删除拓展信息,
| [linkkit_gateway_get_num_devices](#linkkit_gateway_get_num_devices)                     | 获取注册到SDK的设备总数, 包括网关和所有子设备

网关API详细调用方法请查看`examples/linkkit/linkkit_example_gateway.c`


*[回到目录](#目录)*
## <a name="6.3 新版API接口详解">6.3 新版API接口详解</a>
*[回到目录](#目录)*
### <a name="IOT_OpenLog">IOT_OpenLog</a>

原型
---
```
void IOT_OpenLog(const char *ident);
```

接口说明
---
日志系统的初始化函数, 本接口被调用后, SDK才有可能向终端打印日志文本, 但打印的文本详细程度还是由 `IOT_SetLogLevel()` 确定, 默认情况下, 无日志输出

参数说明
---
| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------
| ident   | const char *    | 输入      | 日志的标识符字符串, 例如: `IOT_OpenLog("linkkit")`

返回值说明
---
无返回值

-----

*[回到目录](#目录)*
### <a name="IOT_CloseLog">IOT_CloseLog</a>

原型
---
```
void IOT_CloseLog(void);
```

接口说明
---
日志系统的销毁函数, 本接口被调用后, SDK停止向终端打印任何日志文本, 但之后可以由调用 `IOT_OpenLog()` 接口重新使能日志输出

关闭和重新使能日志系统之后, 需要重新调用 `IOT_SetLogLevel()` 接口设置日志级别, 否则日志系统虽然使能了, 也不会输出文本

参数说明
---
无参数

返回值说明
---
无返回值

-----

*[回到目录](#目录)*
### <a name="IOT_SetLogLevel">IOT_SetLogLevel</a>

原型
---
```
void IOT_SetLogLevel(IOT_LogLevel level);
```

接口说明
---
日志系统的日志级别配置函数, 本接口用于设置SDK的日志显示级别, 需要 `IOT_OpenLog()` 后被调用

参数说明
---
| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------
| level   | IOT_LogLevel    | 输入      | 需要显示的日志级别

返回值说明
---
无返回值

参数的数据结构说明说明
---
```
typedef enum _IOT_LogLevel {
    IOT_LOG_EMERG = 0,
    IOT_LOG_CRIT,
    IOT_LOG_ERROR,
    IOT_LOG_WARNING,
    IOT_LOG_INFO,
    IOT_LOG_DEBUG,
} IOT_LogLevel;
```

-----

*[回到目录](#目录)*
### <a name="IOT_DumpMemoryStats">IOT_DumpMemoryStats</a>

原型
---
```
void IOT_DumpMemoryStats(IOT_LogLevel level);
```

接口说明
---
该接口可显示出SDK各模块的内存使用情况, 当WITH_MEM_STATS=1时起效, 显示级别设置得越高, 显示的信息越多

参数说明
---
| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------
| level   | IOT_LogLevel    | 输入      | 需要显示的日志级别

返回值说明
---
无返回值

-----

*[回到目录](#目录)*
### <a name="IOT_SetupConnInfo">IOT_SetupConnInfo</a>

原型
---
```
int IOT_SetupConnInfo(const char *product_key,
                        const char *device_name,
                        const char *device_secret,
                        void **info_ptr);
```

接口说明
---
在连接云端之前, 需要做一些认证流程, 如一型一密获取DeviceSecret或者根据当前所选认证模式向云端进行认证

该接口在SDK基础版中需要在连接云端之前由用户显式调用；而在高级版中SDK会自动进行调用, 不需要用户显式调用

参数说明
---
| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------
| product_key     | const char *    | 输入      | 设备三元组的ProductKey
| device_name     | const char *    | 输入      | 设备三元组的DeviceName
| device_secret   | const char *    | 输入      | 设备三元组的DeviceSecret
| info_ptr        | void **         | 输出      | 该void**数据类型为iotx_conn_info_t, 在认证流程通过后, 会得到云端的相关信息, 用于建立与云端连接时使用

返回值说明
---
| 值 | 说明
|--------|----
| 0       | 成功
| < 0     | 失败

参数的数据结构说明
---
```
typedef struct {
    uint16_t        port;
    char            host_name[HOST_ADDRESS_LEN + 1];
    char            client_id[CLIENT_ID_LEN + 1];
    char            username[USER_NAME_LEN + 1];
    char            password[PASSWORD_LEN + 1];
    const char     *pub_key;
} iotx_conn_info_t, *iotx_conn_info_pt;
```

-----
*[回到目录](#目录)*
### <a name="IOT_Ioctl">IOT_Ioctl</a>

原型
---
```
int IOT_Ioctl(int option, void *data);
```
接口说明
---
在SDK连接云端之前, 用户可用此接口进行SDK部分参数的配置或获取, 如连接的region, 是否使用一型一密等

该接口在基础版和高级版中均适用, 需要注意的是, 该接口需要在SDK建立网络连接之前调用

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------
| option  | iotx_ioctl_option_t     | 输入          | 选择需要配置或获取的参数
| data    | void *                  | 输入/输出       | 在配置或获取参数时需要的buffer, 依据`option`而定

返回值说明
---
| 值 | 说明
|--------|----
| 0       | 成功
| < 0     | 失败

参数的数据结构说明
---
```
typedef enum {
    IOTX_IOCTL_SET_DOMAIN,              /* value(int*): iotx_cloud_domain_types_t */
    IOTX_IOCTL_GET_DOMAIN,              /* value(int*) */
    IOTX_IOCTL_SET_DYNAMIC_REGISTER,    /* value(int*): 0 - Disable Dynamic Register, 1 - Enable Dynamic Register */
    IOTX_IOCTL_GET_DYNAMIC_REGISTER     /* value(int*) */
} iotx_ioctl_option_t;
```
+ `IOTX_IOCTL_SET_DOMAIN`: 设置需要访问的的region, `data`为`int *`类型, 取值如下:
    - IOTX_CLOUD_DOMAIN_SH, 华东2(上海)
    - IOTX_CLOUD_DOMAIN_SG, 新加坡
    - IOTX_CLOUD_DOMAIN_JP, 日本(东京)
    - IOTX_CLOUD_DOMAIN_US, 美国(硅谷)
    - IOTX_CLOUD_DOMAIN_GER, 德国(法兰克福)

+ `IOTX_IOCTL_GET_DOMAIN`: 获取当前的region, `data`为`int *`类型

+ `IOTX_IOCTL_SET_DYNAMIC_REGISTER`: 设置是否需要直连设备动态注册(一型一密), `data`为`int *`类型, 取值如下：
    - 0, 不使用直连设备动态注册
    - 1, 使用直连设备动态注册

+ `IOTX_IOCTL_GET_DYNAMIC_REGISTER`: 获取当前设备注册方式, `data`为`int *`类型

-----

*[回到目录](#目录)*
### <a name="IOT_CoAP_Init">IOT_CoAP_Init</a>

原型
---
```
iotx_coap_context_t *IOT_CoAP_Init(iotx_coap_config_t *p_config);
```

接口说明
---
CoAP模块初始化函数, 在使用CoAP的功能之前, 需要使用该函数进行初始化

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------
| p_config    | iotx_coap_config_t *    | 输入      | CoAP模块初始化参数

返回值说明
---
|数据类型 |说明
|--------|----
| iotx_coap_context_t *   | CoAP当前实例的Context

参数的数据结构说明
---
```
typedef struct {
    char                  *p_url;
    int                   wait_time_ms;
    iotx_device_info_t    *p_devinfo;
    iotx_event_handle_t   event_handle; /* not supported now */
} iotx_coap_config_t;
```
+ `p_url`: 需要连接的云端服务器地址
    - 使用DTLS：`coaps://%s.iot-as-coap.cn-shanghai.aliyuncs.com:5684`, `%s` 为ProductKey
    - 使用PSK：`coap-psk://%s.iot-as-coap.cn-shanghai.aliyuncs.com:5683`, `%s` 为ProductKey

+ `wait_time_ms`: CoAP收发消息的超时时间

+ `p_devinfo`: 设备信息, 包含 Product_Key/ProductSecret/DeviceName 和 DeviceSecret

-----

*[回到目录](#目录)*
### <a name="IOT_CoAP_Deinit">IOT_CoAP_Deinit</a>

原型
---
```
void IOT_CoAP_Deinit(iotx_coap_context_t **pp_context);
```

接口说明
---
CoAP反初始化函数, 断开与云端的连接并释放所有指定Context中分配的资源

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| pp_context  | iotx_coap_context_t **  | 输入      | 需要反初始化的CoAP Context

返回值说明
---
无返回值

-----

*[回到目录](#目录)*
### <a name="IOT_CoAP_DeviceNameAuth">IOT_CoAP_DeviceNameAuth</a>

原型
---
```
int IOT_CoAP_DeviceNameAuth(iotx_coap_context_t *p_context)
```

接口说明
---
向云端发送设备认证请求, 认证通过后才能通过CoAP与云端正常通信

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| pp_context  | iotx_coap_context_t **  | 输入      | CoAP Context

返回值说明
---
| 值 | 说明
|--------|----
| 0   | 成功
| -1  | 输入参数非法
| -2  | 内存不足
| -4  | 认证失败
| -8  | CoAP消息发送失败

-----

*[回到目录](#目录)*
### <a name="IOT_CoAP_Yield">IOT_CoAP_Yield</a>

原型
---
```
int IOT_CoAP_Yield(iotx_coap_context_t *p_context)
```

接口说明
---
当CoAP连接云端后, 用于尝试从网络上接收报文

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| p_context   | iotx_coap_context_t *   | 输入      | CoAP Context

返回值说明
---
| 值 | 说明
|--------|----
| 0       | 成功
| < 0     | 失败

-----

*[回到目录](#目录)*
### <a name="IOT_CoAP_SendMessage">IOT_CoAP_SendMessage</a>

原型
---
```
int IOT_CoAP_SendMessage(iotx_coap_context_t *p_context, char *p_path, iotx_message_t *p_message);
```

接口说明
---
当CoAP连接云端后, 用于向云端发送CoAP消息

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| p_context   | iotx_coap_context_t *   | 输入      | CoAP Context
| p_path      | char *                  | 输入      | 发送消息的目标资源地址
| p_message   | iotx_message_t *        | 输入      | 待发送消息

返回值说明
---

| 值 | 说明
|--------|----
| 0   | 成功
| -1  | 输入参数非法
| -5  | 设备尚未认证成功
| -7  | 待发送消息的payload过长

-----

参数的数据结构说明
---
```
typedef struct {
    unsigned char            *p_payload;
    unsigned short           payload_len;
    iotx_content_type_t      content_type;
    iotx_msg_type_t          msg_type;
    void                     *user_data;
    iotx_response_callback_t resp_callback;
} iotx_message_t;
```

+ `p_payload`: 需要发送的数据内容
+ `payload_len`: 需要发送的数据长度
+ `content_type`: 数据的格式
+ `iotx_msg_type_t`: CoAP消息类型(是否需要Confirmx消息)
+ `user_data`: 用户数据, 在收到应答后, 会送回给用户
+ `resp_callback`: 用户注册的回调函数, 当收到该消息的应答时被调用

*[回到目录](#目录)*
### <a name="IOT_CoAP_GetMessagePayload">IOT_CoAP_GetMessagePayload</a>

原型
---
```
int IOT_CoAP_GetMessagePayload(void *p_message, unsigned char **pp_payload, int *p_len);
```

接口说明
---
当用户通 [#IOT_CoAP_SendMessage](#IOT_CoAP_SendMessage) 送消息并收到应答时, 用该接口可获取CoAP消息中的Payload部分

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| p_message   | void *                  | 输入      | CoAP应答消息句柄, 在应答消息的回调函数中可获得
| pp_payload  | unsigned char ** **     | 输出      | 用于存放从`p_message`中获取的消息Payload
| p_len       | int *                   | 输出      | `pp_payload`的长度

返回值说明
---

| 值 | 说明
|--------|----
| 0   | 成功
| -1  | 输入参数非法
| -2  | 内存不足

-----

*[回到目录](#目录)*
### <a name="IOT_CoAP_GetMessageCode">IOT_CoAP_GetMessageCode</a>

原型
---
```
int IOT_CoAP_GetMessageCode(void *p_message, iotx_coap_resp_code_t *p_resp_code);
```

接口说明
---
当用户通 [#IOT_CoAP_SendMessage](#IOT_CoAP_SendMessage) 送消息并收到应答时, 用该接口可获取CoAP消息中的Code(错误码)部分

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| p_message   | void *                  | 输入      | CoAP应答消息句柄, 在应答消息的回调函数中可获得
| pp_payload  | unsigned char ** **     | 输出      | 用于存放从`p_message`中获取的消息Code

返回值说明
---

| 值 | 说明
|--------|----
| 0   | 成功
| -1  | 输入参数非法

参数的数据格式
---
```
typedef enum {
    IOTX_COAP_RESP_CODE_CONTENT        = 0x45, /* Mapping to 2.05, Content*/
    IOTX_COAP_RESP_CODE_BAD_REQUEST    = 0x80, /* Mapping to 4.00, Bad Request*/
    IOTX_COAP_RESP_CODE_UNAUTHORIZED   = 0x81, /* Mapping to 4.01, Token is invalid or expire*/
    IOTX_COAP_RESP_CODE_NOT_FOUND      = 0x84, /* Mapping to 4.04, Path or uri is not found*/
    IOTX_COAP_RESP_CODE_URL_TOO_LONG   = 0x8E, /* Mapping to 4.14, The request url is too long*/
    IOTX_COAP_RESP_CODE_INTERNAL_SERVER_ERROR = 0xA0,/* Mapping to 5.00, Internal server error*/

} iotx_coap_resp_code_t;
```

以上错误码可参考CoAP协议

-----

*[回到目录](#目录)*
### <a name="IOT_HTTP_Init">IOT_HTTP_Init</a>

原型
---
```
void *IOT_HTTP_Init(iotx_http_param_t *pInitParams);
```

接口说明
---
HTTP模块初始化函数, 在使用HTTP的功能之前, 需要使用该函数进行初始化

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| pInitParams     | iotx_http_param_t *     | 输入      | HTTP模块初始化参数

返回值说明
---
| 值 | 说明
|---|---
| NULL    | 初始化失败
| 非NULL   | HTTP Context

参数的数据结构说明
```
typedef struct {
    iotx_device_info_t *device_info;
    int                 keep_alive;
    int                 timeout_ms;
} iotx_http_param_t;
```
+ `device_info`: 设备信息, 包含Product_Key/ProductSecret/DeviceName和DeviceSecret
+ `keep_alive`: 选择是否采用http的keep alive模式, 即每次与云端通信完成后是否需要断开http连接
+ `timeout_ms:`: 设置等待应答消息的超时时间

-----

*[回到目录](#目录)*
### <a name="IOT_HTTP_DeInit">IOT_HTTP_DeInit</a>

原型
---
```
void IOT_HTTP_DeInit(void **handle);
```

接口说明
---
HTTP反初始化函数, 断开与云端的连接并释放所有指定Context中分配的资源

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| handle  | void *  | 输入      | HTTP Context

返回值说明
---
无返回值

-----

*[回到目录](#目录)*
### <a name="IOT_HTTP_DeviceNameAuth">IOT_HTTP_DeviceNameAuth</a>

原型
```
int IOT_HTTP_DeviceNameAuth(void *handle);
```

接口说明
---
向云端发送设备认证请求, 认证通过后才能通过HTTP与云端正常通信

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| handle  | void *  | 输入      | HTTP Context

返回值说明
---
| 值 | 说明
|---|---
| 0       | 成功
| < 0     | 失败

-----

*[回到目录](#目录)*
### <a name="IOT_HTTP_SendMessage">IOT_HTTP_SendMessage</a>

原型
---
```
int IOT_HTTP_SendMessage(void *handle, iotx_http_message_param_t *msg_param);
```

接口说明
---
当HTTP连接云端后, 用于向云端发送HTTP消息

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| handle      | void *                          | 输入      | HTTP Context
| msg_param   | iotx_http_message_param_t *     | 输入      | 待发送到云端的消息

返回值说明
---
| 值 | 说明
|---|---
| 0       | 成功
| < 0     | 失败

参数的数据结构说明
---
```
typedef struct {
    char       *topic_path;
    uint32_t   request_payload_len;
    char       *request_payload;
    uint32_t   response_payload_len;
    char       *response_payload;
    uint32_t   timeout_ms;
} iotx_http_message_param_t;
```
+ `topic_path`: 待发送消息的目标资源地址

+ `request_payload_len`: 待发送消息的长度

+ `request_payload`: 待发送消息的数据

+ `response_payload_len`: 应答消息buffer长度

+ `response_payload`: 应答消息buffer

+ `timeout_ms`: 等待应答消息的超时时间

-----

*[回到目录](#目录)*
### <a name="IOT_HTTP_Disconnect">IOT_HTTP_Disconnect</a>

原型
---
```
void IOT_HTTP_Disconnect(void *handle)
```

接口说明
---
该接口用于断开指定HTTP Context的连接

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| handle  | void *  | 输入      | HTTP Context

返回值说明
---
无返回值

-----

*[回到目录](#目录)*
### <a name="IOT_Linkkit_Open">IOT_Linkkit_Open</a>

原型
---
```
int IOT_Linkkit_Open(iotx_linkkit_dev_type_t dev_type, iotx_linkkit_dev_meta_info_t *meta_info);
```

接口说明
---
初始化设备资源, 在对设备进行操作之前, 必须先调用此接口

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| dev_type    | iotx_linkkit_dev_type_t         | 输入      | 需要创建资源的设备类型
| meta_info   | iotx_linkkit_dev_meta_info_t    | 输入      | 设备的四元组信息

返回值说明
---
| 值 | 说明
|---|---
| 0       | 创建主设备成功
| > 0     | 创建子设备成功
| < 0     | 失败

参数的数据结构说明
---
```
typedef enum {
    IOTX_LINKKIT_DEV_TYPE_MASTER,
    IOTX_LINKKIT_DEV_TYPE_SLAVE,
    IOTX_LINKKIT_DEV_TYPE_MAX
} iotx_linkkit_dev_type_t;
```

+ IOTX_LINKKIT_DEV_TYPE_MASTER: 创建的设备为主设备, 仅能创建一次
+ IOTX_LINKKIT_DEV_TYPE_SLAVE: 创建的设备为子设备

```
typedef struct {
    char product_key[PRODUCT_KEY_MAXLEN];
    char product_secret[PRODUCT_SECRET_MAXLEN];
    char device_name[DEVICE_NAME_MAXLEN];
    char device_secret[DEVICE_SECRET_MAXLEN];
} iotx_linkkit_dev_meta_info_t;
```

+ product_key: 最大长度为20字节
+ product_secret: 最大长度为64字节
+ device_name: 最大长度为32字节
+ device_secret: 最大长度为64字节

-----

*[回到目录](#目录)*
### <a name="IOT_Linkkit_Ioctl">IOT_Linkkit_Ioctl</a>

原型
---
```
int IOT_Linkkit_Ioctl(int devid, iotx_linkkit_ioctl_cmd_t cmd, void *arg);
```

接口说明
---
在使 [#IOT_Linkkit_Open](#IOT_Linkkit_Open) 建设备后, 可使用该接口对设备参数进行调整

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| devid   | int                         | 输入          | 设备ID
| cmd     | iotx_linkkit_ioctl_cmd_t    | 输入          | 需要执行的命令类型
| arg     | void *                      | 输入/输出       | `cmd`命令的参数

返回值说明
---
| 值 | 说明
|----|---
| 0       | 成功
| < 0     | 失败

参数的数据结构说明
---
```
typedef enum {
    IOTX_LINKKIT_CMD_OPTION_PROPERTY_POST_REPLY,           
    IOTX_LINKKIT_CMD_OPTION_EVENT_POST_REPLY,              
    IOTX_LINKKIT_CMD_OPTION_PROPERTY_SET_REPLY,            
    IOTX_LINKKIT_CMD_MAX
} iotx_linkkit_ioctl_cmd_t;
```
+ IOTX_LINKKIT_CMD_OPTION_PROPERTY_POST_REPLY: 在设备上报属性后, 是否需要云端的应答消息. `arg`数据类型为`int *`
+ IOTX_LINKKIT_CMD_OPTION_EVENT_POST_REPLY: 在设备上报事件后, 是否需要云端的应答消息. `arg`数据类型为`int *`
+ IOTX_LINKKIT_CMD_OPTION_PROPERTY_SET_REPLY: 在收到云端的属性设置后, 是否需要向云端发送应答消息. `arg`数据类型为`int *`

-----

*[回到目录](#目录)*
### <a name="IOT_Linkkit_Connect">IOT_Linkkit_Connect</a>

原型
---
```
int IOT_Linkkit_Connect(int devid, iotx_linkkit_event_handler_t *hdlrs);
```

接口说明
---
对于主设备来说, 将会建立设备与云端的通信；对于子设备来说, 将向云端注册该子设备(如果需要的话), 并添加主子设备拓扑关系

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| devid   | int                             | 输入      | 设备ID
| hdlrs   | iotx_linkkit_event_handler_t *  | 输出      | 用户回调函数(当设备为主设备时有效)

返回值说明
---
| 值 | 说明
|---|---
| 0       | 成功
| < 0     | 失败

参数的数据结构说明
---
```
typedef struct {
    int (* connected)(void);
    int (* disconnected)(void);
    int (* down_raw)(const int devid, const unsigned char *payload, const int payload_len);
    int (* up_raw_reply)(const int devid, const unsigned char *payload, const int payload_len);
    int (* async_service_request)(const int devid, const char *serviceid, const int serviceid_len, const char *request, const int request_len, char **response, int *response_len);
    int (* sync_service_request)(const int devid, const char *serviceid, const int serviceid_len, const char *request, const int request_len, char **response, int *response_len);
    int (* property_set)(const int devid, const char *request, const int request_len);
    int (* property_get)(const int devid, const char *request, const int request_len, char **response, int *response_len);
    int (* post_reply)(const int devid, const int msgid, const int code, const char *reply, const int reply_len);
    int (* ntp_response)(const char *utc);
    int (* permit_join)(void);
    int (* initialized)(const int devid);
} iotx_linkkit_event_handler_t;
```
+ int (* connected)(void);
    - 当设备与云端建立连接后, 会触发此回调函数
+ int (* disconnected)(void);
    - 当设备与云端断开连接后, 会触发此回调函数
+ int (* down_raw)(const int devid, const unsigned char *payload, const int payload_len);
    - 当设备收到透传数据下发时, 会触发此回调
    - `devid`: 设备ID
    - `payload`: 透传数据buffer
    - `paylaod_len`: 透传数据buffer长度
+ int (* up_raw_reply)(const int devid, const unsigned char *payload, const int payload_len);
    - 当用户向云端发送透传数据后, 收到透传数据应答会触发此回调函数
    - `devid`: 设备ID
    - `payload`: 透传数据buffer
    - `payload_len`: 透传数据buffer长度
+ int (* async_service_request)(const int devid, const char *serviceid, const int serviceid_len, const char *request, const int request_len, char **response, int *response_len);
    - 当设备收到异步服务调用请求时, 会触发此回调函数
    - `devid`: 设备ID
    - `serviceid`: TSL异步服务ID, 由用户在云端控制台生成
    - `serviceid_len`: 异步服务ID的长度
    - `request`: 异步服务的输入参数, 由用户在云端控制台生成
    - `request_len`: 异步服务的输入参数长度
    - `response`: 用户需要回复云端的异步服务输出参数. 如果有输出参数需要回复给云端, 则用户应使用`HAL_Malloc`为`*response`分配空间用于存放输出参数, SDK在使用该buffer结束后, 会自动释放这块内存, 用户无需释放；如果没有输出参数需要回复给云端, 忽略此参数
    - `response_len`: 异步服务输出参数长度. 如果没有输出参数需要回复给云端, 忽略此参数
+ int (* sync_service_request)(const int devid, const char *serviceid, const int serviceid_len, const char *request, const int request_len, char **response, int *response_len);
    - 当设备收到同步服务调用请求时, 会触发此回调函数
    - `devid`: 设备ID
    - `serviceid`: TSL同步服务ID, 由用户在云端控制台生成
    - `serviceid_len`: 同步服务ID的长度
    - `request_len`: 同步服务的输入参数长度
    - `response`: 用户需要回复云端的同步服务输出参数. 如果有输出参数需要回复给云端, 则用户应使用`HAL_Malloc`为`*response`分配空间用于存放输出参数, SDK在使用该buffer结束后, 会自动释放这块内存, 用户无需释放；如果没有输出参数需要回复给云端, 忽略此参数
    - `response_len`: 同步服务输出参数长度. 如果没有输出参数需要回复给云端, 忽略此参数
+ int (* property_set)(const int devid, const char *request, const int request_len);
    - 当设备收到属性设置请求时, 会触发此回调函数
    - `devid`: 设备ID
    - `request`: 属性设置请求消息, 为JSON数组格式, 数组含若干JSON对象, 每个JSON对象的Key为属性ID, Value为属性的值
    - `request_len`: 属性设置请求消息的长度
    - 需要注意的是, 云端下发该请求后, 在云端保存的相关属性值并不会改变, 除非用户显示调用 [#IOT_Linkkit_Post](#IOT_Linkkit_Post) 上报相关属性值
+ int (* property_get)(const int devid, const char *request, const int request_len, char **response, int *response_len);
    - 当设备收到获取属性请求时, 会触发此回调函数
    - `devid`: 设备ID
    - `request`: 获取属性值的请求消息, 为JSON数组格式, 数组含若干项JSON字符串, 每个字符串为需要获取值的属性ID
    - `request_len`: 获取属性值的请求消息长度
    - `response`: 用户需要回复的属性值消息. 如果属性值可以回复, 则用户应使用`HAL_Malloc`为`*response`分配空间用于存放属性值, SDK在使用该buffer结束后, 会自动释放这块内存, 用户无需释放；如果没有属性值可以回复, 忽略此参数
    - `response_len`: 回复的属性值消息长度
+ int (* post_reply)(const int devid, const int msgid, const int code, const char *reply, const int reply_len);
    - 当用户使用 [#IOT_Linkkit_Post](#IOT_Linkkit_Post) 或 [#IOT_Linkkit_TriggerEvent](#IOT_Linkkit_TriggerEvent) 发送消息后, 收到应答时, 会触发此回调函数
    - `devid`: 设备ID
    - `msgid`: 应答消息的ID, 与发送消息时产生的ID一致, 可用于消息匹配
    - `code`: 云端返回的错误码, 可参考`云端接口说明`文档
    - `reply`: 应答的payload, 可能为NULL
    - `reply_len`: 应答的payload长度
+ int (* ntp_response)(const char *utc);
    - 当设备连接云端成功后, SDK会向云端请求UTC时间戳, 在SDK获取到时间戳后, 会触发此回调
    - `utc`: 时间戳的值, 为字符串类型
+ int (* initialized)(const int devid);
    - 当设备完成初始化时, 会触发此回调
    - `devid`: 设备ID

-----

*[回到目录](#目录)*
### <a name="IOT_Linkkit_Yield">IOT_Linkkit_Yield</a>

原型
---
```
void IOT_Linkkit_Yield(int timeout_ms);
```

接口说明
---
用于接收网络报文并将消息分发到用户的回调函数中

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| timeout_ms  | int     | 输入      | 单线程模式下, 每次尝试接收网络报文的超时时间

返回值说明
---
无返回值

-----

*[回到目录](#目录)*
### <a name="IOT_Linkkit_Close">IOT_Linkkit_Close</a>

原型
---
```
int IOT_Linkkit_Close(int devid);
```

接口说明
---
若设备ID为主设备, 则关闭网络连接并释放Linkkit所有占用资源

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| devid   | int     | 输入      | 设备ID

返回值说明
---
| 值 | 说明
|---|---
| 0       | 成功
| < 0     | 失败

-----

*[回到目录](#目录)*
### <a name="IOT_Linkkit_Post">IOT_Linkkit_Post</a>

原型
---
```
int IOT_Linkkit_Post(int devid, 
                        iotx_linkkit_msg_type_t msg_type, 
                        unsigned char *payload, 
                        int payload_len);
```

接口说明
---
向云端发送消息, 包括属性上报/设备标签信息更新上报/设备标签信息删除上报/透传数据上报

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| devid           | int                         | 输入      | 设备ID
| msg_type        | iotx_linkkit_msg_type_t     | 输入      | 需要上报的消息类型
| payload         | unsigned char *             | 输入      | 消息Payload
| payload_len     | int                         | 输入      | 消息Payload的长度

返回值说明
---
| 值 | 说明
|---|---
| 0       | 成功
| >= 1    | 消息ID
| < 0     | 失败

参数的数据结构
---
```
typedef enum {
    IOTX_LINKKIT_MSG_POST_PROPERTY,
    IOTX_LINKKIT_MSG_DEVICEINFO_UPDATE,
    IOTX_LINKKIT_MSG_DEVICEINFO_DELETE,
    IOTX_LINKKIT_MSG_POST_RAW_DATA,
    IOTX_LINKKIT_MSG_MAX
} iotx_linkkit_msg_type_t;
```
+ IOTX_LINKKIT_MSG_POST_PROPERTY: 设备属性数据上报
+ IOTX_LINKKIT_MSG_DEVICEINFO_UPDATE: 设备标签更新信息上报
+ IOTX_LINKKIT_MSG_DEVICEINFO_DELETE: 设备标签删除信息上报
+ IOTX_LINKKIT_MSG_POST_RAW_DATA: 设备透传数据上报

-----

*[回到目录](#目录)*
### <a name="IOT_Linkkit_TriggerEvent">IOT_Linkkit_TriggerEvent</a>

原型
---
```
int IOT_Linkkit_TriggerEvent(int devid, char *eventid, int eventid_len, char *payload, int payload_len);
```

接口说明
---
向云端上报设备事件

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| devid           | int     | 输入      | 设备ID
| eventid         | char *  | 输入      | TSL中定义的事件ID
| eventid_len     | int     | 输入      | 事件ID的长度
| payload         | char *  | 输入      | 事件Payload
| payload_len     | int     | 输入      | 事件Payload的长度

返回值说明
---
| 值 | 说明
|---|---
| >= 1    | 消息ID
| < 0     | 失败

-----

*[回到目录](#目录)*
### <a name="IOT_MQTT_Construct">IOT_MQTT_Construct</a>

原型
---
```
void *IOT_MQTT_Construct(iotx_mqtt_param_t *pInitParams)
```

接口说明
---
与云端建立MQTT连接, 在调用该接口连接云端前应先使用 [#IOT_SetupConnInfo](#IOT_SetupConnInfo) 向云端进行设备认证, 得到连接云端所需参数

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| pInitParams     | iotx_mqtt_param_t *     | 输入      | MQTT初始化参数

返回值说明
---
| 值 | 说明
|---|---
| NULL    | 失败
| 非NULL   | MQTT Context

参数的数据结构说明
---
```
typedef struct {
    uint16_t                   port;
    const char                 *host;
    const char                 *client_id;
    const char                 *username;
    const char                 *password;
    const char                 *pub_key;
    uint8_t                    clean_session;
    uint32_t                   request_timeout_ms;
    uint32_t                   keepalive_interval_ms;
    char                       *pwrite_buf;
    uint32_t                   write_buf_size;
    char                       *pread_buf;
    uint32_t                   read_buf_size;
    iotx_mqtt_event_handle_t    handle_event;
} iotx_mqtt_param_t, *iotx_mqtt_param_pt;
```

+ `port`: 云端服务器端口, 可通 [IOT_SetupConnInfo](#IOT_SetupConnInfo) 输出参数得到
+ `host`: 云端服务器地址, 可通 [IOT_SetupConnInfo](#IOT_SetupConnInfo) 输出参数得到
+ `client_id`: MQTT客户端ID, 可通 [IOT_SetupConnInfo](#IOT_SetupConnInfo) 输出参数得到
+ `username`: 登录MQTT服务器用户名, 可通 [IOT_SetupConnInfo](#IOT_SetupConnInfo) 输出参数得到
+ `password`: 登录MQTT服务器密码, 可通 [IOT_SetupConnInfo](#IOT_SetupConnInfo) 输出参数得到
+ `pub_key`: MQTT连接加密方式及密钥, 可通 [IOT_SetupConnInfo](#IOT_SetupConnInfo) 输出参数得到
+ `clean_session`: 选择是否使用MQTT协议的clean session特性
+ `request_timeout_ms`: MQTT消息发送的超时时间
+ `keepalive_interval_ms`: MQTT心跳超时时间
+ `pwrite_buf`: MQTT消息发送buffer
+ `write_buf_size`: MQTT消息发送buffer最大长度
+ `pread_buf`: MQTT消息接收buffer
+ `read_buf_size`: MQTT消息接收buffer最大长度
+ `handle_event`: 用户回调函数, 用与接收MQTT模块的事件信息

-----

*[回到目录](#目录)*
### <a name="IOT_MQTT_Destroy">IOT_MQTT_Destroy</a>

原型
---
```
int IOT_MQTT_Destroy(void **phandle);
```

接口说明
---
销毁指定MQTT连接并释放资源

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| phandle     | void **     | 输入      | MQTT Context

返回值说明
---
| 值 | 说明
|---|---
| 0       | 成功
| < 0     | 失败

-----

*[回到目录](#目录)*
### <a name="IOT_MQTT_Yield">IOT_MQTT_Yield</a>

原型
---
```
int IOT_MQTT_Yield(void *handle, int timeout_ms);
```

接口说明
---
用于接收网络报文并将消息分发到用户的回调函数中

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| handle      | void *  | 输入      | MQTT Context
| timeout_ms  | int     | 输入      | 尝试接收报文的超时时间

返回值说明
---
| 值 | 说明
|---|---
| 0   | 成功

-----

*[回到目录](#目录)*
### <a name="IOT_MQTT_LogPost">IOT_MQTT_LogPost</a>

原型
---
```
int IOT_MQTT_LogPost(void *pHandle, const char *level, const char *module, const char *msg);
```

接口说明
---
上报重要日志到云端保存

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| pHandle     | void *          | 输入      | MQTT Context
| level       | const char *    | 输入      | 日志级别信息, 由用户自定义填写, 在云端控制台可见
| module      | const char *    | 输入      | 使用该日志接口的模块名, 由用户自定义填写, 在云端控制台可见
| msg         | const char *    | 输入      | 需要上传的日志信息

返回值说明
---
| 值 | 说明
|---|---
| 0       | 成功
| < 0     | 失败

-----

*[回到目录](#目录)*
### <a name="IOT_MQTT_CheckStateNormal">IOT_MQTT_CheckStateNormal</a>

原型
---
```
int IOT_MQTT_CheckStateNormal(void *handle);
```

接口说明
---
获取当前MQTT连接状态

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| handle  | void *  | 输入      | MQTT Context

返回值说明
---
| 值 | 说明
|---|---
| 0   | 未连接
| 1   | 已连接

-----

*[回到目录](#目录)*
### <a name="IOT_MQTT_Subscribe">IOT_MQTT_Subscribe</a>

原型
---
```
int IOT_MQTT_Subscribe(void *handle, 
                        const char *topic_filter, 
                        iotx_mqtt_qos_t qos, 
                        iotx_mqtt_event_handle_func_fpt topic_handle_func, 
                        void *pcontext);
```

接口说明
---
向云端订阅指定的MQTT Topic

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| handle              | void *                              | 输入      | MQTT Context
| topic_filter        | const char *                        | 输入      | 需要订阅的topic
| qos                 | iotx_mqtt_qos_t                     | 输入      | 采用的QoS策略
| topic_handle_func   | iotx_mqtt_event_handle_func_fpt     | 输入      | 用于接收MQTT消息的回调函数
| pcontext            | void *                              | 输入      | 用户Context, 会通过回调函数送回

返回值说明
---
| 值 | 说明
|---|---
| 0       | 成功
| < 0     | 失败

-----

*[回到目录](#目录)*
### <a name="IOT_MQTT_Subscribe_Sync">IOT_MQTT_Subscribe_Sync</a>

原型
---
```
int IOT_MQTT_Subscribe_Sync(void *handle,
                            const char *topic_filter,
                            iotx_mqtt_qos_t qos,
                            iotx_mqtt_event_handle_func_fpt topic_handle_func,
                            void *pcontext,
                            int timeout_ms,
                            int do_yield);
```

接口说明
---
向云端订阅指定的MQTT Topic, 该接口为同步接口

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| handle              | void *                              | 输入      | MQTT Context
| topic_filter        | const char *                        | 输入      | 需要订阅的topic
| qos                 | iotx_mqtt_qos_t                     | 输入      | 采用的QoS策略
| topic_handle_func   | iotx_mqtt_event_handle_func_fpt     | 输入      | 用于接收MQTT消息的回调函数
| pcontext            | void *                              | 输入      | 用户Context, 会通过回调函数送回
| timeout_ms          | int                                 | 输入      | 该同步接口的超时时间

返回值说明
---
| 值 | 说明
|---|---
| 0       | 成功
| < 0     | 失败

-----

*[回到目录](#目录)*
### <a name="IOT_MQTT_Unsubscribe">IOT_MQTT_Unsubscribe</a>

原型
---
```
int IOT_MQTT_Unsubscribe(void *handle, const char *topic_filter);
```

接口说明
---
向云端取消订阅指定的topic

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| handle          | void *          | 输入      | MQTT Context
| topic_filter    | const char *    | 输入      | 需要取消订阅的topic

返回值说明
---
| 值 | 说明
|---|---
| 0       | 成功
| < 0     | 失败

-----

*[回到目录](#目录)*
### <a name="IOT_MQTT_Publish">IOT_MQTT_Publish</a>

原型
---
```
int IOT_MQTT_Publish(void *handle, const char *topic_name, iotx_mqtt_topic_info_pt topic_msg);
```

接口说明
---
向指定topic推送消息

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| handle      | void *                      | 输入      | MQTT Context
| topic_name  | const char *                | 输入      | 接收此推送消息的目标topic
| topic_msg   | iotx_mqtt_topic_info_pt     | 输入      | 需要推送的消息

返回值说明
---
| 值 | 说明
|---|---
| 0       | 成功
| < 0     | 失败

-----

*[回到目录](#目录)*
### <a name="IOT_OTA_Init">IOT_OTA_Init</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_OTA_Deinit">IOT_OTA_Deinit</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_OTA_ReportVersion">IOT_OTA_ReportVersion</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_OTA_RequestImage">IOT_OTA_RequestImage</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_OTA_ReportProgress">IOT_OTA_ReportProgress</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_OTA_GetConfig">IOT_OTA_GetConfig</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_OTA_IsFetching">IOT_OTA_IsFetching</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_OTA_IsFetchFinish">IOT_OTA_IsFetchFinish</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_OTA_FetchYield">IOT_OTA_FetchYield</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_OTA_Ioctl">IOT_OTA_Ioctl</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_OTA_GetLastError">IOT_OTA_GetLastError</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Shadow_Construct">IOT_Shadow_Construct</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Shadow_Destroy">IOT_Shadow_Destroy</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Shadow_Yield">IOT_Shadow_Yield</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Shadow_RegisterAttribute">IOT_Shadow_RegisterAttribute</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Shadow_DeleteAttribute">IOT_Shadow_DeleteAttribute</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Shadow_PushFormat_Init">IOT_Shadow_PushFormat_Init</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Shadow_PushFormat_Add">IOT_Shadow_PushFormat_Add</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Shadow_PushFormat_Finalize">IOT_Shadow_PushFormat_Finalize</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Shadow_Push">IOT_Shadow_Push</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Shadow_Push_Async">IOT_Shadow_Push_Async</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Shadow_Pull">IOT_Shadow_Pull</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Gateway_Generate_Message_ID">IOT_Gateway_Generate_Message_ID</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Gateway_Construct">IOT_Gateway_Construct</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Gateway_Destroy">IOT_Gateway_Destroy</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Subdevice_Register">IOT_Subdevice_Register</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Subdevice_Unregister">IOT_Subdevice_Unregister</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Subdevice_Login">IOT_Subdevice_Login</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Subdevice_Logout">IOT_Subdevice_Logout</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Gateway_Get_TOPO">IOT_Gateway_Get_TOPO</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Gateway_Get_Config">IOT_Gateway_Get_Config</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Gateway_Publish_Found_List">IOT_Gateway_Publish_Found_List</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Gateway_Yield">IOT_Gateway_Yield</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Gateway_Subscribe">IOT_Gateway_Subscribe</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Gateway_Unsubscribe">IOT_Gateway_Unsubscribe</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Gateway_Publish">IOT_Gateway_Publish</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Gateway_RRPC_Register">IOT_Gateway_RRPC_Register</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="IOT_Gateway_RRPC_Response">IOT_Gateway_RRPC_Response</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
## <a name="6.4 高级版(旧版)单品API详解">6.4 高级版(旧版)单品API详解</a>

> 本节介绍的API适用于您的设备不具有对子设备的管理能力的场景, 参看 [单品](#单品) 的名词解释部分

*[回到目录](#目录)*
### <a name="linkkit_answer_service">linkkit_answer_service</a>

原型
---
```
int deprecated linkkit_answer_service(const void *thing_id, const char *service_identifier, int response_id, int code);
```

接口说明
---
在用户收到异步服务调用请求后, 可使用该接口向云端回复异步服务的输出参数

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| thing_id            | const void *    | 输入      | 设备ID
| service_identifier  | const char *    | 输入      | 输出参数的ID
| response_id         | int             | 输入      | 消息ID, 应该与接收到异步服务请求的消息ID一致
| code                | int             | 输入      | 返回给云端的错误码

返回值说明
---
| 值 | 说明
|---|---
| 0       | 成功
| < 0     | 失败

-----

*[回到目录](#目录)*
### <a name="linkkit_cota_init">linkkit_cota_init</a>

原型
---
```
int deprecated linkkit_cota_init(handle_service_cota_callback_fp_t callback_fp);
```

接口说明
---
初始化Config OTA的用户回调函数. 当获取到配置文件的更新信息时, 会通过此回调函数通知用户

当用户收到通知后, 可调用`linkkit_invoke_cota_service`获取配置文件

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| callback_fp     | handle_service_cota_callback_fp_t   | 输入      | 用户回调函数

返回值说明
---
| 值 | 说明
|---|---
| 0       | 成功
| < 0     | 失败

参数的数据结构说明
---
```
typedef void (*handle_service_cota_callback_fp_t)(
                                service_cota_callback_type_t callback_type, 
                                const char *configid,
                                uint32_t configsize,
                                const char *gettype,
                                const char *sign,
                                const char *signmethod,
                                const char *cota_url);
```
+ `callback_type`: 消息类型, 目前仅支持`service_cota_callback_type_new_version_detected`
+ `configid`: 配置文件ID
+ `configsize`: 配置文件大小
+ `gettype`: 配置文件数据类型, 当前为`文件`
+ `sign`: 文件的签名
+ `signmethod`: 计算签名所用的签名方法
+ `cota_url`: 配置文件下载地址

-----

*[回到目录](#目录)*
### <a name="linkkit_dispatch">linkkit_dispatch</a>

原型
---
```
void *linkkit_dispatch(void);
```

接口说明
---
该接口用于用户获取sdk中的各种事件, 如属性设置请求/服务调用请求等, 需要用户周期性调用

参数说明
---
无参数

返回值说明
---
| 值 | 说明
|---|---
| NULL    | 无

-----

*[回到目录](#目录)*
### <a name="linkkit_end">linkkit_end</a>

原型
---
```
int deprecated linkkit_end(void);
```

接口说明
---
关闭Linkkit与云端的连接并释放所有资源

参数说明
---
无参数

返回值说明
---
| 值 | 说明
|---|---
| 0       | 成功
| < 0     | 失败

-----

*[回到目录](#目录)*
### <a name="linkkit_fota_init">linkkit_fota_init</a>

原型
---
```
int deprecated linkkit_fota_init(handle_service_fota_callback_fp_t callback_fp);
```

接口说明
---
初始化Firmware OTA的用户回调函数. 当获取到固件的更新信息时, 会通过此回调函数通知用户

当用户收到通知后, 可调用`linkkit_invoke_fota_service`获取固件

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| callback_fp     | handle_service_fota_callback_fp_t   | 输入      | 用户回调函数

返回值说明
---
| 值 | 说明
|---|---
| 0       | 成功
| < 0     | 失败

参数的数据结构说明
---
```
typedef void (*handle_service_fota_callback_fp_t)(
                                service_fota_callback_type_t callback_type, 
                                const char *version);
```
+ `callback_type`: 消息类型, 目前仅支持`service_fota_callback_type_new_version_detected`
+ `version`: 当前可获取固件的版本号

-----

*[回到目录](#目录)*
### <a name="linkkit_get_value">linkkit_get_value</a>

原型
---
```
int deprecated linkkit_get_value(linkkit_method_get_t method_get, 
                                    const void *thing_id, 
                                    const char *identifier,
                                    void *value, 
                                    char **value_str);
```

接口说明
---
该接口用于获取设备影子中的TSL的相关值, `value`和`value_str`选填一个即可

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| method_get  | linkkit_method_get_t    | 输入      | 需要获取的TSL数据类型
| thing_id    | const void *            | 输入      | 设备ID
| identifier  | const char *            | 输入      | TSL数据标识的ID
| value       | void *                  | 输出      | 该buffer用于存放获取到的值
| value_str   | char **                 | 输出      | 获取值的字符串形式

返回值说明
---
| 值 | 说明
|---|---
| 0       | 成功
| < 0     | 失败

参数的数据类型说明
---
```
typedef enum {
    linkkit_method_get_property_value = 0,
    linkkit_method_get_event_output_value,
    linkkit_method_get_service_input_value,
    linkkit_method_get_service_output_value,
    linkkit_method_get_number,
} linkkit_method_get_t;
```
+ linkkit_method_get_property_value: TSL中的属性
+ linkkit_method_get_event_output_value: TSL中事件的输出参数
+ linkkit_method_get_service_input_value: TSL中服务的输入参数
+ linkkit_method_get_service_output_value: TSL中服务的输出参数

-----

*[回到目录](#目录)*
### <a name="linkkit_invoke_cota_get_config">linkkit_invoke_cota_get_config</a>

原型
---
```
int deprecated linkkit_invoke_cota_get_config(
                                const char *config_scope, 
                                const char *get_type,
                                const char *attribute_Keys, 
                                void *option);
```

接口说明
---
该接口用于向云端主动发起Config OTA, 即主动发起配置文件获取请求

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| config_scope    | const char *    | 输入      | 配置域, 当前为`product`
| get_type        | const char *    | 输入      | 配置的数据类型, 当前为`file`
| attribute_Keys  | const char *    | 输入      | 配置的属性信息, 当前为空字符串即可
| option          | void *          | 输入      | 保留

返回值说明
---
| 值 | 说明
|---|---
| 0       | 成功
| < 0     | 失败

-----

*[回到目录](#目录)*
### <a name="linkkit_invoke_cota_service">linkkit_invoke_cota_service</a>

原型
---
```
int deprecated linkkit_invoke_cota_service(void *data_buf, int data_buf_length);
```

接口说明
---
该接口用于发起Config OTA请求, 需要在`linkkit_cota_init`设置的回调函数中使用

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| data_buf            | void *  | 输入      | 用于获取配置文件的临时buffer
| data_buf_length     | int     | 输入      | `data_buf`的长度

返回值说明
---
| 值 | 说明
|---|---
| 0       | 成功
| < 0     | 失败

-----

*[回到目录](#目录)*
### <a name="linkkit_invoke_fota_service">linkkit_invoke_fota_service</a>

原型
---
```
int deprecated linkkit_invoke_fota_service(void *data_buf, int data_buf_length);
```

接口说明
---
该接口用于发起Firmware OTA请求, 需要在`linkkit_fota_init`设置的回调函数中使用

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| data_buf            | void *  | 输入      | 用于获取固件的临时buffer
| data_buf_length     | int     | 输入      | `data_buf` 的长度

返回值说明
---
| 值 | 说明
|---|---
| 0       | 成功
| < 0     | 失败

-----

*[回到目录](#目录)*
### <a name="linkkit_invoke_raw_service">linkkit_invoke_raw_service</a>

原型
---
```
int deprecated linkkit_invoke_raw_service(
                            const void *thing_id, 
                            int is_up_raw, 
                            void *raw_data, 
                            int raw_data_length);
```

接口说明
---
透传数据接口. 当用户在云端控制台定义设备数据格式为`透传/自定义`时, 可使用此接口上传透传数据

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| thing_id            | const void *    | 输入      | 设备ID
| is_up_raw           | int             | 输入      | 保留
| raw_data            | void *          | 输入      | 透传数据Payload
| raw_data_length     | int             | 输入      | 透传数据的长度

返回值说明
---
| 值 | 说明
|---|---
| 0       | 成功
| < 0     | 失败

-----

*[回到目录](#目录)*
### <a name="linkkit_is_end">linkkit_is_end</a>

原型
---
```
int deprecated linkkit_is_end();
```

接口说明
---
获取当前linkkit运行状态

参数说明
---
无参数

返回值说明
---
| 值 | 说明
|---|---
| 0   | linkkit未结束运行
| 1   | linkkit已结束运行

-----

*[回到目录](#目录)*
### <a name="linkkit_is_try_leave">linkkit_is_try_leave</a>

原型
---
```
int deprecated linkkit_is_try_leave(void)
```

接口说明
---
检测当前linkkit是否正在尝试退出

参数说明
---
无参数

返回值说明
---
| 值 | 说明
|---|---
| 0   | linkkit未尝试退出
| 1   | linkkit正尝试退出

-----

*[回到目录](#目录)*
### <a name="linkkit_post_property">linkkit_post_property</a>

原型
---
```
int deprecated linkkit_post_property(const void *thing_id, 
                                        const char *property_identifier, 
                                        handle_post_cb_fp_t cb);
```

接口说明
---
向云端上报属性信息

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| thing_id                | const void *            | 输入      | 设备ID
| property_identifier     | const char *            | 输入      | 属性ID
| cb                      | handle_post_cb_fp_t     | 输入      | 属性上报返回应答时触发的回调函数

返回值说明
---
| 值 | 说明
|---|---
| 0       | 成功
| < 0     | 失败

-----

*[回到目录](#目录)*
### <a name="linkkit_set_opt">linkkit_set_opt</a>

原型
---
```
int deprecated linkkit_set_opt(linkkit_opt_t opt, void *data);
```

接口说明
---
在用户使用`linkkit_start`启动linkkit之前, 可用此接口对SDK部分参数进行调整

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| opt     | linkkit_opt_t   | 输入      | 选择需要配置的参数类型
| data    | void *          | 输入      | 需要配置的参数

返回值说明
---
| 值 | 说明
|---|---
| 0       | 成功
| < 0     | 失败

参数的数据结构
---
```
typedef enum {
    linkkit_opt_property_post_reply,
    linkkit_opt_event_post_reply,
    linkkit_opt_property_set_reply
} linkkit_opt_t;
```

+ `linkkit_opt_property_post_reply`: 在设备上报属性后, 是否需要云端的应答消息. `arg`数据类型为`int *`
+ `linkkit_opt_event_post_reply`: 在设备上报事件后, 是否需要云端的应答消息. `arg`数据类型为`int *`
+ `linkkit_opt_property_set_reply`: 在收到云端的属性设置后, 是否需要向云端发送应答消息. `arg`数据类型为`int *`

-----

*[回到目录](#目录)*
### <a name="linkkit_set_tsl">linkkit_set_tsl</a>

原型
---
```
void *linkkit_set_tsl(const char *tsl, int tsl_len);
```

接口说明
---
该接口可将 `TSL` 转换成设备影子

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| tsl         | const char *    | 输入      | 设备所属产品的TSL
| tsl_len     | int             | 输入      | `tsl`的长度

返回值说明
---
| 值 | 说明
|---|---
| 0       | 成功
| < 0     | 失败

-----

*[回到目录](#目录)*
### <a name="linkkit_set_value">linkkit_set_value</a>

原型
---
```
int deprecated linkkit_set_value(linkkit_method_set_t method_set, 
                                    const void *thing_id, 
                                    const char *identifier,
                                    const void *value, 
                                    const char *value_str);
```

接口说明
---
用户可通过此接口设置设备影子的值, `value`和`value_str`选填一个即可

参数说明
---

| 参数                  | 数据类型              | 方向  |说明
|-----------------------|-----------------------|-------|--------------------------------------------------------
| method_set  | linkkit_method_set_t    | 输入      | 需要设置的TSL数据类型
| thing_id    | const void *            | 输入      | 设备ID
| identifier  | const char *            | 输入      | TSL数据标识的ID
| value       | const void *            | 输入      | 需要设置的具体value
| value_str   | const char *            | 输入      | 需要设置的具体value的字符串表达形式

返回值说明
---
| 值 | 说明
|---|---
| 0       | 成功
| < 0     | 失败

-----

*[回到目录](#目录)*
### <a name="linkkit_start">linkkit_start</a>

原型
---
```
int deprecated linkkit_start(int max_buffered_msg,
                                int get_tsl_from_cloud, 
                                linkkit_loglevel_t log_level,
                                linkkit_ops_t *ops,
                                linkkit_cloud_domain_type_t domain_type, 
                                void *user_context);
```

接口说明
---
该接口用于启动linkkit, 在调用该接口之前, 可使用 `linkkit_set_opt` 对SDK进行部分参数的调整

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_trigger_event">linkkit_trigger_event</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_trigger_extended_info_operate">linkkit_trigger_extended_info_operate</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_try_leave">linkkit_try_leave</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_yield">linkkit_yield</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
## <a name="6.5 高级版(旧版)网关API详解">6.5 高级版(旧版)网关API详解</a>

> 本节介绍的API适用于您的设备具有对子设备的管理能力的场景, 参看 [网关](#网关) 的名词解释部分

*[回到目录](#目录)*
### <a name="linkkit_gateway_delete_extinfos">linkkit_gateway_delete_extinfos</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_gateway_exit">linkkit_gateway_exit</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_gateway_fota_init">linkkit_gateway_fota_init</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_gateway_get_default_params">linkkit_gateway_get_default_params</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_gateway_get_devinfo">linkkit_gateway_get_devinfo</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_gateway_get_num_devices">linkkit_gateway_get_num_devices</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_gateway_init">linkkit_gateway_init</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_gateway_invoke_fota_service">linkkit_gateway_invoke_fota_service</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_gateway_post_extinfos">linkkit_gateway_post_extinfos</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_gateway_post_property_json">linkkit_gateway_post_property_json</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_gateway_post_property_json_sync">linkkit_gateway_post_property_json_sync</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_gateway_post_rawdata">linkkit_gateway_post_rawdata</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_gateway_set_event_callback">linkkit_gateway_set_event_callback</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_gateway_setopt">linkkit_gateway_setopt</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_gateway_start">linkkit_gateway_start</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_gateway_stop">linkkit_gateway_stop</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_gateway_subdev_create">linkkit_gateway_subdev_create</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_gateway_subdev_destroy">linkkit_gateway_subdev_destroy</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_gateway_subdev_login">linkkit_gateway_subdev_login</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_gateway_subdev_logout">linkkit_gateway_subdev_logout</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_gateway_subdev_register">linkkit_gateway_subdev_register</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_gateway_subdev_unregister">linkkit_gateway_subdev_unregister</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_gateway_trigger_event_json">linkkit_gateway_trigger_event_json</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----

*[回到目录](#目录)*
### <a name="linkkit_gateway_trigger_event_json_sync">linkkit_gateway_trigger_event_json_sync</a>

原型
---

接口说明
---

参数说明
---

返回值说明
---

-----




第七章目录
---
+ [第七章 典型场景示例](#第七章 典型场景示例)
    * [7.1 MQTT站点配置](#7.1 MQTT站点配置)
        - [基础版](#基础版)
        - [高级版(旧版)](#高级版(旧版))
    * [7.2 TSL静态集成和动态拉取](#7.2 TSL静态集成和动态拉取)
        - [TSL静态集成](#TSL静态集成)
        - [TSL动态拉取](#TSL动态拉取)
            + [高级版单品使用TSL动态拉取](#高级版单品使用TSL动态拉取)
            + [高级版网关使用TSL动态拉取](#高级版网关使用TSL动态拉取)
    * [7.3 单品动态注册/一型一密](#7.3 单品动态注册/一型一密)
        - [动态注册/一型一密的概念](#动态注册/一型一密的概念)
        - [相关API和例程](#相关API和例程)
        - [完整过程示例](#完整过程示例)


*[回到目录](#目录)*
# <a name="第七章 典型场景示例">第七章 典型场景示例</a>
*[回到目录](#目录)*
## <a name="7.1 MQTT站点配置">7.1 MQTT站点配置</a>
在使用阿里云物联网套件连接阿里云时, 需要指定MQTT需要连接的站点, 在基础版和高级版中, 站点配置方法如下:

*[回到目录](#目录)*
### <a name="基础版">基础版</a>
在 `iotx-sdk-c/include/iot_export.h` 中, 枚举类型 `iotx_cloud_domain_types_t` 定义了当前可连接的MQTT站点:

    /* domain type */
    typedef enum IOTX_CLOUD_DOMAIN_TYPES {
        /* Shanghai */
        IOTX_CLOUD_DOMAIN_SH,

        /* Singapore */
        IOTX_CLOUD_DOMAIN_SG,

        /* Japan */
        IOTX_CLOUD_DOMAIN_JP,

        /* America */
        IOTX_CLOUD_DOMAIN_US,

        /* Germany */
        IOTX_CLOUD_DOMAIN_GER,

        /* Maximum number of domain */
        IOTX_CLOUD_DOMAIN_MAX
    } iotx_cloud_domain_types_t;

在使用基础版MQTT接口连接阿里云时, 使用 [IOT_Ioctl](#IOT_Ioctl) 的 `IOTX_IOCTL_SET_DOMAIN` 选项设置要连接的站点, 然后再使用 [IOT_SetupConnInfo](#IOT_SetupConnInfo) 来建立设备到阿里云的连接

*[回到目录](#目录)*
### <a name="高级版(旧版)">高级版(旧版)</a>
在 `iotx-sdk-c/include/exports/linkkit_export.h` 中, 枚举类型 `linkkit_cloud_domain_type_t` 定义了当前可连接的MQTT站点:

    typedef enum {
        /* shanghai */
        linkkit_cloud_domain_shanghai,
        /* singapore */
        linkkit_cloud_domain_singapore,
        /* japan */
        linkkit_cloud_domain_japan,
        /* america */
        linkkit_cloud_domain_america,
        /* germany */
        linkkit_cloud_domain_germany,

        linkkit_cloud_domain_max,
    } linkkit_cloud_domain_type_t;

在使用高级版接口连接阿里云时, 在 `linkkit_start` 中传入需要连接的站点即可

**注意事项: 如果在阿里云物联网控制台申请的三元组与连接时使用的域名不符合, 连接站点时会出现认证错误(错误码-35)**

*例如: 在阿里云物联网控制台的华东2站点申请的三元组, 在物联网套件中应该连接华东2(上海)站点*

*[回到目录](#目录)*
## <a name="7.2 TSL静态集成和动态拉取">7.2 TSL静态集成和动态拉取</a>

物模型指将物理空间中的实体数字化, 并在云端构建该实体的数据模型. 在物联网平台中, 定义物模型即定义产品功能. 完成功能定义后, 系统将自动生成该产品的物模型

物模型描述产品是什么, 能做什么, 可以对外提供哪些服务

物模型以 JSON 格式表述, 称之为 TSL(即 Thing Specification Language). 在产品的功能定义页面, 单击查看物模型即可查看 JSON 格式的 TSL:

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/LP-%E4%BA%A7%E5%93%81%E7%AE%A1%E7%90%86-%E4%BA%A7%E5%93%81%E8%AF%A6%E6%83%85-%E7%89%A9%E6%A8%A1%E5%9E%8B.png)

此时有两种方式在物联网套件中使用TSL:

- 静态集成: 指将TSL直接写到代码中, 这时需要上图中选择导出物模型, 将设备的TSL(JSON格式)导出到文件, 并手动集成到代码中
- 动态拉取: 指在物联网套件运行时去云端拉取TSL

*[回到目录](#目录)*
### <a name="TSL静态集成">TSL静态集成</a>
如上所述, 将设备的TSL(JSON格式)导出到文件后(默认文件名为model.json), 由于C语言需要将字符串中的双引号`"`进行转义, 所以需要将导出的TSL文件进行转义

在网上有很多这样的转义工具, 物联网套件中也自带了一个可以转义JSON的小工具

当SDK编译完成后, 使用 `iotx-sdk-c/output/release/bin` 目录下的 `linkkit_tsl_convert` 可以完成该转义任务. 使用方法如下:

     $./linkkit_tsl_convert -i model.json

上述命令执行完成后, 会在调用命令的当前目录生成一个 `conv.txt` 文件, 里面就是转义后的JSON字符串. 目前仅高级版单品支持静态集成TSL, 集成方法如下:

高级版单品使用TSL静态集成
---
默认情况下, 物联网套件编译单品版本的例子程序, 源代码位于 `iotx-sdk-c/examples/linkkit` 目录下的 `linkkit_example_solo.c`

同目录下的 `example_tsl_solo.data` 为存放静态集成TSL的文件

将刚才 `conv.txt` 中的内容复制, 替换 `example_tsl_solo.data` 文件中的 `TSL_STRING` 变量, 重新编译即可

不要忘了将 `linkkit_example_solo.c` 中的三元组替换成该TSL对应产品下设备的三元组

*[回到目录](#目录)*
### <a name="TSL动态拉取">TSL动态拉取</a>
如果使用动态拉取TSL, 就不用去替换代码中的静态TSL

需要注意的是, 使用动态拉取TSL时, 在TSL占用空间较大(TSL定义的服务、属性、事件越多, TSL越大)的情况下, 有可能需要更改物联网套件默认的MQTT接收buffer长度

可以按如下两种方法进行配置更改:

- 修改平台配置文件, 在当前使用平台的config.xxx.xxx文件(位于iotx-sdk-c/src/board目录下)中, 修改 `CONFIG_ENV_CFLAGS` 中的 `CONFIG_MQTT_RX_MAXLEN` 编译选项

> 以ubuntu为例, 如下图所示:

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/iotkit-MQTT%E9%BB%98%E8%AE%A4%E6%8E%A5%E6%94%B6%E9%95%BF%E5%BA%A6%E5%9C%A8Makefile%E4%B8%AD%E7%9A%84%E9%85%8D%E7%BD%AE.png)

- 在代码中修改默认值, 在 `iotx-sdk-c/include/imports/iot_import_config.h` 文件中修改 `CONFIG_MQTT_RX_MAXLEN` 即可, 如下图所示:

![image](https://linkkit-export.oss-cn-shanghai.aliyuncs.com/iotkit-MQTT%E9%BB%98%E8%AE%A4%E6%8E%A5%E6%94%B6%E9%95%BF%E5%BA%A6%E5%9C%A8iot_import_config.h%E4%B8%AD%E7%9A%84%E9%85%8D%E7%BD%AE.png)

*注意: 在Makefile中设置的 `CONFIG_MQTT_RX_MAXLEN` 值会覆盖掉 `iot_import_config.h` 中设置的值*

#### <a name="高级版单品使用TSL动态拉取">高级版单品使用TSL动态拉取</a>
默认情况下, 在高级版单品的example(源代码位于 `iotx-sdk-c/examples/linkkit` 目录下的 `linkkit_example_solo.c` )中, 使用的是TSL静态集成

若要使用TSL动态拉取, 只需要将 `linkkit_start` 的第二个入参 `get_tsl_from_cloud` 设为`1`即可

#### <a name="高级版网关使用TSL动态拉取">高级版网关使用TSL动态拉取</a>
默认情况下, 在高级版网关的example(源代码位于 `iotx-sdk-c/examples/linkkit` 目录下的 `linkkit_example_gateway.c` )中, 使用的总是TSL动态拉取

*[回到目录](#目录)*
## <a name="7.3 单品动态注册/一型一密">7.3 单品动态注册/一型一密</a>
*[回到目录](#目录)*
### <a name="动态注册/一型一密的概念">动态注册/一型一密的概念</a>
*[回到目录](#目录)*
### <a name="相关API和例程">相关API和例程</a>
*[回到目录](#目录)*
### <a name="完整过程示例">完整过程示例</a>

附录A目录
---
+ [附录A 功能分项说明](#附录A 功能分项说明)
    * [A.1 基础通信](#A.1 基础通信)
        - [使用 MQTT 连接阿里云IoT平台](#使用 MQTT 连接阿里云IoT平台)
        - [使用 CoAP 连接阿里云IoT平台](#使用 CoAP 连接阿里云IoT平台)
        - [使用 HTTPS 连接阿里云IoT平台](#使用 HTTPS 连接阿里云IoT平台)
        - [使用 HTTP2 连接阿里云IoT平台](#使用 HTTP2 连接阿里云IoT平台)
    * [A.2 设备管理](#A.2 设备管理)
        - [设备TSL物模型管理](#设备TSL物模型管理)
        - [设备拓扑关系管理](#设备拓扑关系管理)
        - [透传与脚本解析](#透传与脚本解析)
        - [固件升级](#固件升级)
        - [远程配置](#远程配置)
        - [设备标签管理](#设备标签管理)
        - [设备影子](#设备影子)
    * [A.3 WiFi配网](#A.3 WiFi配网)
    * [A.4 本地通信](#A.4 本地通信)

*[回到目录](#目录)*
# <a name="附录A 功能分项说明">附录A 功能分项说明</a>

*[回到目录](#目录)*
## <a name="A.1 基础通信">A.1 基础通信</a>

*[回到目录](#目录)*
### <a name="使用 MQTT 连接阿里云IoT平台">使用 MQTT 连接阿里云IoT平台</a>
*[回到目录](#目录)*
### <a name="使用 CoAP 连接阿里云IoT平台">使用 CoAP 连接阿里云IoT平台</a>
*[回到目录](#目录)*
### <a name="使用 HTTPS 连接阿里云IoT平台">使用 HTTPS 连接阿里云IoT平台</a>
*[回到目录](#目录)*
### <a name="使用 HTTP2 连接阿里云IoT平台">使用 HTTP2 连接阿里云IoT平台</a>

*[回到目录](#目录)*
## <a name="A.2 设备管理">A.2 设备管理</a>

*[回到目录](#目录)*
### <a name="设备TSL物模型管理">设备TSL物模型管理</a>
*[回到目录](#目录)*
### <a name="设备拓扑关系管理">设备拓扑关系管理</a>
*[回到目录](#目录)*
### <a name="透传与脚本解析">透传与脚本解析</a>
*[回到目录](#目录)*
### <a name="固件升级">固件升级</a>
*[回到目录](#目录)*
### <a name="远程配置">远程配置</a>
*[回到目录](#目录)*
### <a name="设备标签管理">设备标签管理</a>
*[回到目录](#目录)*
### <a name="设备影子">设备影子</a>

*[回到目录](#目录)*
## <a name="A.3 WiFi配网">A.3 WiFi配网</a>
*[回到目录](#目录)*
## <a name="A.4 本地通信">A.4 本地通信</a>


附录C目录
---
+ [附录C 云端接口说明](#附录C 云端接口说明)
    * [C.1 设备身份注册](#C.1 设备身份注册)
        - [直连设备动态注册(一型一密)](#直连设备动态注册(一型一密))
        - [子设备动态注册](#子设备动态注册)
        - [子设备动态注销](#子设备动态注销)
    * [C.2 子设备与网关的拓扑关系](#C.2 子设备与网关的拓扑关系)
        - [添加拓扑关系](#添加拓扑关系)
        - [删除拓扑关系](#删除拓扑关系)
        - [获取拓扑关系列表](#获取拓扑关系列表)
        - [设备发现上报](#设备发现上报)
        - [通知设备添加拓扑关系](#通知设备添加拓扑关系)
    * [C.3 设备属性、服务、事件](#C.3 设备属性、服务、事件)
        - [设备属性上报](#设备属性上报)
        - [设备属性设置](#设备属性设置)
        - [设备属性获取](#设备属性获取)
        - [设备服务触发(异步)](#设备服务触发(异步))
        - [设备服务触发(同步)](#设备服务触发(同步))
        - [数据透传](#数据透传)
    * [C.4 设备标签](#C.4 设备标签)
        - [设备标签信息上报](#设备标签信息上报)
        - [设备标签信息删除](#设备标签信息删除)


*[回到目录](#目录)*
# <a name="附录C 云端接口说明">附录C 云端接口说明</a>

本节描述由阿里云物联网平台定义的, 云端对设备开放的通信接口, 并配合 C-SDK 中的相关部分加以说明, 比如

+ 应用层协议类型: MQTT, HTTP ...
+ 应用层协议方法: Post, Get ...
+ 应用层协议路径: URI, Topic ...
+ 应用层协议报文格式: JSON, ByteStream ...
+ 设备端 C-SDK 对应的 API 接口

*[回到目录](#目录)*
## <a name="C.1 设备身份注册">C.1 设备身份注册</a>

设备身份注册包含直连设备的动态注册(一型一密)和子设备的动态注册

*[回到目录](#目录)*
### <a name="直连设备动态注册(一型一密)">直连设备动态注册(一型一密)</a>

- 动态注册URL: https://{host}/auth/register/device (host根据所选区域不同而有所差别)
- HTTP Method: post

Request:

    POST /auth/register/device  HTTP/1.1
    Host: iot-auth.cn-shanghai.aliyuncs.com
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 123
    productKey=1234556554&deviceName=deviceName1234&random=567345&sign=adfv123hdfdh&signMethod=hmac_md5

Response:

    {
        "code" : 200,
        "data" : {
            "productKey" : "1234556554",
            "deviceName" : "deviceName1234",
            "deviceSecret" : "adsfweafdsf"
        },
        "message": "success"
    }

说明: SDK使用https向云端注册直连设备, 需要用户提供设备的ProductKey和DeviceName, 结合随机数Random和签名方法产生签名, 向云端发起请求

云端校验签名成功后, 会在response中下发设备的三元组, 这时用户可以获取到设备的DeviceSecret

注意事项:
---

- SDK获取到设备DeviceSecret后会调用 [HAL_SetDeviceSecret](#HAL_SetDeviceSecret) 进行持久化操作
- 每个设备的DeviceSecret仅能通过本方式获取一次, 第二次获取时云端回会返回错误

        {"code":6289,"message":"device is already active"}

- 目前支持的签名方式: hmac_md5, hmacsha1, hmacsha256
- 签名计算方法: 使用**deviceName%sproductKey%srandom%s**作为原始数据, **ProductSecret**作为机密密钥, 使用hmac系列算法计算出hash值. 当前SDK仅使用hmacsha256计算签名
- 区域设置, 以华东2为例:

        /* Choose Login Server */
        int domain_type = IOTX_CLOUD_DOMAIN_SH;
        IOT_Ioctl(IOTX_IOCTL_SET_DOMAIN, (void *)&domain_type);

- 使用一型一密:

        /* Choose Login Method */
        int dynamic_register = 1;
        IOT_Ioctl(IOTX_IOCTL_SET_DYNAMIC_REGISTER, (void *)&dynamic_register);

*[回到目录](#目录)*
### <a name="子设备动态注册">子设备动态注册</a>

- 上行topic: /sys/{productKey}/{deviceName}/thing/sub/register
- 下行topic: /sys/{productKey}/{deviceName}/thing/sub/register_reply

Request:

    {
        "id":"4",
        "version":"1.0",
        "params":[{
            "productKey":"a13Npv1vjZ4",
            "deviceName":"example1"
        }],
        "method":"thing.sub.register"
    }

Response:

    {
        "code":200,
        "data":[{
            "iotId":"I2aQWKeC1YgPyPr18og1001052f300",
            "deviceSecret":"2o8iZ3jLuDd40JIKlRRPAhQSfjqbhkkH",
            "productKey":"a13Npv1vjZ4",
            "deviceName":"example1"
        }],
        "id":"4",
        "message":"success",
        "method":"thing.sub.register",
        "version":"1.0"
    }

说明: SDK在网关场景下, 提供子设备动态注册功能, 需要用户使用待注册设备的ProductKey和DeviceSecret, 向云端获取设备的DeviceSecret

注意事项:
---

- 使用子设备动态注册之前, 需要在云端控制台上建立好设备, 否则在进行注册操作时, 云端会返回如下形式的错误

        {"code":6100,"data":{},"id":"4","message":"device not found","method":"thing.sub.register","version":"1.0"}

- 在每次使用子设备动态注册时, 子设备的DeviceSecret会在云端重新生成

- 使用方法:

        /* subdev_pk: 子设备ProductKey,
         * subdev_dn: 子设备DeviceName,
         * subdev_ds:  子设备DeviceSecret, 若要使用动态注册功能, 此入参应为NULL或者空字符串
         */
        linkkit_gateway_subdev_register(subdev_pk,subdev_dn,subdev_ds);

*[回到目录](#目录)*
### <a name="子设备动态注销">子设备动态注销</a>

- 上行topic: /sys/{productKey}/{deviceName}/thing/sub/unregister
- 下行topic: /sys/{productKey}/{deviceName}/thing/sub/unregister_reply

Request:

    {
        "id":"12",
        "version":"1.0",
        "params":[{
            "productKey":"a13Npv1vjZ4",
            "deviceName":"example_unreg1"
        }],
        "method":"thing.sub.unregister"
    }

Response:

    {
        "code":200,
        "data":{},
        "id":"12",
        "message":"success",
        "method":"thing.sub.unregister",
        "version":"1.0"
    }

说明: 在网关场景下, 子设备动态注销时, 需要子设备和网关之间存在topo关系, 否则不能注销

子设备注销执行成功后, 云端会删掉与该子设备相关的所有信息, 若想找回该设备, 只能在云端控制台重新添加设备

注意事项:
---

 - 此接口SDK暂不露出

*[回到目录](#目录)*
## <a name="C.2 子设备与网关的拓扑关系">C.2 子设备与网关的拓扑关系</a>

网关可添加、删除与子设备的拓扑关系

*[回到目录](#目录)*
### <a name="添加拓扑关系">添加拓扑关系</a>

- 上行topic: /sys/{productKey}/{deviceName}/thing/topo/add
- 下行topic: /sys/{productKey}/{deviceName}/thing/topo/add_reply

Request:

    {
        "id":"4",
        "version":"1.0",
        "params":[{
            "productKey":"a13Npv1vjZ4",
            "deviceName":"example1",
            "signmethod":"hmacSha1",
            "sign":"db001f871e4cff9698a433b65ba0f3ef27af398d",
            "timestamp":"453313937",
            "clientId":"a13Npv1vjZ4.example1"
        }],
        "method":"thing.topo.add"
    }

Response:

    {
        "code":200,
        "data":{},
        "id":"4",
        "message":"success",
        "method":"thing.topo.add",
        "version":"1.0"
    }

说明: 只有当子设备与网关建立拓扑关系后, 子设备和网关才能共用一条MQTT连接与云端进行通信, 同时所有发往子设备的消息都会到达网关

注意事项:
---

- 签名算法: 使用**clientId%sdeviceName%sproductKey%stimestamp%s**作为原始数据, **DeviceSecret**作为机密密钥, 使用hmac系列算法计算出签名
- 当前SDK默认使用hmacsha1计算签名
- 使用方法: 目前添加拓扑关系的功能已并入 [linkkit_gateway_subdev_register](#linkkit_gateway_subdev_register) 接口中

*[回到目录](#目录)*
### <a name="删除拓扑关系">删除拓扑关系</a>

- 上行topic: /sys/{productKey}/{deviceName}/thing/topo/delete
- 下行topic: /sys/{productKey}/{deviceName}/thing/topo/delete_reply

Request:

    {
        "id":"11",
        "version":"1.0",
        "params":[{
            "productKey":"a13Npv1vjZ4",
            "deviceName":"example1"
        }],
        "method":"thing.topo.delete"
    }

Response:

    {
        "code":200,
        "data":{},
        "id":"11",
        "message":"success",
        "method":"thing.topo.delete",
        "version":"1.0"
    }

说明: 用户提供子设备的ProductKey和DeviceName, 即可向云端发起删除网关和子设备拓扑关系的请求

注意事项:
---

- 使用方法: 目前添删除拓扑关系的功能已并入 [linkkit_gateway_subdev_unregister](#linkkit_gateway_subdev_unregister) 接口中

*[回到目录](#目录)*
### <a name="获取拓扑关系列表">获取拓扑关系列表</a>

- 上行topic: /sys/{productKey}/{deviceName}/thing/topo/get
- 下行topic: /sys/{productKey}/{deviceName}/thing/topo/get_reply

Request:

    {
        "id":"3",
        "version":"1.0",
        "params":{},
        "method":"thing.topo.get"
    }

Response:

    {
        "code":200,
        "data":[{
            "deviceSecret":"2o8iZ3jLuDd40JIKlRRPAhQSfjqbhkkH",
            "productKey":"a13Npv1vjZ4",
            "deviceName":"example1"
        }],
        "id":"3",
        "message":"success",
        "method":"thing.topo.get",
        "version":"1.0"
    }

说明: 用户可通过此接口获取当前与网关存在拓扑关系的子设备列表

注意事项:
---

- 此接口SDK暂不露出

*[回到目录](#目录)*
### <a name="设备发现上报">设备发现上报</a>

- 上行topic: /sys/{productKey}/{deviceName}/thing/list/found
- 下行topic: /sys/{productKey}/{deviceName}/thing/list/found_reply

Request:

    {
        "id":"8",
        "version":"1.0",
        "params":[{
            "productKey":"a13Npv1vjZ4",
            "deviceName":"example1"
        }],
        "method":"thing.list.found"
    }

Response:

    {
        "code":200,
        "data":{},
        "id":"8",
        "message":"success",
        "method":"thing.list.found",
        "version":"1.0"
    }

说明: 用户若网关本地发现新设备, 可通过此接口通知云端

此时若用户在云端做了相关接口的云云对接, 那么可通过云端消息流转将发现的新设备发往用户云端做鉴权等操作

此接口应与**通知设备添加拓扑关系**结合使用

注意事项:
---

- 此接口SDK暂不露出

*[回到目录](#目录)*
### <a name="通知设备添加拓扑关系">通知设备添加拓扑关系</a>

- 下行topic: /sys/{productKey}/{deviceName}/thing/topo/add/notify
- 上行topic: /sys/{productKey}/{deviceName}/thing/topo/add/notify_reply

Request:

    {
        "id" : "7659872",
        "version":"1.0",
        "params" : [{
            "deviceName" : "a13Npv1vjZ4",
            "productKey" : "example1"
        }],
        "method":"thing.topo.add.notify"
    }

Response:

    {
        "id":"7659872",
        "code":200,
        "data":{}
    }

说明: 若用户通过 [设备发现上报](#设备发现上报) 将子设备信息上报至云端, 接着云端消息流转到用户云端

用户云端得到子设备信息并进行鉴权等操作后, 若需要该子设备与网关建立拓扑关系, 则可通过此接口下发通知

当网关收到该消息后, 可调用SDK接口发起建立拓扑关系的请求

注意事项:
---

- 此接口SDK暂不露出

*[回到目录](#目录)*
## <a name="C.3 设备属性、服务、事件">C.3 设备属性、服务、事件</a>

关于对设备物模型的操作将在这里介绍

属性: 指备的特性, 如灯的开关状态, 功率大小等

服务: 当设备需要被控制时, 从云端下发的指令. 服务只能由云端主动发起, 设备只能被动应答

事件: 当设备有重要事件需要上报时使用. 事件只能由设备主动发起, 云端被动接收

*[回到目录](#目录)*
### <a name="设备属性上报">设备属性上报</a>

- 上行topic: /sys/{productKey}/{deviceName}/thing/event/property/post
- 下行topic: /sys/{productKey}/{deviceName}/thing/event/property/post_reply

Request:

    {
        "id":"9",
        "version":"1.0",
        "params":{
            "LightSwitch":1
        },
        "method":"thing.event.property.post"
    }

Response:

    {
        "code":200,
        "data":{},
        "id":"9",
        "message":"success",
        "method":"thing.event.property.post",
        "version":"1.0"
    }

说明: 用户可以通过此接口向云端上报自己的属性值, params格式为JSON对象, 可同时上报多个属性

注意事项:
---

- 上报的最小粒度为属性, 即使属性为符合属性, 也只能上报整个属性
- 对于上报属性的应答(/thing/event/property/post_reply), SDK可配置是否需要从云端接收该应答来达到减小用户流量的目的(目前SDK默认关闭该应答), 设置方法如下:

        /* 单品接口 */
        int post_property_reply = 0; /* 0 - 关闭应答, 1 - 打开应答 */
        linkkit_set_opt( [linkkit_opt_property_post_reply](#linkkit_opt_property_post_reply) , &post_property_reply);

        /* 网关接口 */
        int post_property_reply = 0; /* 0 - 关闭应答, 1 - 打开应答 */
        linkkit_params_t *initParams = linkkit_gateway_get_default_params();
	    linkkit_gateway_setopt(initParams, LINKKIT_OPT_PROPERTY_POST_REPLY, &prop_post_reply, sizeof(int));

- 使用方法:

        /* 单品接口 */
        int deprecated linkkit_post_property(const void *thing_id, const char *property_identifier, handle_post_cb_fp_t cb);

        /* 网关接口 */
        int linkkit_gateway_post_property_json_sync(int devid, char *property, int timeout_ms);
        int linkkit_gateway_post_property_json(int devid, char *property, int timeout_ms, void (*func)(int retval, void *ctx),void *ctx);

*[回到目录](#目录)*
### <a name="设备属性设置">设备属性设置</a>

- 下行topic: /sys/{productKey}/{deviceName}/thing/service/property/set
- 上行topic: /sys/{productKey}/{deviceName}/thing/service/property/set_reply

Request:

    {
        "method":"thing.service.property.set",
        "id":"79665347",
        "params":{
            "LightSwitch":1
        },
        "version":"1.0.0"
    }

Response:

    {
        "id":"79665347",
        "code":200,
        "data":{}
    }

说明: 用户可通过此接口从云端设置属性到设备, params格式为JSON对像, 可同时设置多个属性

注意事项:
---

- 对于设置属性的应答(/thing/service/property/set_reply), SDK可配置是否需要向云端发送该应答来达到减小用户流量的目的(目前SDK默认关闭该应答), 设置方法如下:

        /* 单品接口 */
        int set_property_reply = 0; /* 0 - 关闭应答, 1 - 打开应答 */
        linkkit_set_opt( [linkkit_opt_property_set_reply](#linkkit_opt_property_set_reply) , &set_property_reply);

        /* 网关接口 */
        int set_property_reply = 0; /* 0 - 关闭应答, 1 - 打开应答 */
        linkkit_params_t *initParams = linkkit_gateway_get_default_params();
	    linkkit_gateway_setopt(initParams, LINKKIT_OPT_PROPERTY_SET_REPLY, &set_property_reply, sizeof(int));

- 使用方法:

        当设备接收到从云端设置的属性值时, 会将该消息分发到用户的回调函数中:
        /* 单品回调函数, 用户会收到属性改变的通知, 此时使用 [linkkit_get_value](#linkkit_get_value) 即可取得属性的值 */
        int (*thing_prop_changed)(const void *thing_id, const char *property, void *ctx);
        int deprecated linkkit_get_value( [linkkit_method_get_](#linkkit_method_get_) t method_get, const void *thing_id, const char *identifier,void *value, char **value_str);

*[回到目录](#目录)*
### <a name="设备属性获取">设备属性获取</a>

- 下行topic: /sys/{productKey}/{deviceName}/thing/service/property/get
- 上行topic: /sys/{productKey}/{deviceName}/thing/event/property/post

Request:

    {
        "id" : "78283493",
        "version":"1.0",
        "params" : [
            "power" , "temp"
        ],
        "method":"thing.service.property.get"
    }

Response:

    {
        "id":"9",
        "version":"1.0",
        "params":{
            "power":34,
            "temp":3
        },
        "method":"thing.event.property.post"
    }

说明: 由于目前云端会直接从自己的设备缓存中返回属性值给用户, 此接口暂时无效

*[回到目录](#目录)*
### <a name="设备服务触发(异步)">设备服务触发(异步)</a>

- 下行topic: /sys/{productKey}/{deviceName}/thing/service/{serviceid}
- 上行topic: /sys/{productKey}/{deviceName}/thing/service/{serviceid}_reply；

Request:

    {
        "method":"thing.service.Custom",
        "id":"79687227",
        "params":{
            "transparency":34
        },
        "version":"1.0.0"
    }

Response:

    {
        "id":"79687227",
        "code":200,
        "data":{
            "Contrastratio":35
        }
    }

说明: 用户可通过该接口进行异步服务调用, 上述示例中, **Custom**为服务ID, **transparency**为服务的输入参数(从云端发送到设备), **Contrastratio**为服务的输出参数(从设备发送到云端)

注意事项:
---

- 使用方法:

        /* 单品回调函数 */
        int (*thing_call_service)(const void *thing_id, const char *service, int request_id, void *ctx);

        /* 网关回调函数 */
        int (*call_service)(char *identifier, char *in, char *out, int out_len, void *ctx);

*[回到目录](#目录)*
### <a name="设备服务触发(同步)">设备服务触发(同步)</a>

- 下行topic: /sys/${productKey}/${deviceName}/rrpc/request/${messageId}
- 上行topic: /sys/${productKey}/${deviceName}/rrpc/response/${messageId}

Request:

    {
        "method":"thing.service.SyncService",
        "id":"79699490",
        "params":{
            "FromCloud":78
        },
        "version":"1.0.0"
    }

Response:

    {
        "id":"79699490",
        "code":200,
        "data":{
            "ToCloud":79
        }
    }

说明: 用户可通过该接口进行同步服务调用, 上述示例中, **SyncService**为服务ID, **FromCloud**为服务的输入参数(从云端发送到设备), **ToCloud**为服务的输出参数(从设备发送到云端)

注意事项:
---

- 使用方法:

        /* 回调函数 */
        int (* sync_service_request)(
                        const int devid,
                        const char *serviceid,
                        const int serviceid_len,
                        const char *request,
                        const int request_len,
                        char **response,
                        int *response_len); -->

*[回到目录](#目录)*
### <a name="数据透传">数据透传</a>

- 上行topic: /sys/{productKey}/{deviceName}/thing/model/up_raw
- 下行topic: /sys/{productKey}/{deviceName}/thing/model/up_raw_reply

- 下行topic: /sys/{productKey}/{deviceName}/thing/model/down_raw

说明: 当用户在创建设备, 选择的数据格式为**透传/自定义**时, 可使用透传接口传输数据

注意事项:
---

- 使用方法:

        /* 单品透传上行接口及应答回调函数 */
        int deprecated linkkit_invoke_raw_service(const void *thing_id, int is_up_raw, void *raw_data, int raw_data_length);

        /* 单品透传下行接口 */
        int (*raw_data_arrived)(const void *thing_id, const void *data, int len, void *ctx);

        /* 网关透传上行接口及应答回调函数 */
        int linkkit_gateway_post_rawdata(int devid, void *data, int len);
        int (*post_rawdata_reply)(const void *data, int len, void *ctx);

        /* 网关透传下行接口 */
        ssize_t (*down_rawdata)(const void *in, int in_len, void *out, int out_len, void *ctx);

*[回到目录](#目录)*
## <a name="C.4 设备标签">C.4 设备标签</a>

设备有一些标签, 如厂商信息, 设备型号信息. 这种扩展信息比较静态, 一般只做为展示使用, 设备可以将这些信息存储在标签中

*[回到目录](#目录)*
### <a name="设备标签信息上报">设备标签信息上报</a>

- 上行topic: /sys/{productKey}/{deviceName}/thing/deviceinfo/update
- 下行topic: /sys/{productKey}/{deviceName}/thing/deviceinfo/update_reply

Request:

    {
        "id":"4",
        "version":"1.0",
        "params":[{
            "attrKey":"abc",
            "attrValue":"hello,world"
        }],
        "method":"thing.deviceinfo.update"
    }

Response:

    {
        "code":200,
        "data":{},
        "id":"4",
        "message":"success",
        "method":"thing.deviceinfo.update",
        "version":"1.0"
    }

说明: 用户在上报标签信息时, 需遵循以下格式: **[{"attrKey":"","attrValue":""},{"attrKey":"","attrValue":""}...]**. **attrKey**作为标签的Key, 其值必须以字母开头, **attrValue**作为标签的Value

注意事项:
---

- 使用方法:

        /* 单品接口 */
        int deprecated linkkit_trigger_extended_info_operate(
                            const void *thing_id,
                            const char *params,
                            linkkit_extended_info_operate_t linkkit_extended_info_operation);

        /* 网关接口 */
        int linkkit_gateway_post_extinfos(
                            int devid,
                            linkkit_extinfo_t *extinfos,
                            int nb_extinfos, int timeout_ms);

*[回到目录](#目录)*
### <a name="设备标签信息删除">设备标签信息删除</a>

- 上行topic: /sys/{productKey}/{deviceName}/thing/deviceinfo/delete
- 下行topic: /sys/{productKey}/{deviceName}/thing/deviceinfo/delete_reply

Request:

    {
        "id":"117",
        "version":"1.0",
        "params":[{
            "attrKey":"abc"
        }],
        "method":"thing.deviceinfo.delete"
    }

Response:

    {
        "code":200,
        "data":{},
        "id":"117",
        "message":"success",
        "method":"thing.deviceinfo.delete",
        "version":"1.0"
    }

说明: 用户在删除标签信息时, 需遵循以下格式: **[{"attrKey":""},{"attrKey":""}...]**. **attrKey**作为标签的Key, 其值必须以字母开头

注意事项:
---

- 使用方法:

        /* 单品接口 */
        int deprecated linkkit_trigger_extended_info_operate(
                            const void *thing_id,
                            const char *params,
                            linkkit_extended_info_operate_t linkkit_extended_info_operation);

        /* 网关接口 */
        int linkkit_gateway_delete_extinfos(int devid, linkkit_extinfo_t *extinfos, int nb_extinfos, int timeout_ms);


