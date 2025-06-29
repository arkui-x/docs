# OpenHarmony接口定义跨平台支持列表
- 平台桥接
  - [@arkui-x.bridge (平台桥接)](js-apis-bridge.md)

- UI界面
  - [@ohos.animator (动画)](js-apis-animator.md)
  - [@ohos.arkui.componentSnapshot (组件截图)](js-apis-arkui-componentSnapshot.md)
  - [@ohos.arkui.componentUtils (componentUtils)](js-apis-arkui-componentUtils.md)
  - [@ohos.arkui.dragController (DragController)](js-apis-arkui-dragController.md)
  - [@ohos.arkui.observer (无感监听)](js-apis-arkui-observer.md)
  - [@ohos.arkui.inspector (布局回调)](js-apis-arkui-inspector.md)
  - [@ohos.arkui.theme (主题换肤)](js-apis-arkui-theme.md)
  - [@ohos.arkui.UIContext (UIContext)](js-apis-arkui-UIContext.md)
  - [@ohos.arkui.StateManagement (状态管理)](js-apis-StateManagement.md)
  - [@ohos.curves (插值计算)](js-apis-curve.md)
  - [@ohos.font (注册自定义字体)](js-apis-font.md)
  - [@ohos.matrix4 (矩阵变换)](js-apis-matrix4.md)
  - [@ohos.measure (文本计算)](js-apis-measure.md)
  - [@ohos.mediaquery (媒体查询)](js-apis-mediaquery.md)
  - [@ohos.promptAction (弹窗)](js-apis-promptAction.md)
  - [@ohos.router (页面路由)](js-apis-router.md)
  - [@ohos.arkui.drawableDescriptor (DrawableDescriptor)](js-apis-arkui-drawableDescriptor.md)
  - [@ohos.arkui.Prefetcher (Prefetching)](js-apis-arkui-Prefetcher.md)
- ArkTS组件
  - 按钮与选择
    - [SegmentButton](js-apis-arkui-advanced-segmentButton.md)
  - 标题栏与工具栏
    - [ToolBar](js-apis-arkui-advanced-toolBar.md)
    - [SubHeader](js-apis-arkui-advanced-subHeader.md)
  - 弹窗
    - [弹出框 (Dialog)](js-apis-arkui-advanced-dialog.md)
  - 系统预置UI组件库
    - [SwipeRefresher](js-api-arkui-advanced-SwipeRefresher.md)
    - [ComposeListItem](js-apis-arkui-advanced-composeListItem.md)
    - [ComposeTitleBar](js-apis-arkui-advanced-composeTitleBar.md)
    - [SelectTitleBar](js-apis-arkui-advanced-selectTitleBar.md)
    - [SelectionMenu](js-apis-arkui-advanced-selectionMenu.md)
- Ability框架
  - Stage模型能力的接口(推荐)
    - [@ohos.app.ability.abilityLifecycleCallback (AbilityLifecycleCallback)](js-apis-app-ability-abilityLifecycleCallback.md)
    - [@ohos.app.ability.ApplicationStateChangeCallback (ApplicationStateChangeCallback)](js-apis-app-ability-applicationStateChangeCallback.md)
    - [@ohos.app.ability.AbilityConstant (AbilityConstant)](js-apis-app-ability-abilityConstant.md)
    - [@ohos.app.ability.AbilityStage (AbilityStage)](js-apis-app-ability-abilityStage.md)
    - [@ohos.app.ability.UIAbility (UIAbility)](js-apis-app-ability-uiAbility.md)
    - [@ohos.app.ability.common (应用上下文Context)](js-apis-app-ability-common.md)
  - 通用能力的接口(推荐)
    - [@ohos.app.ability.Configuration (Configuration)](js-apis-app-ability-configuration.md)
    - [@ohos.app.ability.ConfigurationConstant (ConfigurationConstant)](js-apis-app-ability-configurationConstant.md)
    - [@ohos.app.ability.Want (Want)](js-apis-app-ability-want.md)
    - [@ohos.app.ability.errorManager (ErrorManager)](js-apis-app-ability-errorManager.md)

  - 接口依赖的元素及定义
    - application
      - [abilityDelegator](js-apis-inner-application-abilityDelegator.md)
      - [abilityDelegatorArgs](js-apis-inner-application-abilityDelegatorArgs.md)
      - [abilityMonitor](js-apis-inner-application-abilityMonitor.md)
      - [AbilityStageContext](js-apis-inner-application-abilityStageContext.md)
      - [abilityStageMonitor](js-apis-inner-application-abilityStageMonitor.md)
      - [ApplicationContext](js-apis-inner-application-applicationContext.md)
      - [BaseContext](js-apis-inner-application-baseContext.md)
      - [Context](js-apis-inner-application-context.md)
      - [UIAbilityContext](js-apis-inner-application-uiAbilityContext.md)

- 包管理
  - [@ohos.bundle.bundleManager (bundleManager模块)](js-apis-bundleManager.md)
  - [@ohos.zlib (Zip模块)](js-apis-zlib.md)
  - bundleManager
    - [abilityInfo](js-apis-bundleManager-abilityInfo.md)
    - [applicationInfo](js-apis-bundleManager-applicationInfo.md)
    - [bundleInfo](js-apis-bundleManager-bundleInfo.md)
    - [hapModuleInfo](js-apis-bundleManager-hapModuleInfo.md)
    - [metadata](js-apis-bundleManager-metadata.md)

- 图形图像
  - [@ohos.window (窗口)](js-apis-window.md)
  - [@ohos.display (屏幕属性)](js-apis-display.md)
  - [@ohos.effectKit (图像效果)](js-apis-effectKit.md)

- 媒体
  - [@ohos.multimedia.image (图片处理)](js-apis-image.md)
  - [@ohos.multimedia.audio (音频管理)](js-apis-audio.md)
  - [@ohos.multimedia.media (媒体服务)](js-apis-media.md)
  
- 资源管理
  - [@ohos.i18n (国际化-I18n)](js-apis-i18n.md)
  - [@ohos.intl (国际化-Intl)](js-apis-intl.md)
  - [@ohos.resourceManager (资源管理)](js-apis-resource-manager.md)

- 网络管理
  - [@ohos.request (上传下载)](js-apis-request.md)
  - [@ohos.net.webSocket (WebSocket连接)](js-apis-webSocket.md)
  - [@ohos.net.socket (Socket连接)](js-apis-socket.md)
  - [@ohos.net.http (数据请求)](js-apis-http.md)
  - [@ohos.net.connection (网络连接管理)](js-apis-net-connection.md)

- WiFi管理
  - [@ohos.wifiManager (WLAN)](js-apis-wlan.md)

- 基础通信
  - [@ohos.bluetooth.a2dp ](js-apis-bluetooth-a2dp.md)
  - [@ohos.bluetooth.access ](js-apis-bluetooth-access.md)
  - [@ohos.bluetooth.baseProfile ](js-apis-bluetooth-baseProfile.md)
  - [@ohos.bluetooth.ble ](js-apis-bluetooth-ble.md)
  - [@ohos.bluetooth.connection ](js-apis-bluetooth-connection.md)
  - [@ohos.bluetooth.constant ](js-apis-bluetooth-constant.md)
- 文件管理
  - [@ohos.file.fs (文件管理)](js-apis-file-fs.md)
  - [@ohos.file.hash (文件哈希处理)](js-apis-file-hash.md)
  - [@ohos.file.statvfs (文件系统空间统计)](js-apis-file-statvfs.md)
  - [@ohos.file.photoAccessHelper (相册管理模块)](js-apis-photoAccessHelper.md)
  - [@ohos.file.picker(选择器)](js-apis-file-picker.md)

- 设备管理
  - [@ohos.deviceInfo (设备信息)](js-apis-device_Info.md)

- 数据管理
  - [@ohos.data.dataSharePredicates (数据共享谓词)](js-apis-data-dataSharePredicates.md)
  - [@ohos.data.preferences (用户首选项)](js-apis-data-preferences.md)
  - [@ohos.data.relationalStore (关系型数据库)](js-apis-data-relationalStore.md)
  - [@ohos.data.unifiedDataChannel(标准化数据通路)](js-apis-data-unifiedDataChannel.md)
  - [@ohos.data.uniformTypeDescriptor (标准化数据定义与描述)](js-apis-data-uniformTypeDescriptor.md)
  - [@ohos.data.ValuesBucket (数据集)](js-apis-data-valuesBucket.md)
  
- 安全
  - [@ohos.abilityAccessCtrl (程序访问控制管理)](js-apis-abilityAccessCtrl.md)
  - [@ohos.security.cryptoFramework (加解密算法库框架)](js-apis-cryptoFramework.md)
  - [@ohos.security.cert(证书算法库框架)](js-apis-cert.md)

- 语言基础类库
  - [@arkts.collections (ArkTS容器集)](js-apis-arkts-collections.md)
  - [@arkts.lang (ArkTS语言基础能力)](js-apis-arkts-lang.md)
  - [@arkts.math.Decimal (高精度数学库Decimal)](js-apis-arkts-decimal.md)
  - [@arkts.utils (ArkTS工具库)](js-apis-arkts-utils.md)
  - [@ohos.buffer (Buffer)](js-apis-buffer.md)
  - [@ohos.convertxml (xml转换JavaScript)](js-apis-convertxml.md)
  - [@ohos.process (获取进程相关的信息)](js-apis-process.md)
  - [@ohos.taskpool (启动任务池)](js-apis-taskpool.md)
  - [@ohos.uri (URI字符串解析)](js-apis-uri.md)
  - [@ohos.url (URL字符串解析)](js-apis-url.md)
  - [@ohos.util (util工具函数)](js-apis-util.md)
  - [@ohos.util.ArrayList (线性容器ArrayList)](js-apis-arraylist.md)
  - [@ohos.util.Deque (线性容器Deque)](js-apis-deque.md)
  - [@ohos.util.HashMap (非线性容器HashMap)](js-apis-hashmap.md)
  - [@ohos.util.HashSet (非线性容器HashSet)](js-apis-hashset.md)
  - [@ohos.util.LightWeightMap (非线性容器LightWeightMap)](js-apis-lightweightmap.md)
  - [@ohos.util.LightWeightSet (非线性容器LightWeightSet)](js-apis-lightweightset.md)
  - [@ohos.util.LinkedList (线性容器LinkedList)](js-apis-linkedlist.md)
  - [@ohos.util.List (线性容器List)](js-apis-list.md)
  - [@ohos.util.PlainArray (非线性容器PlainArray)](js-apis-plainarray.md)
  - [@ohos.util.Queue (线性容器Queue)](js-apis-queue.md)
  - [@ohos.util.Stack (线性容器Stack)](js-apis-stack.md)
  - [@ohos.util.TreeMap (非线性容器TreeMap)](js-apis-treemap.md)
  - [@ohos.util.TreeSet (非线性容器TreeSet)](js-apis-treeset.md)
  - [@ohos.worker (启动一个Worker)](js-apis-worker.md)
  - [@ohos.xml (xml解析与生成)](js-apis-xml.md)

- 系统基础能力
  - [@ohos.hilog (HiLog日志打印)](js-apis-hilog.md)
  - [@ohos.hiTraceMeter (性能打点)](js-apis-hitracemeter.md)
  - [@ohos.web.webview(Webview)](js-apis-webview.md)

- 公共事件与通知
  - [系统公共事件定义](commonEventManager-definitions.md)
  - [@ohos.commonEventManager (公共事件模块)](js-apis-commonEventManager.md)
  - [@ohos.notificationManager (NotificationManager模块)](js-apis-notificationManager.md)
  - application
    - [EventHub](js-apis-inner-application-eventHub.md)
- 性能分析服务
  - [@ohos.hiviewdfx.hiAppEvent (应用事件打点)](js-apis-hiviewdfx-hiappevent.md)

- 进程线程通信
  - [@ohos.events.emitter (Emitter)](js-apis-emitter.md)

- 其他
  - [@ohos.systemDateTime (系统时间、时区)](js-apis-date-time.md)