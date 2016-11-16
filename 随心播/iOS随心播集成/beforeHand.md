# 准备

# 1.下载SDK

因GitHub有文件大小限制，现将ILiveSDK、IMSDK、AVSDK以及相关Framework上传到腾讯云COS上。 更新时，请到对应的地址进行更新，并添加到工程下面对应的目录下.

Frameworks : http://dldir1.qq.com/hudongzhibo/ILiveSDK/Frameworks.zip 下载后解压，然后再放至对应放到[工程](https://github.com/zhaoyang21cn/ILiveSDK_iOS_Demos)目录 TILLiveSDKShow/


# 2.导入项目
参照 https://github.com/zhaoyang21cn/ILiveSDK_iOS_Demos/blob/master/ILiveSDK-README.md


# 3.修改Appid和AccountType
Appid应用唯一腾讯云服务的标示。[如何申请Appid](https://www.qcloud.com/doc/product/268/4899)

随心播修改位置
TILLiveSDKShow/TILLiveSDKShow/ConstHeader.h
![](http://img.blog.csdn.net/20161116193820944)

简单直播修改位置
TCILiveSDKDemo/TCILiveSDKDemo/AppDelegate.m
![](http://img.blog.csdn.net/20161116194345186)

#4修改后台地址
目前随心播后台主要用来维护直播房间列表。如果复用随心播客户端代码，需要修改随心播后台地址为业务方自己部署的服务器地址。 <br />     

| 接口| 说明 |
|---------|---------|
| GET_MYROOMID | 获取自己分配的房间号 |
| NEW_ROOM_INFO | 创建新房间 |
| STOP_ROOM | 退出房间 |
| GET_LIVELIST | 获取房间列表 |
| SEND_HEARTBEAT | 房间心跳 |
| GET_COS_SIG | 图片上传相关 |


#5编译运行
请参考  https://github.com/zhaoyang21cn/ILiveSDK_iOS_Demos/blob/master/ILiveSDK-README.md
