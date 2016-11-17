

#自定义音视频输入流
##自定义音频数据

音频本地处理流程图:

> 观众侧
![](https://zhaoyang21cn.github.io/ilivesdk_help/readme_img/audio_member.jpg)

> 主播侧
![](https://zhaoyang21cn.github.io/ilivesdk_help/readme_img/audio_host.jpg)

如上图所示，用户可以其中任意环节对数据进行拦截并作相应的处理。

这里以混音为例:

### Android

1、注册回调

接口名|接口描述
:--|:--:
registAudioDataCallbackWithByteBuffer|注册具体数据类型的回调函数

参数类型|说明
:--|:--:
int|音频数据类型(参考上图)
RegistAudioDataCompleteCallbackWithByteBuffer|指向App定义的音频数据回调函数

```java
ILiveSDK.getInstance().getAvAudioCtrl().registAudioDataCallbackWithByteBuffer(
    AVAudioCtrl.AudioDataSourceType.AUDIO_DATA_SOURCE_MIXTOSEND, mAudioDataCompleteCallbackWithByffer);
```

2、添加需要混入的音频数据

```java
private AVAudioCtrl.RegistAudioDataCompleteCallbackWithByteBuffer mAudioDataCompleteCallbackWithByffer = 
      new AVAudioCtrl.RegistAudioDataCompleteCallbackWithByteBuffer() {
        @Override
        public int onComplete(AVAudioCtrl.AudioFrameWithByteBuffer audioFrameWithByteBuffer, int srcType) {
            if (srcType==AudioDataSourceType.AUDIO_DATA_SOURCE_MIXTOSEND) {
                synchronized (obj){
                  /*************************************************
                    将要混入的音频数据写入audioFrameWithByteBuffer中
                  *************************************************/
                }
            }
            return AVError.AV_OK;
        }
    };
```

### iOS
*(待补充)*

##自定义视频数据

自定义视频数据流程图:

![](https://zhaoyang21cn.github.io/ilivesdk_help/readme_img/custom_flow.png)

### Android

1、打开摄像头，同时需要调用enableExternalCapture做一些准备

接口名|接口描述
:--|:--:
enableExternalCapture|开启/关闭外部视频捕获设备

参数类型|说明
:--|:--:
boolean|true表示开启,false表示关闭
EnableExternalCaptureCompleteCallback|指向App定义的回调函数

```java
ILiveSDK.getInstance().getAvVideoCtrl().enableExternalCapture(false, 
       new AVVideoCtrl.EnableExternalCaptureCompleteCallback(){
                @Override
                protected void onComplete(boolean enable, int result) {
                    super.onComplete(enable, result);
                }
            });
```

2、获取原始视频数据，加工处理

3、上传视频数据

接口名|接口描述
:--|:--:
fillExternalCaptureFrame|输入从外部视频捕获设备获取的视频图像到SDK

参数类型|说明
:--|:--:
byte数组|图像数据
int|图像数据长度
int|图像宽度
int|图像高度
int|图像渲染角度。角度可以是0,90,180,270
int|图像颜色格式。当前仅支持COLOR_FORMAT_I420
int|视频源类型。当前仅支持VIDEO_SRC_TYPE_CAMERA

```java
// 图像需要旋转90度
ILiveSDK.getInstance().getAvVideoCtrl().fillExternalCaptureFrame(data, data.length,
    mCameraSize.width, mCameraSize.height, 270, AVVideoCtrl.COLOR_FORMAT_I420, AVView.VIDEO_SRC_TYPE_CAMERA);
```

### iOS
*(待补充)*
