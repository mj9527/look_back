# 概念
1. 会议码(外显id):会议码是有可能重复的
2. 会议id
3. PMI
4. PSTN
5. 语音评分:
6. sdkappid: 各端使用的appid各不相同
7. 房间号:
8. 媒体机，流控机，接口机

# 屏幕分享

# 摄像头
## mac 端
- 分辨率720*1280， 采集帧率15，编码帧率12，码率500~1500kbps，格式H264
- 音频格式OPUS，码率45kbps, 包间隔20ms, 每秒50个包，采样率48000，单声道，开启3A


## ios 端
- 采集分辨率640*480， 编码分辨率640*260，采集和编码帧率都是30


## pc 端
- 分辨率1280*720， 采集帧率30， 编码帧率30，码率500~1500kbps 格式H264，硬编没有开启
- 音频格式OPUS，码率45kbps, 包间隔20ms, 每秒50个包，采样率48000，单声道，开启3A

- 虚拟摄像头用的yuyv, yuyv->i420(xcast内部 video_frame_transform)->nv12 (camera_deive)->i420(还原)
- 摄像头输出的是i420, i420->i420(xcast内部 video_frame_transform)->nv12（camera_device)->i420(还原）

## mac 端
super:[25,1280,720,30,0] high:[25,1280,720,30,0] middle:[20,960,540,45,0] low:[15,640,360] vip:[right:0, 15,960,540]|
super:[25,960,540,30,0] high:[25,960,540,30,0] middle:[20,960,540,45,0] low:[15,640,360] vip:[right:0, 15,960,540]|


#
on_video_transform_output(void *data, void *user_data)->

# 摄像头分辨率选择策略
- 分辨率策略： 优先选择比请求分辨率高并且差距最小的分辨率，如果不存在则选择比请求分辨率低并且差距最新的帧率
# 例子1 ：
request : 640*480 rgb24 10
support : 640*460 rgb24 10, 640*500 rgb24, 10
result : 640*500 rgb24 10

# 例子2：
request : 640*480 rgb24 10
support : 640*470 rgb24 10, 640*500 rgb24, 10
result : 640*500 rgb24 10


# 例子3：
request : 640*480 rgb24 10
support : 640*470 rgb24 10, 640*460 rgb24, 10
result : 640*470 rgb24 10

- 帧率策略：优先选择比请求帧率高并且差距最小的帧率，如果不存在则选择比请求帧率低并且差距最小的帧率
# 例子1：
request : 640*480 rgb24 10
support : 640*480 rbg24 15, 640*480 rgb24, 5
result : 640*480 rbg24 15 

# 例子2
request : 640*480 rgb24 10
support : 640*480 rbg24 15, 640*480 rgb24, 12
result : 640*480 rgb24 12,

# 例子 3
request : 640*480 rgb24 10
support : 640*480 rgb24 15, 640*480 rgb24 9,
result : 640*480 rgb24 15,

- 视频格式选择策略：优先选择和请求格式一样的格式，如果不存在优先选择和请求格式统一类型的格式，如果还不存在则选择非mjpg格式
# 例子 1
request: nv12
support: nv21, nv12
result: nv12

# 例子2
request : nv12
support : rgb24, yuyv
result : yuyv

# 例子3：
request : nv12
support : yuyv, nv21
result : yuyv

# 例子4：
request : nv12
support : mjpg, rgb24
result : rgb24

# 腾讯会议采集分辨率策略
- 开启前处理，优先使用nv12, 关闭前处理，还原为原来的格式
- 只有虚拟背景，绿幕，画中画有可能调整帧率
- 

## 场景1
        - SetVideoHD
- 开启摄像头        
    - OnCameraWillPreview （有可能触发cpu 策略的调整）
- SetDefaultFps   
    - OnCameraSetDefaultFps
- SetCustomFps    
    - OnCameraSetCustomFps
        - SetStreamResolutionPolicy
            - SetResolutionPolicy
 模式: large, 不根据cpu 选择分辨率的, 也不受频率控制

 # 场景2           
- OnQuality
    - SetOverloadProtectionPolicy
        - DoOverloadProtectionPolicy
            - SetStreamResolutionPolicyDynamic
                - SetResolutionPolicy
                    - SetDeviceResolutionPolicy
        - SetStreamResolutionPolicyDynamic
                - SetResolutionPolicy
                    - SetDeviceResolutionPolicy            

# 场景3
- 虚拟背景
- 画中画
    - CameraCapturePropertyManagerInstance->SetCameraProperty
            - SetCustomCaptureProperty   
                - OnCameraSetCustomCaptureProperty
- 虚拟背景
- 画中画
- CameraCapturePropertyManager::ClearCameraProperty                
            - SetDefaultCaptureProperty   
                - OnCameraSetDefaultCaptureProperty
                    - SetDeviceResolutionPolicy

# 过载保护策略 - DoOverloadProtectionPolicy
- 默认60秒去一次cpu平均值，cpu 阈值60%
- policy是xcast 的一些功能特性，特性顺序
 - 5 ：kXcastFeatureVideoDenoise
 - 1 ：kXcastFeatureRNNDenoise
 - 2 ：kXcastFeatureMicHowling
 - 8 ：kXcastFeaturePstnBwe
 - 9 ：kXcastFeatureMobaVAD
 - 11 ： kXcastFeatureCNG
 - 12 ： kXcastFeatureCapNoisy
 - 20 ： kXcastFeatureRtvqa
 - 调用分辨率动态调整策略

 # 分辨率动态调整策略 - SetStreamResolutionPolicyDynamic
 - 根据系统cpu 调整分辨率

# 分辨率策略 - SetResolutionPolicy
- 动态模式调整频率控制
- 非动态模式不受频率控制             