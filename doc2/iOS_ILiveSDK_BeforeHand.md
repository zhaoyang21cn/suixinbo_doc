# 准备

# 1 下载SDK

因GitHub有文件大小限制，现将ILiveSDK、IMSDK、AVSDK以及相关Framework上传到腾讯云COS上。 更新时，请到对应的地址进行更新，并添加到工程下面对应的目录下。

Frameworks : http://dldir1.qq.com/hudongzhibo/ILiveSDK/Frameworks.zip 下载后解压，放到工程目录下。


# 2 导入项目
将下载好的Frameworks复制到工程目录下，工程目录右键，Add Files to " you projectname",在demo中如下图所示：

![](http://mc.qcloudimg.com/static/img/03ddb3785250513b0cb7b0fee2380a11/image.png)


## 库类介绍
-----
从官网下载的类库只包含一个文件夹Frameworks，里面包含了集成直播业务的所有SDK

|Frameworks文件夹|说明|
|---|---|
|AVSDK|包括音视频相关的所有SDK|
|IMSDK|包括即时通讯相关的所有SDK|
|ILiveSDK|包括ILiveSDK和所有基于ILiveSDK的业务SDK|

### 库类清单
|序号|名称|所在文件夹|说明|
|---|---|---|---|
|1|QAVSDK.framework|Frameworks/AVSDK/|音视频SDK|
|2|xplatform.framework|Frameworks/AVSDK/|银饰品SDK所依赖的SDK|
|3|IMCore.framework|Frameworks/IMSDK/|即时通讯核心库|
|4|ImSDK.framework|Frameworks/IMSDK/|即时通讯SDK|
|5|IMSDKBugly.framework|Frameworks/IMSDK/|上报SDK|
|6|QALSDK.framework|Frameworks/IMSDK/|即时通讯网络模块SDK|
|7|TLSSDK.framework|Frameworks/IMSDK/|登录服务SDK|
|8|ILiveSDK.framework|Frameworks/ILiveSDK/|互动直播基础功能SDK|
|9|TILLiveSDK.framework|Frameworks/ILiveSDK/|直播SDK(针对直播场景封装的SDK，包括互动连麦等功能)|
|10|TILCallSDK.framework|Frameworks/ILiveSDK/|多人通话SDK(针对多人通话场景封装的SDK，包括拨打电话、接听等功能)|

### [示例下载](https://github.com/zhaoyang21cn/ILiveSDK_iOS_Demos/tree/master/TCILiveSDKDemo)

# 3 修改Appid和AccountType
Appid是应用唯一腾讯云服务的标识。[如何申请Appid](https://www.qcloud.com/doc/product/268/4899)

随心播修改位置
TILLiveSDKShow/TILLiveSDKShow/ConstHeader.h
![](//mc.qcloudimg.com/static/img/78f29b400ff3c1eff1546ade73384dda/image.png)

简单直播修改位置
TCILiveSDKDemo/TCILiveSDKDemo/AppDelegate.m
![](//mc.qcloudimg.com/static/img/720f74698250772e5d1471df78b76892/image.png)


# 4 修改后台地址
目前随心播后台主要用来维护直播房间列表。如果复用随心播客户端代码，需要修改随心播后台地址为业务方自己部署的服务器地址。 <br />     

| 接口| 说明 |
|---------|---------|
| GET_MYROOMID | 获取自己分配的房间号 |
| NEW_ROOM_INFO | 创建新房间 |
| STOP_ROOM | 退出房间 |
| GET_LIVELIST | 获取房间列表 |
| SEND_HEARTBEAT | 房间心跳 |
| GET_COS_SIG | 图片上传相关 |


# 5 编译运行
编译需要增加一系列系统库，以及工程配置
### 5.1 添加系统库
|  需要增加的系统库 |
|------------|
|libc++.tbd|
|libstdc++.tbd|
|libstdc++.6.tbd|
|libz.tbd|
|libbz2.tbd|
|libiconv.tbd|
|libresolv.tbd|
|libsqlite3.tbd|
|libprotobuf.tbd|
|UIKit.framework|
|CoreVideo.framework|
|CoreMedia.framework|
|Accelerate.framework|
|Foundation.framework|
|AVFoundation.framework|
|VideoToolbox.framework|
|CoreGraphics.framework|
|CoreTelephony.framework|
|SystemConfiguration.framework|

### 5.2 工程配置
1、Build Settings/Linking/Other Linker Flags，增加 -ObjC 配置，如下图所示：
![](//mc.qcloudimg.com/static/img/f473f6c580a4196af7d3d33edf140bdb/image.png)

2、Build Settings/Linking/Bitcode，增加 Bitcode 配置，设置为NO，如下图所示:
![](//mc.qcloudimg.com/static/img/f473f6c580a4196af7d3d33edf140bdb/image.png)

若上述步骤均无误，则工程编译可以通过了。
