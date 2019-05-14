# Android

## 一、概述

#### 简介

实时直播SDK能够实现一对一、一对多的纯音频和视频实时直播，相比RTMPC延时更低、极简API接口。适用于在线娃娃机、智能硬件、在线医疗、视频招聘、相亲交友等多种场景。

## 二、集成指南

#### 适用范围

本集成文档适用于Android RTCPEngine SDK 3.0.0 版本。

#### 准备环境

- Android Studio 2.1或以上版本
- Android 版本不低于 4.0.3 且支持音视频的 Android 设备（不支持模拟器）
- Android 设备已经连接到有效网络

#### 导入SDK

**Gradle方式导入（推荐）**

```
implementation 'org.anyrtc:rtcp_kit:3.0.0'

```

**手动导入**

* 前往GitHub[下载Demo](https://github.com/anyRTC/anyRTC-RTCP-Android)，找到**rtcp-release.aar**文件；

* 将rtcp-release.aar文件放入项目的libs目录下，并在build.gradle文件中声明，如下图

![1.png](http://anyrtcboard.oss-cn-beijing.aliyuncs.com/document/20190128150839.png)


#### 权限说明

使用RTCP SDK需以下权限

```
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```
#### 混淆配置
为了避免混淆SDK，在Proguard混淆文件中增加以下配置：

```
-dontwarn org.anyrtc.**
-keep class org.anyrtc.**{*;}
-dontwarn org.webrtc.**
-keep class org.webrtc.**{*;}
```


## 三、API接口文档

### ARRtcpEngine 类

#### 1. 初始化并配置开发者信息

**定义**

```
void initEngine(Context context, String appId, String token)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
context | Context | 上下文对象
appId | String | appId
token | String  | token

**说明**

该方法为配置开发者信息，上述参数均可在https://www.anyrtc.io/ 应用管理中获得；建议在Application调用。

#### 2. 配置私有云

**定义**

```
void configServerForPriCloud(String address,int port)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
address | String | 私有云服务地址
port | int | 私有云服务端口

**说明**

配置私有云信息，当使用私有云时才需要进行配置，默认无需配置。

#### 3. 获取SDK版本号

**定义**

```
String getSdkVersion()
```
**返回值**

SDK版本号

#### 4. 关闭硬解码(安卓特有)

**定义**

```
void disableHWDecode()
```
**说明**  

关闭硬解码,默认打开。

#### 5. 关闭硬编码(安卓特有)

**定义**

```
void disableHWEncode()
```
**说明**  

关闭硬编码,默认关闭。

#### 6. 设置日志显示级别

**定义**

```
void setLogLevel(ARLogLevel logLevel) 
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
logLevel | ARLogLevel | 日志显示级别

---

### ARRtcpOption配置类

#### 1. 获取配置类

**定义**

```
ARRtcpOption rtcpOption = ARRtcpEngine.Inst().getARRtcpOption();
```
#### 2. 设置可配置参数

**定义**
```
void setOptionParams(boolean isDefaultFrontCamera, ARVideoCommon.ARVideoOrientation videoOrientation, ARVideoCommon.ARVideoProfile videoProfile, ARVideoCommon.ARMediaType mediaType) 

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
isDefaultFrontCamera | boolean | 是否默认前置摄像头 true 前置 false 后置  默认true
videoOrientation | ARVideoOrientation |视频方向 默认竖直
videoProfile | ARVideoProfile | 视频分辨率  默认360x640
mediaType | ARMediaType |发布媒体类型 Video音视频 Audio 音频 默认音视频

### ARRtcpKit 类

#### 1. 实例化ARRtcpKit对象

**定义**

```
ARRtcpKit rtcpKit = new ARRtcpKit(String userId,String userData);

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户ID，确保自己平台唯一，不能为空
userData | String |用户自定义信息


#### 2. 设置ARRtcpKit接口回调

**定义**

```
void setRtcpEvent(ARRtcpEvent rtcpEvent)

```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpEvent | ARRtcpEvent | RTCP相关回调实现类


#### 3. 设置本地视频采集窗口

**定义**

```
int setLocalVideoCapturer(long render)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
render | long | 底层视频渲染对象


**返回值**

0/1/2：没有相机权限/打开相机成功/打开相机失败


#### 4. 设置显示其他人的视频窗口

**定义**

```
void setRemoteVideoRender(String rtcpId, long render)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId | String | RTCP服务生成的通道Id (用于标识通道，每次发布随机生成)
render | long |  底层视频渲染对象

**说明**

该方法用于订阅成功通后，视频即将显示的回调中（onRTCOpenVideoRender）使用


#### 5. 设置本地音频是否传输

**定义**

```
void setLocalAudioEnable(boolean enabled)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | boolean| 打开或关闭本地音频传输

**说明**

true为传输音频，false为不传输音频，默认传输

#### 6. 设置本地视频是否传输

**定义**

```
void setLocalVideoEnable(boolean enabled)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | boolean| 打开或关闭本地视频传输

**说明**

true为传输视频，false为不传输视频，默认视频传输

#### 7. 获取本地音频传输是否打开

**定义**

```
 boolean getLocalAudioEnabled()
```

**返回值**

音频传输与否

#### 8. 获取本地视频传输是否打开

**定义**

```
boolean getLocalVideoEnabled()
```

**返回值**

视频传输与否


#### 9. 设置视频竖屏

**定义**

```
void setScreenToPortrait()
```

#### 10. 设置视频横屏

**定义**

```
void setScreenToLandscape()
```

#### 11. 切换前后摄像头

**定义**

```
void switchCamera()
```

#### 12. 设置本地前置摄像头镜像是否打开

**定义**

```
void setFrontCameraMirrorEnable(boolean bEnable)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | boolean | true为打开，alse为关闭 

#### 13. 获取前置摄像头是否镜像

**定义**

```
 boolean getFrontCameraMirror()
```

**返回值**

是否镜像，默认关闭。

#### 14. 不接收某人视频

**定义**

```
 void muteRemoteVideoStream(String rtcpId,boolean mute)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
mute | boolean | true禁止，false接收
rtcpId | String | 流ID


#### 15. 不接收某人音频

**定义**

```
void muteRemoteAudioStream(String rtcpId,  boolean mute)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
mute | boolean | true禁止，false接收
rtcpId | String | 流ID


#### 16. 设置token验证

**定义**

```
boolean setUserToken(String token) 
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userToken | String | token字符串:客户端向自己服务器申请

**说明**

设置token验证必须放在publish、subscribe之前

#### 17. 发布媒体

**定义**

```
int publish(String anyrtcId)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
anyRTCId | String| 该参数可以随意填写，但是不能为空,如果发布成功，SDK会给你分配一个频道ID

**返回值**

发布结果,0/1:发布失败（没有RECORD_AUDIO权限）/发布成功。

#### 18. 取消发布媒体

**定义**

```
 void unPublish()
```

#### 19. 订阅视频

**定义**

```
int subscribe(String rtcpId)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId | String | 订阅视频的频道Id

#### 20. 取消订阅媒体流

**定义**

```
void unSubscribe(String rtcpId)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId | String| 媒体频道Id



#### 22. 停止视频采集

**定义**

```
void stopCapture()
```
**说明**  

停止视频采集

#### 23. 关闭离开

**定义**

```
void clear()
```
**说明**  

停止采集，释放SDK



#### 24. 设置音频检测

**定义**

```
void setAudioActiveCheck(boolean open)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
open | boolean | 是否开启音频检测

**说明**

默认音频检测打开

#### 25. 获取音频检测是否打开

**定义**

```
boolean isOpenAudioCheck()
```
**返回值**

音频检测打开与否


#### 26. 设置视频网络状态是否打开

**定义**

```
void setNetworkStatus(boolean enable)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | boolean | true打开，false关闭

**说明**

默认视频网络状态关闭

#### 27. 获取当前视频网络状态是否打开

**定义**

```
boolean networkStatusEnabled() 
```

**返回值**

视频网络状态检测打开与否

### ARRtcpEvent 回调接口类

#### 1. 发布媒体成功回调

**定义**

```
void onPublishOK(String rtcpId,String liveInfo);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId | String | 通道Id
liveInfo | String | 直播信息

#### 2. 发布媒体失败回调

**定义**

```
void onPublishFailed(int code, String reason);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
code | int | 失败的code值
reason | String | 错误原因

#### 3. 订阅通道成功的回调

**定义**

```
void onSubscribeOK(String rtcpId)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId | String | 通道Id

#### 4. 订阅通道失败的回调

**定义**

```
void onSubscribeFailed(String rtcpId, int code, String reason);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId | String | 通道Id
code | int | 失败的code值
reason | String | 错误原因

#### 5. 订阅后音视频即将显示的回调

**定义**

```
void onRTCOpenRemoteVideoRender(String rtcpId);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId | String | 通道Id

#### 6. 订阅的音视频离开的回调

**定义**

```
void onRTCCloseRemoteVideoRender(String rtcpId);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId | String | 通道Id

#### 7. 订阅音频后成功的回调

**定义**

```
void onRTCOpenRemoteAudioTrack(String rtcpId);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId | String | 通道Id

#### 8. 订阅的音频离开的回调

**定义**

```
void onRTCCloseRemoteAudioTrack(String rtcpId)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId |String | 通道Id

#### 9. 订阅的流 音视频状态回调

**定义**

```
void onRTCRemoteAVStatus(String rtcpId, boolean audio, boolean video);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId |String | 通道Id
audio | boolean | true 音频打开 false 音频关闭
video | boolean | true 视频打开 false 视频关闭

#### 10. 本地RTC音频检测

**定义**

```
void onRTLocalAudioActive(int level, int time);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
level | int | 音频检测音量（0~100）
time | int | 音频检测在time毫秒内不会再回调该方法（单位：毫秒）

**说明**

自己本地音频检测

#### 11. 远程（其他人）RTC音频检测

**定义**

```
void onRTCRemoteAudioActive(String rtcpId, int level, int time);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId |String | 通道Id
level | int | 音频检测音量（0~100）
time | int | 音频检测在time毫秒内不会再回调该方法（单位：毫秒）

**说明**

对方关闭音频传输后（setLocalAudioEnable为false）,该回调将不再回调对方音频状态；对方关闭音频检测后（setAudioActiveCheck为false）,该回调也将不再回调对方音频状态。

#### 12. 本地网络状态

**定义**

```
onRTCLocalNetworkStatus(int netSpeed, int packetLost, ARNetQuality netQuality);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
nNetSpeed | int |网络上行
nPacketLost | int | 丢包率(1~100)
netQuality | ARNetQuality | 网络质量

#### 13. 远程（其他人）网络状态

**定义**

```
onRTCRemoteNetworkStatus(String rtcpId, int netSpeed, int packetLost, ARNetQuality netQuality);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId |String | 通道Id
nNetSpeed | int |网络上行
nPacketLost | int | 丢包率(1~100)
netQuality | ARNetQuality | 网络质量


## 四、更新日志


## 五、错误码对照表

以下为介绍RTCP SDK 的错误码。

名称 | 值            | 备注
---|------------------------------|----
AnyRTC_OK | 0 | 正常
AnyRTC_UNKNOW | 1 | 未知错误
AnyRTC_EXCEPTION | 2 | SDK调用异常
AnyRTC_EXP_UNINIT | 3 | SDK未初始化
AnyRTC_EXP_PARAMS_INVALIDE | 4 | 参数非法
AnyRTC_EXP_NO_NETWORK | 5 | 没有网络链接
AnyRTC_EXP_NOT_FOUND_CAMERA | 6 | 没有找到摄像头设备
AnyRTC_EXP_NO_CAMERA_PERMISSION | 7 | 没有打开摄像头权限
AnyRTC_EXP_NO_AUDIO_PERMISSION | 8 | 没有音频录音权限
AnyRTC_EXP_NOT_SUPPORT_WEBRTC | 9 | 浏览器不支持原生的webrtc
AnyRTC_NET_ERR | 100 | 网络错误 
AnyRTC_NET_DISSCONNECT | 101 | 网络断开
AnyRTC_LIVE_ERR | 102 | 直播出错
AnyRTC_EXP_ERR | 103 | 异常错误
AnyRTC_EXP_Unauthorized | 104 | 服务未授权(仅可能出现在私有云项目)
AnyRTC_BAD_REQ | 201 | 服务不支持的错误请求
AnyRTC_AUTH_FAIL | 202  | 认证失败
AnyRTC_NO_USER | 203 | 此开发者信息不存在
AnyRTC_SVR_ERR | 204 | 服务器内部错误
AnyRTC_SQL_ERR | 205 | 服务器内部数据库错误
AnyRTC_ARREARS | 206 | 账号欠费
AnyRTC_LOCKED | 207 | 账号被锁定
AnyRTC_SERVER_NOT_OPEN | 208 | 服务未开通
AnyRTC_ALLOC_NO_RES | 209 | 没有服务器资源
AnyRTC_SERVER_NOT_SURPPORT | 210 | 不支持的服务
AnyRTC_FORCE_EXIT | 211 | 强制离开

  


