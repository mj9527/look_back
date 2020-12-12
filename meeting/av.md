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
