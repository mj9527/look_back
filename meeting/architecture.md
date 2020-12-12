
# c++11

# 线程管理
- 线程分类：main, io, worker, file, log, watchdog

- looper

- 使用了libuv   

# DataRouter

# xmpp
- 为什么选择xmpp

- 

# xcast
- 视频分三档

# MVVM
## 数据绑定分析
- 属性映射: view使用kWRPropSmsCodeLoginVmBtnLoginInProgress（ResourceProp.h）-> viewmodel 使用 kBtnLoginInProgress(r_prop.h)
- 对应的viewModel : kViewModelSmsCodeLogin(类型枚举)->SmsCodeLoginViewModel(实现类)
- action映射: view 使用kSmsCodeLoginActionLoginSmsCode(ViewModelDefine.h)->kActionLoginSmsCode(view_model_define.h)

# WeMeetPlatform(WMP)
- PluginModule : 插件模块，根据业务属性划分
- PluginInstance : 插件服务实例，每个插件模块可以有多个插件服务实例
- PluginModuleBuilder->PluginModule->PluginInstance

 filters = xc_variant_dict_get(params, "filters");
 WMBeautyLitePanel.m->VideoBeautyModel

 - (void)captureOutput:(AVCaptureOutput *)output
 xc_video_media_frame_output->xc_device_video_frame_output
 xc_device_update(xc_cell_t *c, xc_variant_t *params)->xc_signal_link(c->s_in, dev->transform->s_in)(组建美颜链条)
 int32_t FaceBeauty::DoFaceBeauty(XNNHANDLE ctx, uint8_t* yuv, int32_t width, int32_t height)

 摄像头出来的数据->video_transform_action(裁剪)->


 # 代码流程梳理
VideoModel::InvokeCallPushVideo->
MeetingVideoController::PushVideo->
- 请求视频上行流
PushVideoWithLocalDevice->  Stream::Video::Start("video-out", true)
->VideoStreamListener::OnUpdate(异步启动摄像头)->VideoStreamListener::OnVideoOutAdd

上行推流成功，业务层收到回调
MeetingVideoController::OnVideoStreamUpdate->VideoModel::OnPushVideoComplete->SDKError MeetingDeviceController::Preview->Device::Video::Preview(char const*, bool) 
->异步启动摄像头async_start->dev_start

xcast 摄像头->video_capture_class->video_transform_class->xc_videotoolbox_h264_encoder->video_render_driver_class

流程的串接关键：video_capture_t


# wmp av
MeetingVideoController->video_stream.cc(start)->

# 视频渲染

Stream::Video::Start(char const*, bool)->VideoStreamListener::OnVideoOutAdd(bool)->void MeetingUsersController::OnVideoStreamUpdate

VideoUserCell->RenderView(WMPRender)->RootView(xcast)

关键：
摄像头->video_view.m : - (BOOL)updateWithFrame:(xc_media_frame_t *)frame // 摄像头处理后的数据传递给render

## windows
渲染
- WM_PAINT->root_view_draw->view_draw
关键：
摄像头->video_view.cc : view_update_frame (frame 保存在render_view_t 里面)-> root_view_invalidate(t->root)(提示界面刷新)


# 视频编码

关键：摄像头->capture_done(video_codec_t* codec, xc_media_frame_t* frame) -> on_receive_video_frame(xc_cell_t *c, xc_media_frame_t *frame)
video_stream_encoder->video_encoder->videotoolboxencoder

## window 硬件编码侦测流程
- start_ability_detect->setup_video_ability_environment
- 程序启动后第一次进入会议会开启硬件编码检测, 关键日志disable hw

- 如何快速确定使用了硬编还是软编，使用了哪个显卡的硬编？

- 视频渲染支持硬件加速

读取配置项->优先使用系统硬件侦测配置(历史)->使用文件配置->

## 关键日志
- disable hw detect (存在gpu_detect_crash)
- hw avc d:

硬件显卡日志过滤: intel, nivida

9 - nv12
16 - yuyv
29 - mjpeg

摄像头类型日志video_capture_sink_filter.cc :subtype

# 采集和渲染的串联
1. on_dev_preview 进行采集和渲染的串联
2. 渲染屏蔽xcast_core::set_property(XC_CAMERA_START, src, true), 也就是只启动摄像头
3. 渲染开启xcast_core::set_property(XC_CAMERA_PREVIEW, src, true), 启动摄像头并且预览

# 采集和编码测串联-如何控制是否进行编码
1.  create_encoder: codec.media-encoder
2. stream_encoder_update
4. av_session.cc->stream_tracks.cc -> video_codec.cc->video_stream_encoder.cc->encoder_big, encoder_sml

# 如何控制上限大画面流和小画面流
1.  int32_t rt = xc_cell_fire_signal(is_big ? e->encoder_big : e->encoder_sml, "s_config", vp);

# 编码和发送的绑定
1.  xc_signal_bind(encoder_cell->s_out, (xc_func_pt)on_receive_video_packet, c);