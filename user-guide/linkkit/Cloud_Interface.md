# <a name="目录">目录</a>
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
    * [C.3 设备属性/服务/事件](#C.3 设备属性/服务/事件)
        - [设备属性上报](#设备属性上报)
        - [设备属性设置](#设备属性设置)
        - [设备属性获取](#设备属性获取)
        - [设备服务触发(异步)](#设备服务触发(异步))
        - [设备服务触发(同步)](#设备服务触发(同步))
        - [数据透传](#数据透传)
    * [C.4 设备标签](#C.4 设备标签)
        - [设备标签信息上报](#设备标签信息上报)
        - [设备标签信息删除](#设备标签信息删除)
    * [C.5 时间同步](#C.5 时间同步)


# <a name="附录C 云端接口说明">附录C 云端接口说明</a>

本节描述由阿里云物联网平台定义的, 云端对设备开放的通信接口, 并配合 C-SDK 中的相关部分加以说明, 比如

+ 应用层协议类型: MQTT, HTTP ...
+ 应用层协议方法: Post, Get ...
+ 应用层协议路径: URI, Topic ...
+ 应用层协议报文格式: JSON, ByteStream ...
+ 设备端 C-SDK 对应的 API 接口

## <a name="C.1 设备身份注册">C.1 设备身份注册</a>

设备身份注册包含直连设备的动态注册(一型一密)和子设备的动态注册

### <a name="直连设备动态注册(一型一密)">直连设备动态注册(一型一密)</a>

- 动态注册URL: https://{host}/auth/register/device (host根据所选区域不同而有所差别)
- HTTP Method: post

Request
---

    POST /auth/register/device  HTTP/1.1
    Host: iot-auth.cn-shanghai.aliyuncs.com
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 123
    productKey=1234556554&deviceName=deviceName1234&random=567345&sign=adfv123hdfdh&signMethod=hmac_md5

Response
---

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

### <a name="子设备动态注册">子设备动态注册</a>

- 上行topic: /sys/{productKey}/{deviceName}/thing/sub/register
- 下行topic: /sys/{productKey}/{deviceName}/thing/sub/register_reply

Request
---

    {
        "id":"4",
        "version":"1.0",
        "params":[{
            "productKey":"a13Npv1vjZ4",
            "deviceName":"example1"
        }],
        "method":"thing.sub.register"
    }

Response
---

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

### <a name="子设备动态注销">子设备动态注销</a>

- 上行topic: /sys/{productKey}/{deviceName}/thing/sub/unregister
- 下行topic: /sys/{productKey}/{deviceName}/thing/sub/unregister_reply

Request
---

    {
        "id":"12",
        "version":"1.0",
        "params":[{
            "productKey":"a13Npv1vjZ4",
            "deviceName":"example_unreg1"
        }],
        "method":"thing.sub.unregister"
    }

Response
---

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

## <a name="C.2 子设备与网关的拓扑关系">C.2 子设备与网关的拓扑关系</a>

网关可添加/删除与子设备的拓扑关系

### <a name="添加拓扑关系">添加拓扑关系</a>

- 上行topic: /sys/{productKey}/{deviceName}/thing/topo/add
- 下行topic: /sys/{productKey}/{deviceName}/thing/topo/add_reply

Request
---

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

Response
---

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

### <a name="删除拓扑关系">删除拓扑关系</a>

- 上行topic: /sys/{productKey}/{deviceName}/thing/topo/delete
- 下行topic: /sys/{productKey}/{deviceName}/thing/topo/delete_reply

Request
---

    {
        "id":"11",
        "version":"1.0",
        "params":[{
            "productKey":"a13Npv1vjZ4",
            "deviceName":"example1"
        }],
        "method":"thing.topo.delete"
    }

Response
---

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

### <a name="获取拓扑关系列表">获取拓扑关系列表</a>

- 上行topic: /sys/{productKey}/{deviceName}/thing/topo/get
- 下行topic: /sys/{productKey}/{deviceName}/thing/topo/get_reply

Request
---

    {
        "id":"3",
        "version":"1.0",
        "params":{},
        "method":"thing.topo.get"
    }

Response
---

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

### <a name="设备发现上报">设备发现上报</a>

- 上行topic: /sys/{productKey}/{deviceName}/thing/list/found
- 下行topic: /sys/{productKey}/{deviceName}/thing/list/found_reply

Request
---

    {
        "id":"8",
        "version":"1.0",
        "params":[{
            "productKey":"a13Npv1vjZ4",
            "deviceName":"example1"
        }],
        "method":"thing.list.found"
    }

Response
---

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

### <a name="通知设备添加拓扑关系">通知设备添加拓扑关系</a>

- 下行topic: /sys/{productKey}/{deviceName}/thing/topo/add/notify
- 上行topic: /sys/{productKey}/{deviceName}/thing/topo/add/notify_reply

Request
---

    {
        "id" : "7659872",
        "version":"1.0",
        "params" : [{
            "deviceName" : "a13Npv1vjZ4",
            "productKey" : "example1"
        }],
        "method":"thing.topo.add.notify"
    }

Response
---

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

## <a name="C.3 设备属性/服务/事件">C.3 设备属性/服务/事件</a>

关于对设备物模型的操作将在这里介绍

属性: 指备的特性, 如灯的开关状态, 功率大小等

服务: 当设备需要被控制时, 从云端下发的指令. 服务只能由云端主动发起, 设备只能被动应答

事件: 当设备有重要事件需要上报时使用. 事件只能由设备主动发起, 云端被动接收

### <a name="设备属性上报">设备属性上报</a>

- 上行topic: /sys/{productKey}/{deviceName}/thing/event/property/post
- 下行topic: /sys/{productKey}/{deviceName}/thing/event/property/post_reply

Request
---

    {
        "id":"9",
        "version":"1.0",
        "params":{
            "LightSwitch":1
        },
        "method":"thing.event.property.post"
    }

Response
---

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

### <a name="设备属性设置">设备属性设置</a>

- 下行topic: /sys/{productKey}/{deviceName}/thing/service/property/set
- 上行topic: /sys/{productKey}/{deviceName}/thing/service/property/set_reply

Request
---

    {
        "method":"thing.service.property.set",
        "id":"79665347",
        "params":{
            "LightSwitch":1
        },
        "version":"1.0.0"
    }

Response
---

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

### <a name="设备属性获取">设备属性获取</a>

- 下行topic: /sys/{productKey}/{deviceName}/thing/service/property/get
- 上行topic: /sys/{productKey}/{deviceName}/thing/event/property/post

Request
---

    {
        "id" : "78283493",
        "version":"1.0",
        "params" : [
            "power" , "temp"
        ],
        "method":"thing.service.property.get"
    }

Response
---

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

### <a name="设备服务触发(异步)">设备服务触发(异步)</a>

- 下行topic: /sys/{productKey}/{deviceName}/thing/service/{serviceid}
- 上行topic: /sys/{productKey}/{deviceName}/thing/service/{serviceid}_reply.

Request
---

    {
        "method":"thing.service.Custom",
        "id":"79687227",
        "params":{
            "transparency":34
        },
        "version":"1.0.0"
    }

Response
---

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

### <a name="设备服务触发(同步)">设备服务触发(同步)</a>

- 下行topic: /sys/${productKey}/${deviceName}/rrpc/request/${messageId}
- 上行topic: /sys/${productKey}/${deviceName}/rrpc/response/${messageId}

Request
---

    {
        "method":"thing.service.SyncService",
        "id":"79699490",
        "params":{
            "FromCloud":78
        },
        "version":"1.0.0"
    }

Response
---

    {
        "id":"79699490",
        "code":200,
        "data":{
            "ToCloud":79
        }
    }

说明: 用户可通过该接口进行同步服务调用, 上述示例中, **SyncService**为服务ID, **FromCloud**为服务的输入参数(从云端发送到设备), **ToCloud**为服务的输出参数(从设备发送到云端)

<!-- 注意事项:
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

## <a name="C.4 设备标签">C.4 设备标签</a>

设备有一些标签, 如厂商信息, 设备型号信息. 这种扩展信息比较静态, 一般只做为展示使用, 设备可以将这些信息存储在标签中

### <a name="设备标签信息上报">设备标签信息上报</a>

- 上行topic: /sys/{productKey}/{deviceName}/thing/deviceinfo/update
- 下行topic: /sys/{productKey}/{deviceName}/thing/deviceinfo/update_reply

Request
---

    {
        "id":"4",
        "version":"1.0",
        "params":[{
            "attrKey":"abc",
            "attrValue":"hello,world"
        }],
        "method":"thing.deviceinfo.update"
    }

Response
---

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

### <a name="设备标签信息删除">设备标签信息删除</a>

- 上行topic: /sys/{productKey}/{deviceName}/thing/deviceinfo/delete
- 下行topic: /sys/{productKey}/{deviceName}/thing/deviceinfo/delete_reply

Request
---

    {
        "id":"117",
        "version":"1.0",
        "params":[{
            "attrKey":"abc"
        }],
        "method":"thing.deviceinfo.delete"
    }

Response
---

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

## <a name="C.5 时间同步">C.5 时间同步</a>

- 上行topic: /ext/ntp/{productKey}/{deviceName}/request
- 下行topic: /ext/ntp/{productKey}/{deviceName}/response

Request
---

    {"deviceSendTime":"1234"}

Response
---

    {
        "deviceSendTime": "1234",
        "serverSendTime": "1531382849743",
        "serverRecvTime": "1531382849743"
    }
