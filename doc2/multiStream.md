# 一路RTMP+多路上麦混流方案文档

### 需求场景

主播游戏解说、赛事解说、视频解说等需多路混流场景

||||
:-----:|:-----:|:-----:
![](https://mc.qcloudimg.com/static/img/81bbf05065bdbeac8dbbc48e17131e93/2.png)|![](https://mc.qcloudimg.com/static/img/ca04c477a4338b4752b1cf6ab44121cd/3.png)|![](https://mc.qcloudimg.com/static/img/feff9b2df5797d0a9fb82286e62304c0/1.png)
游戏解说|赛事解说|视频分享

### 方案设计

||
:-----:|
![](https://mc.qcloudimg.com/static/img/0e459b3bb1e12af56f7c65df97c4402a/4.png)|


 
> 步骤解读

（1） rtmp视频源以直播码的形式推送rtmp流到直播服务器。[直播码传送门](https://www.qcloud.com/document/product/267/5956)</br>
（2） 连麦者使用直播SDK拉取一路rtmp视频流，使用互动直播SDK上传自己视频流或请求其他上麦者视频流进行互动。[直播SDK传送门](https://www.qcloud.com/document/product/267)和[互动直播SDK传送门](https://github.com/zhaoyang21cn/ILiveSDK_iOS_Suixinbo)
（3） 连麦者手动或自动将互动直播流旁路到直播服务器。[旁路直播传送门](https://www.qcloud.com/document/product/268/8560)
（4） 业务服务器调用混流接口将一路rtmp流和多路互动直播流混成一路输出流。[混流传送门](https://www.qcloud.com/document/product/267/8832)
（5） 观众使用直播SDK拉取一路混合流观看。


> 注意事项

（1）业务服务器延迟调用混流接口：由于互动直播流旁路到直播服务器并生成一路新的rtmp流存在耗时，因此建议业务服务器延迟8秒或以上调用混流接口，否则直播服务器返回无法找到互动直播流的错误。
（2）直播服务器支持推送混合流到第三方CDN，需要提单配置。
