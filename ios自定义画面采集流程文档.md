## 简介 ##
本文档主要针对[AVSDK](https://www.qcloud.com/product/ilvb.html)自定义采集画面流程做详细讲解，自定义采集画面的用途主要用于预处理原始数据，比如用户需要人脸识别，画面美化，动效处理等，如下是通过自定义采集画面后，增加动效效果图：
![](http://img.blog.csdn.net/20160914113734996)
**注：动效效果是用户自己增加的，和本文档无关，用户可以对原始数据做任何预处理（不仅是动效）**


## 流程说明 ##
首先请记住，若要使用自定义采集画面，则采集画面的过程与AVSDK没有任何关系，完全不依赖AVSDK，自定义采集画面的流程中，AVSDK的作用是透传数据以及渲染远程数据。而本文介绍的流程是：***自定义采集画面－>画面传入AVSDK－>远程端收到画面帧渲染*** 的整个过程，流程图如下：
![](http://img.blog.csdn.net/20160914112952279)


## 采集前准备 ##
***进入房间之后，采集画面之前***，
调用	`[videoCtrl enableExternalCapture:YES];`
`videoCtrl`是`QAVVideoCtrl`对象的实例，在此方法返回`QAV_OK`之后才能进行下一步，此方法告诉AVSDK，你需要自定义采集画面，且你将会把采集到的画面传给AVSDK，以AVSDK做画面数据传输通道，用了此方法之后，***不能***再调用AVSDK的打开摄像头接口，且AVSDK的美白，美颜将不再生效。

## 自定义采集 ##
通过`AVCaptureSession`采集画面，实现`AVCaptureVideoDataOutputSampleBufferDelegate`代理，采集到的画面会在
`- (void)captureOutput:(AVCaptureOutput *)captureOutput didOutputSampleBuffer:(CMSampleBufferRef)sampleBuffer fromConnection:(AVCaptureConnection *)connection`
回调中吐出，吐出的是原始画面，用户可以在这里对原始画面做任何预处理，当然，渲染也和AVSDK没有任何关系，本地画面只需要设置`AVCaptureVideoPreviewLayer`即可显示，采集画面和渲染是iOS基础API的调用，不做过多描述。

## 采集后处理 ##
接收到系统回吐出的原始数据（`CMSampleBufferRef`类型数据），用户就可以对其做预处理，比如美白，美颜，人脸识别等，预处理之后的画面需要用户自己完成渲染，与AVSDK无任何关系。

## AVSDK透传 ##
通过上面步骤，得到采集后处理的数据，传入AVSDK需调用接口
` QAVResult result = [videoCtrl fillExternalCaptureFrame:frame];`
`videoCtrl`是`QAVVideoCtrl`对象的实例，`frame`是采集后处理过的数据转换成的`QAVVideoFrame`对象，`result`为`QAV_OK`，说明成功传入AVSDK，否则失败，需要检查前面的流程是否都正确了。

## 远端渲染 ##
AVSDK的回调接口   
`-(void)OnVideoPreview:(QAVVideoFrame*)frameData`
接收远程帧数据，再使用AVSDK的开放类`AVGLBaseView`渲染画面，这里的渲染用户只需要设置一个渲染视图，不需要做额外的操作，详情可参考[随心播](https://github.com/zhaoyang21cn/iOS_Suixinbo)的渲染逻辑。

## 注意事项 ##
1、如果渲染自定义采集的画面使用了OpenGL，则不能使用AVSDK中开放的`AVGLBaseView`作为渲染视图，否则会Crash。也就是说，此时界面上应该有两个view，一个渲染自定义采集的画面，另一个渲染`QAVVideoFrame`对象。
2、转换成`QAVVideoFrame`时，属性`color_format`必需填写`AVCOLOR_FORMAT_NV12`
`srcType`属性必须填写`QAVVIDEO_SRC_TYPE_CAMERA`