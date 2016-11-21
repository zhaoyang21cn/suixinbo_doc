#准备

鉴于Google已经官方声明不再支持eclipse开发，强烈建议你用Android Studio开发。<br/> 


#1.下载SDK
### 方法1 aar方式集成(强烈推荐)<br/>
	如果你使用的Android Studio开发，那么导入iliveSDK非常简单。只需一行代码就可以搞定了 

	compile 'com.tencent.ilivesdk:ilivesdk:X.X.X'  
	
	(X.X.X 替换成对应版本号 比如0.3.7)


### 方法2 传统库类方式集成
* 在腾讯云官网[下载音视频库类](http://www.oracle.com/index.html)


#2 导入项目
* **aar方式集成**,同步完成之后可以在build文件夹中找到ilivesdk文件夹。    
<br /> 
	![](http://mc.qcloudimg.com/static/img/ecd51eab082087cd2049a6a06a84ea76/image.png)
	
		
* **传统库类方式集成**，则需要把so文件和jar包文件分别放到对应jnilibs和libs里面。        
 <br /> 
<img src="http://mc.qcloudimg.com/static/img/e3cc8175676d647dd657beebb11cc2e3/image.png" width = "600" height = "" alt="helloAndroid.png" align=center />     
 <br /> 
 **特别注意：**目前音视频相关库类只支持armeabi架构。[关于多架构的问题总结](https://github.com/zhaoyang21cn/Android_Suixinbo/issues/6)
   

## 库类介绍
从官网上下载的SDK主要包含以下文件夹：

| 文件夹 | 说明 |
|---------|---------|
| docs | 里面放了一些开发文档 包括API更新列表 |
| libs | SDK是以jar和so文件的形式提供给APP使用的，方便普通方式集成和eclipse方式开发 |
| asserts | 音视频核心库类，目前只支持armeabi架构 |


####库类清单
| 序号  | 名称 | 所在文件夹 | 说明 |
|---------|---------|---------|---------|
| 1 | qavsdk.jar | libs\jar | 音视频SDK。|
| 2 | libhwcodec.so | libs\armeabi | 编解码。 |
| 3 | libqav_graphics.so | libs\armeabi | 音视频图形界面。|
| 4 | libqavsdk.so | libs\armeabi |  音视频SDK。|
| 5 | libstlport_shared.so | libs\armeabi | 音视频基础库。 |
| 6 | libTcVpxDec.so | libs\armeabi | 视频组件。|
| 7 | libTcVpxEnc.so | libs\armeabi | 视频组件。 |
| 8 | libtraeimp-armeabi-v7a.so | libs\armeabi | 音频组件。|
| 9 | libxplatform.so | libs\armeabi | 音视频基础库。| 
| 10 | beacon_1.5.3_imsdk_release.jar | libs\jar | 音视频SDK。|
| 11| bugly_1.2.8_imsdk_release.jar | libs\jar | 即时聊天crash上报。
| 12 | imsdk.jar | libs\jar | 即时聊天的SDK。|
| 13 | mobilepb.jar | libs\jar | protouf编解码相关。|
| 14| qalsdk.jar | libs\jar | imsdk网络层。|
| 15 | tls_sdk.jar | libs\jar | 登录相关。|
| 16 | wup-1.0.0-SNAPSHOT.jar | libs\jar | imSdk相关依赖包。|
| 17 | lib_imcore_jni_gyp.so | libs\armeabi |  即时聊天。|
| 18 | libBugly.so | libs\armeabi | crash上报。|
| 19 | libqalcodecwrapper.so | libs\armeabi | qalsdk相关。|
| 20 | libqalmsfboot.so | libs\armeabi | qalsdk相关。|
| 21 | libwtcrypto.so | libs\armeabi | 登录依赖。|
**特别注意：**APP开发者在更新替换SDK的时候，务必要保证以上所有文件的完整性。如果仅局部地替换个别文件，很可能会引入异常。

### 附录
#### [了解更多IMSDK的信息](http://www.qcloud.com/product/im.html)
#### [eclipse开发集成文档](https://github.com/zhaoyang21cn/ILiveSDK_Android_Demos/blob/master/doc/ILiveSDK/eclipse_readme.md) (不推荐)


## 示例参考
 为了方便客户快速集成直播业务，我们在github提供一个[简单直播的示例](https://github.com/zhaoyang21cn/ILiveSDK_Android_Demos)和一个业务级的[应用随心播](https://github.com/zhaoyang21cn/Android_Suixinbo)
 
 <br /> 
	![](http://mc.qcloudimg.com/static/img/9c869853602951b70bf14d29775047b8/image.png)
	
<br /> 
<div align=center>
<img src="http://mc.qcloudimg.com/static/img/0205df956a1d88393b0b850d6ff268f7/image.png" width = "240" height = "320" align=center /> 
<img src="http://mc.qcloudimg.com/static/img/e40a239473b801ad313e0a2b2514f1ff/image.png" width = "240" height = "320" align=center /> 
</div>

#3修改Appid和AccountType
   Appid应用唯一腾讯云服务的标示。[如何申请Appid](https://www.qcloud.com/doc/product/268/4899)
   
   随心播修改位置
   ![](http://mc.qcloudimg.com/static/img/62890dee5794a2ce94404ba762624b94/image.png)
   
   简单直播修改位置
   ![](http://mc.qcloudimg.com/static/img/f1c38008d48d9a33f28a089f57cdb0a5/image.png)

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
   ![](http://mc.qcloudimg.com/static/img/06919328fe28d9088170fc2a6b0f7ee9/image.png)

 
 

#5编译运行
###简单直播配置参考
* 由于目前只支持armeabi架构，如果工程(或依赖库)中有多架构，需要在build.gradle中添加以下配置<br /> （如果包含子工程子工程也要加）
<pre>
android{
    defaultConfig{
        ndk{
            abiFilter 'armeabi'
        }
    }
}
</pre>播配置参考




###随心播配置参考
* 请注意配置jcenter库 腾讯内部是自己的maven<br /> 
  ![](http://mc.qcloudimg.com/static/img/8c4c9bf238499dec32aca993d9ff7ad4/image.png)
* 配置自己的版本号<br /> 
 ![](http://mc.qcloudimg.com/static/img/88beeec46c0fab88f9cf977f6af7f14c/image.png)
* 混淆相关<br /> 

		-keep class com.tencent.**{*;}
		-dontwarn com.tencent.**

		-keep class tencent.**{*;}
		-dontwarn tencent.**

		-keep class qalsdk.**{*;}
		-dontwarn qalsdk.**
* 配置信令服务<br /> 
  ![](http://mc.qcloudimg.com/static/img/afa18e51202e3e80232841d215d90f7b/image.png)
* 申请权限<br /> 
  ![](http://mc.qcloudimg.com/static/img/55db2326bef2d0270ab17e81d945da22/image.png))

    	
	







   
    