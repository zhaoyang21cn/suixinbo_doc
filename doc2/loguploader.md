# 日志上报

## 功能描述
开发和用户使用过程中往往会遇到一些问题，为了尽快帮您定位问题，iLiveSDK提供了日志上报接口。主动调用日志上报接口，sdk会自动收集日志并压缩上报腾讯云。免去手动获取日志的麻烦。
## 使用场景
- 开发过程中遇到腾讯云相关问题，调用接口上报日志，填写问题描述（如发生时间，现象等），然后反馈给腾讯云客服分析日志
- 在产品上添加日志上报的交互，末端用户使用时出现问题，自动把日志上报到腾讯云

## 客户端SDK接口
### Android

#### 接口定义

```
/**
 * 上报日志
 *
 * @param desc 描述
 * @param callBack 回调
 */
ILiveSDK.getInstance().uploadLog(String desc, ILiveCallBack callback);

```

### IOS

#### 接口定义

```
[[ILiveSDK getInstance] uploadLog:@"日志描述" uploadResult:^(int retCode, NSString *retMsg) {
  if(retCode == ILLU_OK){
    NSLog("上报成功");
  }
  else{
    NSLog("上报失败");
  }
}];

```

#### 错误码定义
0    ： 日志上报成功

8101 ： 参数错误

8102 ： 文件不存在

8103 ： 压缩失败

8104 ： 获取签名失败

8105 ： 协议解析失败

8106 ： 上传失败

8107 ： 上报结果失败

#### 注意
用户未登录也可调用上报日志接口，由于用户id有助于问题的定位，建议登录后上报 
