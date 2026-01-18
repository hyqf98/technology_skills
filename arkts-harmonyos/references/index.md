# ArkTS/HarmonyOS Documentation Index

> 📚 **HarmonyOS Next (HarmonyOS 5/6) 全方位开发文档索引**
>
> 本索引文档涵盖了从入门到精通的 HarmonyOS Next 开发所需的各类资源，包括 ArkTS 语言、ArkUI 框架、应用模型、网络编程、数据存储等内容。
>
> **🔥 最新动态**: HarmonyOS 6 已发布，支持更多 AI 能力和原生智能特性！

---

## 📖 文档导航

### 🚀 快速入门 (Getting Started)

适合 HarmonyOS 初学者的入门教程和示例代码：

#### 基础示例
- [ArkUI 页面跳转示例](getting_started.md#samples/ArkUIWantStartAbility/entry/src/main/ets/pages/Second.ets) - ArkUI 基础页面跳转
- [ArkUI 启动 Ability](getting_started.md#samples/ArkUIWantStartAbility/entry/src/main/ets/pages/Index.ets) - 使用 Want 启动 Ability
- [ArkTS Ability 生命周期](getting_started.md#samples/ArkTSWantStartAbility/entry/src/main/ets/entryability/EntryAbility.ets) - UIAbility 完整生命周期
- [ArkTS 备份恢复](getting_started.md#samples/ArkTSWantStartAbility/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets) - 应用数据备份

#### 进阶教程
- [ArkTS 显式启动 Ability](getting_started.md#samples/ArkTSWantStartAbility/entry/src/main/ets/secondability/SecondAbility.ets) - 第二个 Ability 实现
- [ArkTS 页面导航](getting_started.md#samples/ArkTSWantStartAbility/entry/src/main/ets/pages/Second.ets) - 页面间导航
- [ArkTS 启动示例](getting_started.md#samples/ArkTSWantStartAbility/entry/src/main/ets/pages/Index.ets) - 完整启动示例

#### 核心概念
- **[ArkTS 声明式 UI 入门](getting_started.md#5.ArkTS声明式UI入门)** ⭐
  - ArkUI 框架概述
  - 基础组件与容器组件
  - 布局系统（线性、弹性、相对、栅格、层叠）
  - 状态管理（@State、@Prop、@Link）
  - 自定义组件与构建函数
  - 生命周期管理

---

### 🔤 ArkTS 语言 (ArkTS)

HarmonyOS Next 官方推荐的开发语言，基于 TypeScript 扩展：

#### 🎨 图形与动画
- [SVG 图片操作](arkts.md#【画龙迎春】纯血鸿蒙来画龙！基于HarmonyOS-ArkTS来操作SVG图片) - SVG 图形实战

#### 🎵 音频处理
- [音频采集器 - EntryAbility](arkts.md#samples/ArkTSAudioCapturer/entry/src/main/ets/entryability/EntryAbility.ets)
- [音频备份能力](arkts.md#samples/ArkTSAudioCapturer/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [音频采集页面](arkts.md#samples/ArkTSAudioCapturer/entry/src/main/ets/pages/Index.ets)

#### 🎬 多媒体 UI
- [多图显示应用](arkts.md#samples/ArkTSMultiPictureUI/entry/src/main/ets/entryability/EntryAbility.ets)
- [断点常量配置](arkts.md#samples/ArkTSMultiPictureUI/entry/src/main/ets/constants/BreakpointConstants.ets)
- [操作列表组件](arkts.md#samples/ArkTSMultiPictureUI/entry/src/main/ets/view/ActionList.ets)
- [中心显示区域](arkts.md#samples/ArkTSMultiPictureUI/entry/src/main/ets/view/CenterPart.ets)
- [顶部导航栏](arkts.md#samples/ArkTSMultiPictureUI/entry/src/main/ets/view/TopBar.ets)
- [预览列表](arkts.md#samples/ArkTSMultiPictureUI/entry/src/main/ets/view/PreviewList.ets)
- [多图显示主页](arkts.md#samples/ArkTSMultiPictureUI/entry/src/main/ets/pages/Index.ets)

#### 🎤 音频授权与播放
- [麦克风权限申请](arkts.md#samples/ArkTSUserGrantMicrophone/entry/src/main/ets/entryability/EntryAbility.ets)
- [麦克风权限备份](arkts.md#samples/ArkTSUserGrantMicrophone/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [麦克风使用页面](arkts.md#samples/ArkTSUserGrantMicrophone/entry/src/main/ets/pages/Index.ets)
- [AVPlayer 播放器](arkts.md#samples/ArkTSAVPlayer/entry/src/main/ets/entryability/EntryAbility.ets)
- [AVPlayer 备份](arkts.md#samples/ArkTSAVPlayer/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [AVPlayer 页面](arkts.md#samples/ArkTSAVPlayer/entry/src/main/ets/pages/Index.ets)

#### 🎵 音乐播放器
- [歌曲列表数据](arkts.md#samples/ArkTSMusicPlayer/entry/src/main/ets/viewmodel/SongListData.ets)
- [歌曲数据源](arkts.md#samples/ArkTSMusicPlayer/entry/src/main/ets/viewmodel/SongDataSource.ets)
- [头部组件](arkts.md#samples/ArkTSMusicPlayer/entry/src/main/ets/components/Header.ets)
- [专辑组件](arkts.md#samples/ArkTSMusicPlayer/entry/src/main/ets/components/AlbumComponent.ets)
- [播放器控制](arkts.md#samples/ArkTSMusicPlayer/entry/src/main/ets/components/Player.ets)
- [播放列表](arkts.md#samples/ArkTSMusicPlayer/entry/src/main/ets/components/PlayList.ets)
- [内容区域](arkts.md#samples/ArkTSMusicPlayer/entry/src/main/ets/components/Content.ets)
- [专辑封面](arkts.md#samples/ArkTSMusicPlayer/entry/src/main/ets/components/AlbumCover.ets)
- [菜单数据](arkts.md#samples/ArkTSMusicPlayer/entry/src/main/ets/common/bean/MenuData.ets)
- [歌曲项数据](arkts.md#samples/ArkTSMusicPlayer/entry/src/main/ets/common/bean/SongItem.ets)
- [内容常量](arkts.md#samples/ArkTSMusicPlayer/entry/src/main/ets/common/constants/ContentConstants.ets)
- [网格常量](arkts.md#samples/ArkTSMusicPlayer/entry/src/main/ets/common/constants/GridConstants.ets)
- [播放器常量](arkts.md#samples/ArkTSMusicPlayer/entry/src/main/ets/common/constants/PlayerConstants.ets)
- [头部常量](arkts.md#samples/ArkTSMusicPlayer/entry/src/main/ets/common/constants/HeaderConstants.ets)
- [路由常量](arkts.md#samples/ArkTSMusicPlayer/entry/src/main/ets/common/constants/RouterUrlConstants.ets)
- [歌曲常量](arkts.md#samples/ArkTSMusicPlayer/entry/src/main/ets/common/constants/SongConstants.ets)
- [断点常量](arkts.md#samples/ArkTSMusicPlayer/entry/src/main/ets/common/constants/BreakpointConstants.ets)
- [样式常量](arkts.md#samples/ArkTSMusicPlayer/entry/src/main/ets/common/constants/StyleConstants.ets)
- [断点系统](arkts.md#samples/ArkTSMusicPlayer/entry/src/main/ets/common/media/BreakpointSystem.ets)
- [音乐列表](arkts.md#samples/ArkTSMusicPlayer/entry/src/main/ets/common/media/MusicList.ets)
- [音乐播放器主页](arkts.md#samples/ArkTSMusicPlayer/entry/src/main/ets/pages/Index.ets)

#### 🔐 权限管理
- [通用权限申请](arkts.md#samples/ArkTSUserGrant/entry/src/main/ets/entryability/EntryAbility.ets)
- [通用权限备份](arkts.md#samples/ArkTSUserGrant/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [通用权限页面](arkts.md#samples/ArkTSUserGrant/entry/src/main/ets/pages/Index.ets)

#### 🗣️ 语音识别
- [语音识别字幕](arkts.md#samples/ArkTSSpeechAICaption/entry/src/main/ets/entryability/EntryAbility.ets)
- [语音识别备份](arkts.md#samples/ArkTSSpeechAICaption/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [语音识别页面](arkts.md#samples/ArkTSSpeechAICaption/entry/src/main/ets/pages/Index.ets)

#### 🌐 分布式数据
- [分布式数据应用](arkts.md#samples/ArkTSDistributedData/entry/src/main/ets/entryability/EntryAbility.ets)
- [账户数据](arkts.md#samples/ArkTSDistributedData/entry/src/main/ets/database/AccountData.ets)
- [分布式数据备份](arkts.md#samples/ArkTSDistributedData/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [分布式数据工具](arkts.md#samples/ArkTSDistributedData/entry/src/main/ets/common/DistributedDataUtil.ets)
- [分布式数据页面](arkts.md#samples/ArkTSDistributedData/entry/src/main/ets/pages/Index.ets)

#### 🪟 窗口管理
- [子窗口应用](arkts.md#samples/ArkTSSubWindow/entry/src/main/ets/entryability/EntryAbility.ets)
- [子窗口备份](arkts.md#samples/ArkTSSubWindow/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [子窗口主页](arkts.md#samples/ArkTSSubWindow/entry/src/main/ets/pages/Index.ets)
- [子窗口页面](arkts.md#samples/ArkTSSubWindow/entry/src/main/ets/pages/SubWindowPage.ets)
- [窗口示例页面](arkts.md#samples/ArkTSWindow/entry/src/main/ets/pages/Second.ets)
- [窗口主页](arkts.md#samples/ArkTSWindow/entry/src/main/ets/pages/Index.ets)

#### 🎤 核心语音
- [语音识别器](arkts.md#samples/ArkTSCoreSpeechSpeechRecognizer/entry/src/main/ets/entryability/EntryAbility.ets)
- [语音识别备份](arkts.md#samples/ArkTSCoreSpeechSpeechRecognizer/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [音频采集服务](arkts.md#samples/ArkTSCoreSpeechSpeechRecognizer/entry/src/main/ets/service/AudioCapturer.ets)
- [语音识别页面](arkts.md#samples/ArkTSCoreSpeechSpeechRecognizer/entry/src/main/ets/pages/Index.ets)

#### 🔗 页面路由
- [第二个页面](arkts.md#samples/ArkTSPagesRouter/entry/src/main/ets/pages/Second.ets)
- [路由主页](arkts.md#samples/ArkTSPagesRouter/entry/src/main/ets/pages/Index.ets)

#### 🔗 打开链接
- [打开链接应用](arkts.md#samples/ArkTSOpenLink/entry/src/main/ets/entryability/EntryAbility.ets)
- [打开链接备份](arkts.md#samples/ArkTSOpenLink/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [打开链接页面](arkts.md#samples/ArkTSOpenLink/entry/src/main/ets/pages/Index.ets)

#### 📊 数据可视化
- [CPI 图表数据](arkts.md#samples/ArkTSCPIChart/entry/src/main/ets/model/CPIData.ets)
- [CPI 图表页面](arkts.md#samples/ArkTSCPIChart/entry/src/main/ets/pages/Index.ets)

#### ⚡ 原子化服务
- [原子化服务卡片](arkts.md#samples/ArkTSAtomicService/entry/src/main/ets/widget/pages/WidgetCard.ets)
- [原子化服务页面](arkts.md#samples/ArkTSAtomicService/entry/src/main/ets/pages/Index.ets)

#### 🎠 轮播动画
- [Swiper 动画模式](arkts.md#samples/ArkTSSwiperAnimationMode/entry/src/main/ets/entryability/EntryAbility.ets)
- [Swiper 动画备份](arkts.md#samples/ArkTSSwiperAnimationMode/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [Swiper 动画页面](arkts.md#samples/ArkTSSwiperAnimationMode/entry/src/main/ets/pages/Index.ets)

#### 📶 WiFi 管理
- [WiFi 连接管理](arkts.md#samples/ArkTSWifiManagerConnectToWifi/entry/src/main/ets/entryability/EntryAbility.ets)
- [WiFi 连接备份](arkts.md#samples/ArkTSWifiManagerConnectToWifi/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [WiFi 连接页面](arkts.md#samples/ArkTSWifiManagerConnectToWifi/entry/src/main/ets/pages/Index.ets)

#### 🎬 视频播放器
- [视频列表模型](arkts.md#samples/ArkTSVideoPlayer/entry/src/main/ets/viewmodel/HomeVideoListModel.ets)
- [对话框模型](arkts.md#samples/ArkTSVideoPlayer/entry/src/main/ets/viewmodel/HomeDialogModel.ets)
- [视频控制器](arkts.md#samples/ArkTSVideoPlayer/entry/src/main/ets/controller/VideoController.ets)
- [屏幕工具](arkts.md#samples/ArkTSVideoPlayer/entry/src/main/ets/common/util/ScreenUtil.ets)
- [日期格式化](arkts.md#samples/ArkTSVideoPlayer/entry/src/main/ets/common/util/DateFormatUtil.ets)
- [日志工具](arkts.md#samples/ArkTSVideoPlayer/entry/src/main/ets/common/util/Logger.ets)
- [视频数据 Bean](arkts.md#samples/ArkTSVideoPlayer/entry/src/main/ets/common/bean/VideoBean.ets)
- [播放常量](arkts.md#samples/ArkTSVideoPlayer/entry/src/main/ets/common/constants/PlayConstants.ets)
- [通用常量](arkts.md#samples/ArkTSVideoPlayer/entry/src/main/ets/common/constants/CommonConstants.ets)
- [主页常量](arkts.md#samples/ArkTSVideoPlayer/entry/src/main/ets/common/constants/HomeConstants.ets)
- [播放进度](arkts.md#samples/ArkTSVideoPlayer/entry/src/main/ets/view/PlayProgress.ets)
- [播放器界面](arkts.md#samples/ArkTSVideoPlayer/entry/src/main/ets/view/PlayPlayer.ets)
- [列表项](arkts.md#samples/ArkTSVideoPlayer/entry/src/main/ets/view/HomeTabContentListItem.ets)
- [播放标题](arkts.md#samples/ArkTSVideoPlayer/entry/src/main/ets/view/PlayTitle.ets)
- [标签内容](arkts.md#samples/ArkTSVideoPlayer/entry/src/main/ets/view/HomeTabContent.ets)
- [标题对话框](arkts.md#samples/ArkTSVideoPlayer/entry/src/main/ets/view/PlayTitleDialog.ets)
- [内容对话框](arkts.md#samples/ArkTSVideoPlayer/entry/src/main/ets/view/HomeTabContentDialog.ets)
- [内容列表](arkts.md#samples/ArkTSVideoPlayer/entry/src/main/ets/view/HomeTabContentList.ets)
- [播放控制](arkts.md#samples/ArkTSVideoPlayer/entry/src/main/ets/view/PlayControl.ets)
- [标签按钮](arkts.md#samples/ArkTSVideoPlayer/entry/src/main/ets/view/HomeTabContentButton.ets)
- [播放页面](arkts.md#samples/ArkTSVideoPlayer/entry/src/main/ets/pages/PlayPage.ets)
- [主页](arkts.md#samples/ArkTSVideoPlayer/entry/src/main/ets/pages/HomePage.ets)

#### 🛒 购物车
- [购物车应用](arkts.md#samples/ArkTSShoppingCart/entry/src/main/ets/entryability/EntryAbility.ets)
- [购物车备份](arkts.md#samples/ArkTSShoppingCart/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [购物车页面](arkts.md#samples/ArkTSShoppingCart/entry/src/main/ets/pages/Index.ets)

#### 🌐 HTTP 请求
- [HTTP 应用](arkts.md#samples/ArkTSHttp/entry/src/main/ets/entryability/EntryAbility.ets)
- [HTTP 备份](arkts.md#samples/ArkTSHttp/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [HTTP 页面](arkts.md#samples/ArkTSHttp/entry/src/main/ets/pages/Index.ets)

#### ⚙️ 系统设置
- [打开应用管理](arkts.md#samples/ArkTSWantOpenManageApplications/entry/src/main/ets/pages/Index.ets)

#### 📡 事件发送
- [Emitter 应用](arkts.md#samples/ArkTSEmitter/entry/src/main/ets/entryability/EntryAbility.ets)
- [Emitter 备份](arkts.md#samples/ArkTSEmitter/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [Emitter 页面](arkts.md#samples/ArkTSEmitter/entry/src/main/ets/pages/Index.ets)

#### 📍 位置服务
- [位置管理应用](arkts.md#samples/ArkTSGeoLocationManager/entry/src/main/ets/entryability/EntryAbility.ets)
- [位置管理备份](arkts.md#samples/ArkTSGeoLocationManager/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [位置管理页面](arkts.md#samples/ArkTSGeoLocationManager/entry/src/main/ets/pages/Index.ets)

#### 🧩 基础组件
- [基础组件应用](arkts.md#samples/ArkUIBasicComponents/entry/src/main/ets/entryability/EntryAbility.ets)
- [基础组件备份](arkts.md#samples/ArkUIBasicComponents/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [基础组件页面](arkts.md#samples/ArkUIBasicComponents/entry/src/main/ets/pages/Index.ets)

#### 🖼️ 图片编解码
- [图片编解码页面](arkts.md#samples/ArkTSImageCodec/entry/src/main/ets/pages/Index.ets)

#### 🌐 Web 组件
- [Web HTML 应用](arkts.md#samples/ArkTSWebComponentHTML/entry/src/main/ets/entryability/EntryAbility.ets)
- [Web HTML 备份](arkts.md#samples/ArkTSWebComponentHTML/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [Web HTML 页面](arkts.md#samples/ArkTSWebComponentHTML/entry/src/main/ets/pages/Index.ets)

#### 📍 指示器
- [指示器应用](arkts.md#samples/ArkTSIndicator/entry/src/main/ets/entryability/EntryAbility.ets)
- [指示器备份](arkts.md#samples/ArkTSIndicator/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [指示器页面](arkts.md#samples/ArkTSIndicator/entry/src/main/ets/pages/Index.ets)

#### 🌐 Web 组件基础
- [Web 组件应用](arkts.md#samples/ArkTSWebComponent/entry/src/main/ets/entryability/EntryAbility.ets)
- [Web 组件备份](arkts.md#samples/ArkTSWebComponent/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [Web 组件页面](arkts.md#samples/ArkTSWebComponent/entry/src/main/ets/pages/Index.ets)

#### 💾 关系型数据库
- [RDB 应用](arkts.md#samples/ArkTSRdb/entry/src/main/ets/entryability/EntryAbility.ets)
- [账户数据](arkts.md#samples/ArkTSRdb/entry/src/main/ets/viewmodel/AccountData.ets)
- [RDB 备份](arkts.md#samples/ArkTSRdb/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [RDB 工具](arkts.md#samples/ArkTSRdb/entry/src/main/ets/common/database/Rdb.ets)
- [账户表](arkts.md#samples/ArkTSRdb/entry/src/main/ets/common/database/tables/AccountTable.ets)
- [通用常量](arkts.md#samples/ArkTSRdb/entry/src/main/ets/common/constants/CommonConstants.ets)
- [RDB 页面](arkts.md#samples/ArkTSRdb/entry/src/main/ets/pages/Index.ets)

#### 📢 公共事件
- [公共事件应用](arkts.md#samples/ArkTSCommonEventService/entry/src/main/ets/entryability/EntryAbility.ets)
- [公共事件备份](arkts.md#samples/ArkTSCommonEventService/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [公共事件页面](arkts.md#samples/ArkTSCommonEventService/entry/src/main/ets/pages/Index.ets)

#### 🧭 导航组件
- [导航应用](arkts.md#samples/ArkTSNavigation/entry/src/main/ets/entryability/EntryAbility.ets)
- [导航备份](arkts.md#samples/ArkTSNavigation/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [第一页](arkts.md#samples/ArkTSNavigation/entry/src/main/ets/pages/PageOne.ets)
- [导航主页](arkts.md#samples/ArkTSNavigation/entry/src/main/ets/pages/Index.ets)
- [第二页](arkts.md#samples/ArkTSNavigation/entry/src/main/ets/pages/PageTwo.ets)

#### 🪟 全屏布局
- [全屏布局应用](arkts.md#samples/ArkTSWindowLayoutFullScreen/entry/src/main/ets/entryability/EntryAbility.ets)
- [全屏布局备份](arkts.md#samples/ArkTSWindowLayoutFullScreen/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [全屏布局页面](arkts.md#samples/ArkTSWindowLayoutFullScreen/entry/src/main/ets/pages/Index.ets)

#### 💾 首选项
- [首选项应用](arkts.md#samples/ArkTSPreferences/entry/src/main/ets/entryability/EntryAbility.ets)
- [账户数据](arkts.md#samples/ArkTSPreferences/entry/src/main/ets/database/AccountData.ets)
- [首选项备份](arkts.md#samples/ArkTSPreferences/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [首选项工具](arkts.md#samples/ArkTSPreferences/entry/src/main/ets/common/PreferencesUtil.ets)
- [首选项页面](arkts.md#samples/ArkTSPreferences/entry/src/main/ets/pages/Index.ets)

#### 🐉 SVG 图形
- [SVG 中国龙](arkts.md#samples/ArkTSSVGChineseLoong/entry/src/main/ets/pages/Index.ets)

#### ⚙️ 打开设置
- [打开设置应用](arkts.md#samples/ArkTSWantOpenSetting/entry/src/main/ets/entryability/EntryAbility.ets)
- [打开设置备份](arkts.md#samples/ArkTSWantOpenSetting/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [打开设置页面](arkts.md#samples/ArkTSWantOpenSetting/entry/src/main/ets/pages/Index.ets)

#### 👋 Hello World
- [Hello World 应用](arkts.md#samples/ArkTSHelloWorld/entry/src/main/ets/entryability/EntryAbility.ets)
- [Hello World 备份](arkts.md#samples/ArkTSHelloWorld/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [Hello World 页面](arkts.md#samples/ArkTSHelloWorld/entry/src/main/ets/pages/Index.ets)

#### 多图应用
- [构建配置](arkts.md#samples/ArkTSMultiPicture/commons/base/BuildProfile.ets)
- [基础索引](arkts.md#samples/ArkTSMultiPicture/commons/base/Index.ets)
- [基础常量](arkts.md#samples/ArkTSMultiPicture/commons/base/src/main/ets/constants/BaseConstants.ets)
- [断点常量](arkts.md#samples/ArkTSMultiPicture/commons/base/src/main/ets/constants/BreakpointConstants.ets)
- [断点类型](arkts.md#samples/ArkTSMultiPicture/commons/base/src/main/ets/utils/BreakpointType.ets)
- [图片视图构建](arkts.md#samples/ArkTSMultiPicture/features/pictureView/BuildProfile.ets)
- [图片视图索引](arkts.md#samples/ArkTSMultiPicture/features/pictureView/Index.ets)
- [自适应模型](arkts.md#samples/ArkTSMultiPicture/features/pictureView/src/main/ets/viewmodel/Adaptive.ets)
- [图片视图常量](arkts.md#samples/ArkTSMultiPicture/features/pictureView/src/main/ets/constants/PictureViewConstants.ets)
- [底部栏](arkts.md#samples/ArkTSMultiPicture/features/pictureView/src/main/ets/view/BottomBar.ets)
- [中心部分](arkts.md#samples/ArkTSMultiPicture/features/pictureView/src/main/ets/view/CenterPart.ets)
- [顶部栏](arkts.md#samples/ArkTSMultiPicture/features/pictureView/src/main/ets/view/TopBar.ets)
- [预览列表](arkts.md#samples/ArkTSMultiPicture/features/pictureView/src/main/ets/view/PreviewList.ets)
- [图片视图索引页](arkts.md#samples/ArkTSMultiPicture/features/pictureView/src/main/ets/pages/PictureViewIndex.ets)
- [默认能力](arkts.md#samples/ArkTSMultiPicture/product/default/src/main/ets/defaultability/DefaultAbility.ets)
- [默认页面](arkts.md#samples/ArkTSMultiPicture/product/default/src/main/ets/pages/Index.ets)

#### 🎉 背景与总结
- [HarmonyOS 开发背景](arkts.md#背景)

---

### 🎨 ArkUI 框架 (ArkUI)

HarmonyOS Next 的声明式 UI 框架：

#### 🌟 特色应用
- **[HarmonyOS 调用三方库 PhotoView](arkui.md#HarmonyOS体验官【挑战赛第二期】用HarmonyOS-ArkUI调用三方库PhotoView实现图片的联播、缩放)** - 第三方库集成实战
- **[健康饮食应用开发](arkui.md#用HarmonyOS-ArkUI来开发一个健康饮食应用)** - 完整应用开发案例
- **[中秋国庆祝福程序](arkui.md#基于HarmonyOS-ArkTS中秋国庆祝福程序)** - 节日主题应用
- **[七夕壁纸轮播](arkui.md#七夕壁纸轮播)** - 轮播图效果实现
- **[抽盲盒头像](arkui.md#用HarmonyOS-ArkUI抽个盲盒头像)** - 趣味应用开发

#### 📺 媒体组件
- [媒体组件应用](arkui.md#samples/ArkUIMediaComponents/entry/src/main/ets/entryability/EntryAbility.ets)
- [媒体组件备份](arkui.md#samples/ArkUIMediaComponents/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [媒体组件页面](arkui.md#samples/ArkUIMediaComponents/entry/src/main/ets/pages/Index.ets)

#### 🎠 轮播组件
- [Swiper 应用](arkui.md#samples/ArkUISwiper/entry/src/main/ets/entryability/EntryAbility.ets)
- [Swiper 备份](arkui.md#samples/ArkUISwiper/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [Swiper 页面](arkui.md#samples/ArkUISwiper/entry/src/main/ets/pages/Index.ets)

#### 🔗 打开 URI
- [打开 URI 页面](arkui.md#samples/ArkUIWantOpenURI/entry/src/main/ets/pages/Index.ets)

#### 自动播放
- [自动播放应用](arkui.md#samples/EtsUISwiperAutoPlay/entry/src/main/ets/default/app.ets)
- [图片数据](arkui.md#samples/EtsUISwiperAutoPlay/entry/src/main/ets/default/model/imageData.ets)
- [自动播放页面](arkui.md#samples/EtsUISwiperAutoPlay/entry/src/main/ets/default/pages/index.ets)

#### 基础轮播
- [基础应用](arkui.md#samples/EtsUISwiper/entry/src/main/ets/default/app.ets)
- [基础页面](arkui.md#samples/EtsUISwiper/entry/src/main/ets/default/pages/index.ets)

#### 三方库集成
- [外部组件](arkui.md#samples/ArkUIThirdPartyLibrary/entry/src/main/ets/view/OuterComponent.ets)
- [内部组件](arkui.md#samples/ArkUIThirdPartyLibrary/entry/src/main/ets/view/InnerComponent.ets)
- [三方库主页](arkui.md#samples/ArkUIThirdPartyLibrary/entry/src/main/ets/pages/Index.ets)

#### 健康饮食
- [模拟数据](arkui.md#samples/ArkUIHealthyDiet/entry/src/main/ets/mock/MockData.ets)
- [数据工具](arkui.md#samples/ArkUIHealthyDiet/entry/src/main/ets/model/DataUtil.ets)
- [数据模型](arkui.md#samples/ArkUIHealthyDiet/entry/src/main/ets/model/DataModels.ets)
- [健康饮食主页](arkui.md#samples/ArkUIHealthyDiet/entry/src/main/ets/pages/Index.ets)
- [食物详情](arkui.md#samples/ArkUIHealthyDiet/entry/src/main/ets/pages/FoodDetail.ets)

#### 画布组件
- [画布应用](arkui.md#samples/ArkUICanvasComponents/entry/src/main/ets/entryability/EntryAbility.ets)
- [画布备份](arkui.md#samples/ArkUICanvasComponents/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [画布页面](arkui.md#samples/ArkUICanvasComponents/entry/src/main/ets/pages/Index.ets)

#### 节日应用
- [中秋节日](arkui.md#samples/ArkUIMidAutumnFestival/entry/src/main/ets/pages/Index.ets)

#### 页面路由
- [第二个页面](arkui.md#samples/ArkUIPagesRouter/entry/src/main/ets/pages/Second.ets)
- [路由主页](arkui.md#samples/ArkUIPagesRouter/entry/src/main/ets/pages/Index.ets)

#### 爱心表达
- [爱心主页](arkui.md#samples/ArkUIExpressingLove/entry/src/main/ets/pages/Index.ets)

#### 仿微信应用
- [微信应用](arkui.md#samples/ArkUIWeChat/entry/src/main/ets/entryability/EntryAbility.ets)
- [微信备份](arkui.md#samples/ArkUIWeChat/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [通用样式](arkui.md#samples/ArkUIWeChat/entry/src/main/ets/model/CommonStyle.ets)
- [微信数据](arkui.md#samples/ArkUIWeChat/entry/src/main/ets/model/WeChatData.ets)
- [用户数据](arkui.md#samples/ArkUIWeChat/entry/src/main/ets/model/Person.ets)
- [通讯录页面](arkui.md#samples/ArkUIWeChat/entry/src/main/ets/pages/ContactPage.ets)
- [发现页面](arkui.md#samples/ArkUIWeChat/entry/src/main/ets/pages/DiscoveryPage.ets)
- [聊天页面](arkui.md#samples/ArkUIWeChat/entry/src/main/ets/pages/ChatPage.ets)
- [微信主页](arkui.md#samples/ArkUIWeChat/entry/src/main/ets/pages/Index.ets)
- [我的页面](arkui.md#samples/ArkUIWeChat/entry/src/main/ets/pages/MyPage.ets)

#### 容器组件
- [容器应用](arkui.md#samples/ArkUIContainerComponents/entry/src/main/ets/entryability/EntryAbility.ets)
- [容器备份](arkui.md#samples/ArkUIContainerComponents/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [导航示例](arkui.md#samples/ArkUIContainerComponents/entry/src/main/ets/pages/NavigatorExample.ets)
- [返回示例](arkui.md#samples/ArkUIContainerComponents/entry/src/main/ets/pages/BackExample.ets)
- [详情示例](arkui.md#samples/ArkUIContainerComponents/entry/src/main/ets/pages/DetailExample.ets)
- [容器主页](arkui.md#samples/ArkUIContainerComponents/entry/src/main/ets/pages/Index.ets)

#### 登录界面
- [登录应用](arkui.md#samples/ArkUILogin/entry/src/main/ets/entryability/EntryAbility.ets)
- [登录备份](arkui.md#samples/ArkUILogin/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [登录页面](arkui.md#samples/ArkUILogin/entry/src/main/ets/pages/Index.ets)

#### 计算器
- [计算器逻辑](arkui.md#samples/ArkUICalculator/entry/src/main/ets/Calculator.ets)
- [按钮信息](arkui.md#samples/ArkUICalculator/entry/src/main/ets/CalculatorButtonInfo.ets)
- [计算器应用](arkui.md#samples/ArkUICalculator/entry/src/main/ets/entryability/EntryAbility.ets)
- [计算器备份](arkui.md#samples/ArkUICalculator/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [计算器页面](arkui.md#samples/ArkUICalculator/entry/src/main/ets/pages/Index.ets)

#### 应用管理
- [打开应用管理](arkui.md#samples/ArkUIWantOpenManageApplications/entry/src/main/ets/pages/Index.ets)

#### 购物应用
- [购物应用](arkui.md#samples/ArkUIShopping/entry/src/main/ets/entryability/EntryAbility.ets)
- [购物备份](arkui.md#samples/ArkUIShopping/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [菜单数据](arkui.md#samples/ArkUIShopping/entry/src/main/ets/model/Menu.ets)
- [商品数据](arkui.md#samples/ArkUIShopping/entry/src/main/ets/model/GoodsData.ets)
- [商品数据模型](arkui.md#samples/ArkUIShopping/entry/src/main/ets/model/GoodsDataModels.ets)
- [购物详情](arkui.md#samples/ArkUIShopping/entry/src/main/ets/pages/ShoppingDetail.ets)
- [购物主页](arkui.md#samples/ArkUIShopping/entry/src/main/ets/pages/Index.ets)
- [我的页面](arkui.md#samples/ArkUIShopping/entry/src/main/ets/pages/MyPage.ets)
- [购物车](arkui.md#samples/ArkUIShopping/entry/src/main/ets/pages/ShoppingCart.ets)

#### 体验应用
- [NFT 响应](arkui.md#samples/ArkUIExperience/entry/src/main/ets/common/bean/NftResponse.ets)
- [常量定义](arkui.md#samples/ArkUIExperience/entry/src/main/ets/common/constants/Constants.ets)
- [日志工具](arkui.md#samples/ArkUIExperience/entry/src/main/ets/common/utils/LogUtil.ets)
- [信息对话框](arkui.md#samples/ArkUIExperience/entry/src/main/ets/view/InfoDialog.ets)
- [输入组件](arkui.md#samples/ArkUIExperience/entry/src/main/ets/view/Input.ets)
- [错误对话框](arkui.md#samples/ArkUIExperience/entry/src/main/ets/view/ErrorDialog.ets)
- [体验主页](arkui.md#samples/ArkUIExperience/entry/src/main/ets/pages/Index.ets)
- [NFT 页面](arkui.md#samples/ArkUIExperience/entry/src/main/ets/pages/Nft.ets)

#### 绘制组件
- [绘制应用](arkui.md#samples/ArkUIDrawingComponents/entry/src/main/ets/entryability/EntryAbility.ets)
- [绘制备份](arkui.md#samples/ArkUIDrawingComponents/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [绘制页面](arkui.md#samples/ArkUIDrawingComponents/entry/src/main/ets/pages/Index.ets)

#### Hello World
- [Hello 应用](arkui.md#samples/ArkUIHelloWorld/entry/src/main/ets/entryability/EntryAbility.ets)
- [Hello 备份](arkui.md#samples/ArkUIHelloWorld/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)
- [Hello 页面](arkui.md#samples/ArkUIHelloWorld/entry/src/main/ets/pages/Index.ets)

#### 高级特性
- **[属性动画详解](arkui.md#属性动画)** - 动画系统完整教程
- **[UIAbility 详解](arkui.md#UIAbility)** - Ability 组件深入解析
- **[常用组件大全](arkui.md#6.常用组件)** - 组件使用指南

---

### 🏗️ 应用模型 (Application Model)

HarmonyOS Next 的应用架构和生命周期管理：

#### 🔢 字符统计
- [字符统计 Ability](application_model.md#samples/CountTheNumberOfCharacters/entry/src/main/ets/entryability/EntryAbility.ets)
- [字符统计备份](application_model.md#samples/CountTheNumberOfCharacters/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)

#### 🤖 AI 扫描
- [AI 扫描 Ability](application_model.md#samples/AIScanner/entry/src/main/ets/entryability/EntryAbility.ets)
- [AI 扫描备份](application_model.md#samples/AIScanner/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)

#### 📅 打卡应用
- [打卡 Ability](application_model.md#samples/WeLinkPunchCard/entry/src/main/ets/entryability/EntryAbility.ets)
- [打卡备份](application_model.md#samples/WeLinkPunchCard/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets)

---

### 🌐 网络编程 (Network)

网络请求与数据处理：

- **[10. 网络请求详解](network.md#10.网络请求.md)** ⭐
  - HTTP 请求基础
  - 网络权限配置
  - 数据解析与处理
  - 错误处理最佳实践

---

### 💾 数据存储 (Data)

应用数据管理与持久化：

- **点赞数据模型**
  - [图片数据](data.md#samples/GiveThumbsUp/entry/src/main/ets/model/ImageData.ets)

- **状态管理**
  - **[13. 应用状态管理 Storage](data.md#13.应用状态管理Storage)** ⭐
    - 应用级状态管理
    - LocalStorage 使用
    - 状态共享机制

- **数据持久化**
  - **[首选项详解](data.md#首选项)** ⭐
    - 轻量级数据存储
    - 数据读写操作
    - 数据同步机制

---

### 📱 示例代码 (Samples)

丰富的实战示例集合：

#### 🎯 特色示例
- **[挑战赛：点赞美女翻牌动效](samples.md#【挑战赛第三期】用HarmonyOS-ArkUI实现点赞美女翻牌动效)** ⭐
  - 动画效果实现
  - 交互设计
  - 完整源码

#### 🎮 实用示例
- [点赞翻牌实现](samples.md#samples/GiveThumbsUp/entry/src/main/ets/pages/Index.ets)
- [字符计数器](samples.md#samples/CountTheNumberOfCharacters/entry/src/main/ets/pages/Index.ets)
- [父亲节祝福](samples.md#samples/FatherDay/entry/src/main/ets/default/app.ets)
- [父亲节页面](samples.md#samples/FatherDay/entry/src/main/ets/default/pages/index.ets)

#### 🤖 AI 能力
- [身份证识别](samples.md#samples/AIScanner/entry/src/main/ets/pages/PageIdCard.ets)
- [AI 扫描主页](samples.md#samples/AIScanner/entry/src/main/ets/pages/Index.ets)
- [银行卡识别](samples.md#samples/AIScanner/entry/src/main/ets/pages/PageBankCard.ets)
- [文档扫描](samples.md#samples/AIScanner/entry/src/main/ets/pages/PageDocScan.ets)

#### 📅 打卡应用
- [打卡主页](samples.md#samples/WeLinkPunchCard/entry/src/main/ets/pages/Index.ets)

---

### 📚 其他资源 (Other)

扩展学习资源和工具：

#### 📖 完整教程
- **[HarmonyOS Tutorial 系列教程](other.md#HarmonyOS-Tutorial.-《跟老卫学HarmonyOS开发》《鸿蒙HarmonyOS手机应用开发实战》《鸿蒙HarmonyOS应用开发从入门到精通》《鸿蒙HarmonyOS应用开发入门》《鸿蒙之光HarmonyOS-NEXT原生应用开发入门》源码)** ⭐
  - 从零开始学 HarmonyOS
  - 大量实战案例
  - 配套书籍推荐

#### 🔔 通知功能
- **[8. 通知详解](other.md#8.通知.md)** ⭐
  - 基础通知
  - 进度条通知
  - 通知管理

#### ⚡ 异步编程
- **[Promise 详解](other.md#Promise)** ⭐
  - Promise 基础
  - async/await
  - 错误处理

#### 📦 第三方库
- **[12. 使用第三方库](other.md#12.使用第三方库)** ⭐
  - 库的引入方式
  - 常用第三方库
  - 最佳实践

#### 📄 README
- **[项目 README](other.md#README.md)**

#### 🌐 分布式能力
- **[万物互联](other.md#万物互联)** ⭐
  - 分布式软总线
  - 设备发现与连接
  - 跨设备协作

#### 📢 公共事件
- **[14. 公共事件概述](other.md#14.公共事件概述.md)** ⭐
  - 事件发布与订阅
  - 系统事件
  - 自定义事件

#### 🚀 应用开发
- **[4. HarmonyOS 应用开发初探](other.md#4.HarmonyOS应用开发初探)** ⭐

#### 📖 术语表
- **[3. HarmonyOS 开发术语](other.md#3.HarmonyOS开发术语.md)** ⭐

#### 🔗 官方资源
- **[官方资源汇总](other.md#官方资源)** ⭐
  - 官方文档
  - 开发工具
  - 社区资源

---

## 📝 学习路径建议

### 初级阶段（1-2周）🌱
1. 阅读 ArkTS 声明式 UI 入门
2. 运行 HelloWorld 和基础组件示例
3. 理解 Ability 生命周期
4. 掌握基础布局（Column、Row、Stack）
5. 学习状态管理基础（@State）

### 中级阶段（2-4周）🌿
1. 学习页面路由和导航
2. 掌握完整状态管理（@Prop、@Link、@Provide/@Consume）
3. 实践数据存储（首选项、关系型数据库）
4. 学习网络请求与数据处理
5. 开发完整应用（如购物应用、音乐播放器）

### 高级阶段（持续）🌳
1. 分布式能力开发
2. 性能优化最佳实践
3. AI 能力集成（语音识别、图像识别）
4. 原子化服务开发
5. 自定义组件和动画

### 专业方向 🎯
- **UI/UX 方向**: 深入学习 ArkUI 高级特性、动画、自定义绘制
- **后端方向**: 重点关注网络、数据库、分布式数据
- **多媒体方向**: 专注音视频、图像处理、相机开发
- **AI 方向**: 学习 AI 能力集成、大模型应用开发

---

## 🛠️ 开发工具

### DevEco Studio
- **最新版本**: DevEco Studio 5.0+
- **下载地址**: [华为开发者联盟](https://developer.huawei.com/consumer/cn/deveco-studio/)
- **支持平台**: Windows、macOS、Linux

### 模拟器
- 本地模拟器（推荐开发阶段使用）
- 远程真机（需要实名认证）
- Super Device（分布式调试）

---

## 🔥 最新特性

### HarmonyOS 6 新特性
- 原生智能系统
- AI 能力增强
- 更流畅的动画体验
- 改进的分布式能力
- 新增组件和 API

### HarmonyOS Next (HarmonyOS 5) 特性
- 纯血鸿蒙（去 AOSP 代码）
- ArkTS 语言增强
- 方舟引擎性能优化
- 更好的多设备适配

---

## 💡 最佳实践

### 代码规范
1. **命名规范**
   - 组件名使用大驼峰（PascalCase）
   - 变量名使用小驼峰（camelCase）
   - 常量名使用全大写下划线分隔（UPPER_SNAKE_CASE）

2. **文件组织**
   - 按功能模块划分目录
   - 组件、工具、数据分离
   - 合理使用自定义组件

3. **性能优化**
   - 使用 LazyForEach 处理长列表
   - 合理设置 cachedCount
   - 避免不必要的渲染

4. **状态管理**
   - 优先使用 @State
   - 跨组件使用 @Provide/@Consume
   - 双向绑定使用 @Link

---

## 🆘 常见问题

### Q1: ArkTS 和 TypeScript 有什么区别？
**A**: ArkTS 是基于 TypeScript 扩展的 HarmonyOS 官方语言，增加了声明式 UI 描述、状态管理、组件系统等特性。

### Q2: HarmonyOS Next 和之前的版本有什么不同？
**A**: HarmonyOS Next 是"纯血鸿蒙"，移除了所有 AOSP 代码，完全基于鸿蒙内核，性能和安全性都有显著提升。

### Q3: 如何开始学习 HarmonyOS 开发？
**A**: 建议按照"学习路径建议"部分，从基础概念开始，逐步深入实践。官方文档和社区资源也非常丰富。

### Q4: 开发 HarmonyOS 应用需要什么基础？
**A**: 需要具备基础的编程知识，了解 TypeScript/JavaScript 会很有帮助。有前端开发经验（React、Vue）会更容易上手。

### Q5: HarmonyOS 应用能在哪些设备上运行？
**A**: HarmonyOS Next 支持手机、平板、手表、耳机、汽车、智慧屏等多种设备，实现真正的"一次开发，多端部署"。

---

## 📞 获取帮助

### 官方资源
- **华为开发者联盟**: https://developer.huawei.com
- **HarmonyOS 官方文档**: https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/
- **官方论坛**: https://developer.huawei.com/consumer/cn/forum/home

### 社区资源
- **GitHub**: 搜索 HarmonyOS 相关项目
- **CSDN**: HarmonyOS 开发者社区
- **掘金**: HarmonyOS 专题

### 学习资源
- **官方 Codelabs**: https://developer.huawei.com/consumer/cn/codelabs/
- **HarmonyOS 学堂**: https://developer.huawei.com/consumer/cn/training/

---

## 📊 文档统计

- **总文档数**: 300+ 页
- **示例代码**: 100+ 个
- **覆盖主题**: 20+ 类
- **代码行数**: 50,000+ 行

---

## 📅 更新日志

### 2025-01
- 优化文档索引结构
- 添加 HarmonyOS 6 相关内容
- 补充最佳实践和常见问题
- 改进学习路径建议

### 持续更新
- 跟进 HarmonyOS 最新版本
- 补充新特性和 API
- 优化示例代码质量
- 完善文档结构

---

## 🎓 推荐书籍

1. **《鸿蒙之光 HarmonyOS 6 应用开发入门》**
   - 作者: 老卫
   - 适合: 零基础入门
   - 特点: 图文并茂，实例丰富

2. **《鸿蒙HarmonyOS应用开发从入门到精通（第2版）》**
   - 作者: 老卫
   - 适合: 系统学习
   - 特点: 全面深入

3. **《跟老卫学HarmonyOS开发》**
   - 作者: 老卫
   - 适合: 快速上手
   - 特点: 实战导向

---

## 🌟 结语

HarmonyOS Next 代表了华为在操作系统领域的最新成果， ArkTS 和 ArkUI 提供了现代化的开发体验。希望这份文档索引能帮助你快速找到所需的学习资源，开启你的 HarmonyOS 开发之旅！

**记住**: 实践是最好的学习方式。多动手编码，多运行示例，多思考总结，你一定能掌握 HarmonyOS 开发！

---

*最后更新: 2025-01*
*维护者: HarmonyOS 开发社区*
*许可证: MIT*
