# 关键路径的LOG

*在iLiveSDK 1.0.3以后版本过滤关键字Key_Procedure会搜索到创建房间或加入房间的关键路径*

##创建房间流程正确LOG如下

![](http://mc.qcloudimg.com/static/img/f1ee4994ede1dd89199361973a1cfbee/image.jpg)

* 具体包括以下几个步骤

```
1. 初始化SDK
ILiveSDK:Key_Procedure|initSdk|succ

2. 登录SDK（托管模式，如果是独立模式，无tlsLogin的log）
ILiveLogin:Key_Procedure|tlsLogin|start:id:ken1
ILiveLogin:Key_Procedure|tlsLogin|succ
ILiveLogin:Key_Procedure|iLiveLogin|start:id:ken1
ILiveLogin:Key_Procedure|iLiveLogin|succ

3. 创建渲染根视图和主播渲染视图
ILiveOpenGL:Key_Procedure|createOpenGLView|succ
ILiveOpenGL:Key_Procedure|addRenderView|succ:frame:{{0, 0}, {375, 667}},key:ken1

4. 创建聊天群组
ILiveRoom:Key_Procedure|createIMGroup|start:groupId:9878
ILiveRoom:Key_Procedure|createIMGroup|succ

5. 创建直播房间
ILiveRoom:Key_Procedure|enterAVRoom|start:roomId:9878
ILiveRoom:Key_Procedure|enterAVRoom|succ

6. 打开摄像头并收到server事件（此处是摄像头开启事件）回调
ILiveRoom:Key_Procedure|enableCamera|start:enable:YES
ILiveRoom:Key_Procedure|OnEndpointsUpdateInfo|evenId:3,id:ken1
ILiveRoom:Key_Procedure|enableCamera|succ:enable:YES

7. 收到视频帧（间隔打印）
ILiveGLBase:Key_Procedure|videoFrame|id:ken1,index:1
ILiveGLBase:Key_Procedure|videoFrame|id:ken1,index:11
ILiveGLBase:Key_Procedure|videoFrame|id:ken1,index:21

8. 退出直播房间
ILiveRoom:Key_Procedure|exitAVRoom|start
ILiveRoom:Key_Procedure|exitAVRoom|succ

9 退出聊天群组（群主解散群组）
ILiveRoom:Key_Procedure|quitIMGroup|start:groupId:9878
ILiveRoom:Key_Procedure|quitIMGroup|succ:code:10009
ILiveRoom:Key_Procedure|deleteIMGroup|start:groupId:9878
ILiveRoom:Key_Procedure|deleteIMGroup|succ
```

##加入房间流程正确LOG如下

![](http://mc.qcloudimg.com/static/img/54b586d5fa373c5f1c359d624bf4557a/image.png)

* 具体包括以下几个步骤
```
1. 初始化SDK
ILiveSDK:Key_Procedure|initSdk|succ

2. 登录SDK（托管模式，如果是独立模式，无tlsLogin的log）
ILiveLogin:Key_Procedure|tlsLogin|start:id:ken2
ILiveLogin:Key_Procedure|tlsLogin|succ
ILiveLogin:Key_Procedure|iLiveLogin|start:id:ken2
ILiveLogin:Key_Procedure|iLiveLogin|succ

3. 创建渲染根视图和主播渲染视图
ILiveOpenGL:Key_Procedure|createOpenGLView|succ
ILiveOpenGL:Key_Procedure|addRenderView|succ:frame:{{0, 0}, {414, 736}},key:ken1

4. 加入聊天群组
ILiveRoom:Key_Procedure|joinIMGroup|start:groupId:9878
ILiveRoom:Key_Procedure|joinIMGroup|succ

5. 进入直播房间
ILiveRoom:Key_Procedure|enterAVRoom|start:roomId:9878
ILiveRoom:Key_Procedure|enterAVRoom|succ

6. server事件（此处是摄像头开启事件）回调
ILiveRoom:Key_Procedure|OnEndpointsUpdateInfo|evenId:3,id:ken1

7. 收到主播视频帧（间隔打印）
ILiveGLBase:Key_Procedure|videoFrame|id:ken1,index:1
ILiveGLBase:Key_Procedure|videoFrame|id:ken1,index:11
ILiveGLBase:Key_Procedure|videoFrame|id:ken1,index:21

8. 退出直播房间
ILiveRoom:Key_Procedure|exitAVRoom|start
ILiveRoom:Key_Procedure|exitAVRoom|succ

9 退出聊天群组
ILiveRoom:Key_Procedure|quitIMGroup|start:groupId:9878
ILiveRoom:Key_Procedure|quitIMGroup|succ
```

