# 音视频测速
测速功能用于测定客户端与腾讯音视频服务器的网速
## 客户端SDK接口
### Android

#### 1. 设置测速参数
```
ILiveSpeedTestRequestParam param = new ILiveSpeedTestRequestParam();
//房间号 不在房间内填0
param.roomId = 0;
//音视频类型 0:纯音频，1:音视频
param.callType = 1;
```

#### 2. 测速回调
```
/**
 * 测速回调
 */

public interface ILiveSpeedTestCallback {



    /** 出错时回调
     * @param code 错误码
     * @param desc 错误描述
     */
    void onError(int code, String desc);

    /**
     *  开始测速
     *
     * @param serverInfoList 测速接口机列表
     */
    void onStart(List<ILiveServerInfo> serverInfoList);


    /**
     *  进度回调，每当开始新的服务器测速时触发
     *
     *  @param serverInfo 当前测试服务器
     *  @param totalPkg 总发测试包数
     *  @param pkgGap 发包间隔
     */
    void onProgress(ILiveServerInfo serverInfo, int totalPkg, int pkgGap);


    /**
     *  测速结束
     *
     * @param results 结果列表
     */
    void onFinish(List<ILiveSpeedTestResult> results);

}

/**
 *  针对一个服务器的测速结果
 */

public interface ILiveSpeedTestResult {


    /**
     *  获取测速目标服务器信息
     */
    ILiveServerInfo getServerInfo();

    /**
     *  获取上行丢包率
     *
     *  @return 上行丢包率（万分比）
     */
    int getUpLoss();

    /**
     *  获取下行丢包率
     *
     *  @return 下行丢包率（万分比）
     */
    int getDownLoss();

    /**
     *  获取平均时延
     *
     *  @return 平均时延，单位毫秒
     */
    int getAvgRtt();

    /**
     *  获取最大时延
     *
     *  @return 最大时延，单位毫秒
     */
    int getMaxRtt();

    /**
     *  获取最小时延
     *
     *  @return 最小时延，单位毫秒
     */
    int getMinRtt();

    /**
     *  获取上行乱序
     *
     *  @return 上行乱序数，服务器收到包的顺序和发送顺序相比较获得的逆序数
     */
    int getUpDisorder();

    /**
     *  获取下行乱序
     *
     *  @return 下行乱序数，客户端收到回包的顺序和发送顺序相比较获得的逆序数
     */
    int getDownDisorder();



}
```

#### 3. 开始测速
每次只允许有一个测速进行。
```
ILiveSpeedTestRequestParam param = new ILiveSpeedTestRequestParam();
ILiveSpeedTestManager.getInstance().requestSpeedTest(param, new ILiveSpeedTestCallback());
```

#### 4. 停止测速
开始测速后，任何时间都可以停止测速。由于多线程的关系，结束测试需要一定的时间，调用结束测速后可能需要一小段时间才能开始下一次测速。
```
ILiveSpeedTestManager.getInstance().stopSpeedTest();
```
