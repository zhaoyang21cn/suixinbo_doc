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

### iOS

#### 1. 设置测速参数
```
SpeedTestRequestParam *param = [[SpeedTestRequestParam alloc] init];
//房间id(普通测速时不需填写，直播结束时测速填写上直播房间id,默认0)
param.roomId = 0;
//通话类型，0:纯音频，1:音视频 默认1 
param.callType = 1;
```

#### 2. 设置测速回调
    设置回调:[ILiveSpeedTestManager shareInstance].delegate = self;
#### 3. 请求测速
    SpeedTestRequestParam *param = [[SpeedTestRequestParam alloc] init];
    [[ILiveSpeedTestManager shareInstance] requestSpeedTest:param succ:^{
        NSLog(@"request speed test succ");
    } fail:^(NSString *module, int errId, NSString *errMsg) {
        NSLog(@"request speed test fail");
    }];
#### 4. 取消测速
    [[ILiveSpeedTestManager shareInstance] cancelSpeedTest:^{
        NSLog(@"cancel speed test succ");
    } fail:^(int code, NSString *msg) {
        NSLog(@"cancel speed test fail");
    }];
#### 5. 测速回调函数
    //开始测速成功
    - (void)onILiveSpeedTestStartSucc;

    //开始测速失败
    - (void)onILiveSpeedTestStartFail:(int)code errMsg:(NSString *)errMsg;

    //测速进度回调
    - (void)onILiveSpeedTestProgress:(SpeedTestProgressItem *)item;

    //测速完成(超时时间30s)
    - (void)onILiveSpeedTestCompleted:(SpeedTestResult *)result code:(int)code msg:(NSString *)msg;
#### 6. 测速结果
```
//本次测速信息和测试结果
@interface SpeedTestResult : NSObject
@property (nonatomic, assign) uint64_t testId;          // 测速id
@property (nonatomic, assign) uint64_t testTime;        //目前填测速结束时的时间
@property (nonatomic, assign) uint64_t clientType;      //0：unknown 1： pc 2： android 3： iphone 4： ipad
@property (nonatomic, assign) uint32_t netType;         //0:无网络；1:wifi；2:2g；3:3g；4:4g；10:wap；255:unknow；
@property (nonatomic, assign) uint32_t netChangeCnt;    //网络变换的次数
@property (nonatomic, assign) uint32_t clientIp;        // 用户ip,申请测速时server返回的clientip
@property (nonatomic, assign) uint32_t callType;        // 通话类型，0:纯音频，1:音视频
@property (nonatomic, assign) uint32_t sdkAppid;        // sdkappid
@property (nonatomic, strong) NSArray *results;         // 测试结果列表SpeedTestResultItem数组
@end

//单台接口机的测试结果
@interface SpeedTestResultItem : NSObject
@property (nonatomic, assign) uint32_t accessIp;        //接口机地址
@property (nonatomic, assign) uint32_t accessPort;      //接口机端口
@property (nonatomic, assign) uint32_t clientIp;        //客户端地址
@property (nonatomic, assign) uint32_t testCnt;         //测速次数
@property (nonatomic, assign) uint32_t upLoss;          //上行丢包率
@property (nonatomic, assign) uint32_t dwLoss;          //下行丢包率
@property (nonatomic, copy)  NSString *accessCountry;   //国家
@property (nonatomic, copy)  NSString *accessProv;      //省份
@property (nonatomic, copy)  NSString *accessIsp;       //运营商
@property (nonatomic, assign) uint32_t testPkgSize;     //测试包包长
@property (nonatomic, assign) uint32_t avgRtt;          //平均延时
@property (nonatomic, assign) uint32_t maxRtt;          //最大延时
@property (nonatomic, assign) uint32_t minRtt;          //最小延时
@property (nonatomic, assign) uint32_t rtt0_50;         //延时在 0-50ms 区间的包个数 [)区间
@property (nonatomic, assign) uint32_t rtt50_100;
@property (nonatomic, assign) uint32_t rtt100_200;
@property (nonatomic, assign) uint32_t rtt200_300;
@property (nonatomic, assign) uint32_t rtt300_700;
@property (nonatomic, assign) uint32_t rtt700_1000;
@property (nonatomic, assign) uint32_t rtt1000;			//延时在 1000ms以上的包个数
@property (nonatomic, assign) uint32_t jitter0_20;      //抖动在 0-20ms区间的个数
@property (nonatomic, assign) uint32_t jitter20_50;
@property (nonatomic, assign) uint32_t jitter50_100;
@property (nonatomic, assign) uint32_t jitter100_200;
@property (nonatomic, assign) uint32_t jitter200_300;
@property (nonatomic, assign) uint32_t jitter300_500;
@property (nonatomic, assign) uint32_t jitter500_800;
@property (nonatomic, assign) uint32_t jitter800;       //抖动在 800ms以上的个数
@property (nonatomic, assign) uint32_t upConsLoss0;     //上行连续丢0个包的包数量(即没有丢包)
@property (nonatomic, assign) uint32_t upConsLoss1;     //上行连续丢1个包的数量
@property (nonatomic, assign) uint32_t upConsLoss2;
@property (nonatomic, assign) uint32_t upConsLoss3;
@property (nonatomic, assign) uint32_t upConsLossb3;    //上行连续丢包超过3个的数量
@property (nonatomic, assign) uint32_t dwConsLoss0;
@property (nonatomic, assign) uint32_t dwConsLoss1;
@property (nonatomic, assign) uint32_t dwConsLoss2;
@property (nonatomic, assign) uint32_t dwConsLoss3;
@property (nonatomic, assign) uint32_t dwConsLossb3;
@property (nonatomic, assign) uint32_t upDisorder;      //上行乱序包的数量
@property (nonatomic, assign) uint32_t dwDisorder;      //下行乱序包的数量
@property (nonatomic, strong) NSArray *upSeqs;          //上行数据包序号
@property (nonatomic, strong) NSArray *dwSeqs;          //下行数据包序号
@end

```
