# 日志上报
开发和用户使用过程中往往会遇到一些问题，为了尽快帮您定位问题，iLiveSDK提供了日志上报接口。主动调用该接口，sdk会自动收集日志并上报。
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

#### 错误码定义
8101 ： 参数错误

8102 ： 文件不存在

8103 ： 压缩失败

8104 ： 获取签名失败

8105 ： 协议解析失败

8106 ： 上传失败

8107 ： 上报结果失败
