# OpenHarmony接口定义跨平台支持列表
- 平台桥接
  - [@arkui-x.bridge](js-apis-bridge.md)
  
- UI界面
  - [@ohos.animator (动画)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-animator.md)
  - [@ohos.arkui.componentSnapshot (组件截图)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-arkui-componentSnapshot.md)
  - [@ohos.arkui.drawableDescriptor (DrawableDescriptor)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-arkui-drawableDescriptor.md)
  - [@ohos.arkui.UIContext (UIContext)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-arkui-UIContext.md)
  - [@ohos.curves (插值计算)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-curve.md)
  - [@ohos.font (注册自定义字体)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-font.md)
  - [@ohos.matrix4 (矩阵变换)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-matrix4.md)
  - [@ohos.mediaquery (媒体查询)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-mediaquery.md)
  - [@ohos.promptAction (弹窗)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-promptAction.md)
  - [@ohos.router (页面路由)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-router.md)
  - [@ohos.measure (文本计算)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-measure.md)

- Ability框架
  - Stage模型能力的接口(推荐)
    - [@ohos.app.ability.abilityLifecycleCallback (AbilityLifecycleCallback)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-font.md)
    - [@ohos.app.ability.AbilityConstant (AbilityConstant)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-app-ability-abilityConstant.md)
    - [@ohos.app.ability.AbilityStage (AbilityStage)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-app-ability-abilityStage.md)
    - [@ohos.app.ability.UIAbility (UIAbility)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-app-ability-uiAbility.md)
    - [@ohos.app.ability.common (应用上下文Context)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-app-ability-common.md)
  - 通用能力的接口(推荐)
    - [@ohos.app.ability.Configuration (Configuration)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-app-ability-configuration.md)
    - [@ohos.app.ability.ConfigurationConstant (ConfigurationConstant)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-app-ability-configurationConstant.md)
    - [@ohos.app.ability.Want (Want)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-app-ability-want.md)
  - 接口依赖的元素及定义
    - ability
    - app
    - application
      - [AbilityStageContext](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-inner-application-abilityStageContext.md)
      - [ApplicationContext](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-inner-application-applicationContext.md)
      - [BaseContext](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-inner-application-baseContext.md)
      - [Context](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-inner-application-context.md)
      - [ProcessInformation](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-inner-application-processInformation.md)
      - [UIAbilityContext](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-inner-application-uiAbilityContext.md)

- 包管理
  - [@ohos.bundle.bundleManager (bundleManager模块)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-bundleManager.md)
  - bundleManager
    - [abilityInfo](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-bundleManager-abilityInfo.md)
    - [applicationInfo](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-bundleManager-applicationInfo.md)
    - [hapModuleInfo](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-bundleManager-hapModuleInfo.md)
    - [metadata](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-bundleManager-metadata.md)

- 图形图像
  - [@ohos.window (窗口)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-window.md)
  - [@ohos.display (屏幕属性)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-display.md)

- 媒体
  - [@ohos.multimedia.image (图片处理)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md)

- 资源管理
  - [@ohos.i18n (国际化-I18n)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-i18n.md)
  - [@ohos.intl (国际化-Intl)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-intl.md)
  - [@ohos.resourceManager (资源管理)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-resource-manager.md)

- 网络管理
  - [@ohos.request (上传下载)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-request.md)
  - [@ohos.net.webSocket (WebSocket连接)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-webSocket.md)
  - [@ohos.net.socket (Socket连接)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-socket.md)
  - [@ohos.net.http (数据请求)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-http.md)
  - [@ohos.net.connection (网络连接管理)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-net-connection.md)

- 文件管理
  - [@ohos.file.fs (文件管理)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-file-fs.md)

- 设备管理
  - [@ohos.deviceInfo (设备信息)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-device-info.md)

- 数据管理
  - [@ohos.data.preferences (用户首选项)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-data-preferences.md)
  - [@ohos.data.relationalStore (关系型数据库)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-data-relationalStore.md)

- 安全
  - [@ohos.abilityAccessCtrl (程序访问控制管理)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-abilityAccessCtrl.md)

- 语言基础类库
  - [@ohos.buffer (Buffer)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-buffer.md)
  - [@ohos.convertxml (xml转换JavaScript)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-convertxml.md)
  - [@ohos.process (获取进程相关的信息)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-process.md)
  - [@ohos.taskpool (启动任务池)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-taskpool.md)
  - [@ohos.uri (URI字符串解析)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-uri.md)
  - [@ohos.url (URL字符串解析)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-url.md)
  - [@ohos.util (util工具函数)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-util.md)
  - [@ohos.util.ArrayList (线性容器ArrayList)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-arraylist.md)
  - [@ohos.util.Deque (线性容器Deque)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-deque.md)
  - [@ohos.util.HashMap (非线性容器HashMap)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-hashmap.md)
  - [@ohos.util.HashSet (非线性容器HashSet)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-hashset.md)
  - [@ohos.util.LightWeightMap (非线性容器LightWeightMap)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-lightweightmap.md)
  - [@ohos.util.LightWeightSet (非线性容器LightWeightSet)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-lightweightset.md)
  - [@ohos.util.LinkedList (线性容器LinkedList)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-linkedlist.md)
  - [@ohos.util.List (线性容器List)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-list.md)
  - [@ohos.util.PlainArray (非线性容器PlainArray)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-plainarray.md)
  - [@ohos.util.Queue (线性容器Queue)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-queue.md)
  - [@ohos.util.Stack (线性容器Stack)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-stack.md)
  - [@ohos.util.TreeMap (非线性容器TreeMap)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-treemap.md)
  - [@ohos.util.TreeSet (非线性容器TreeSet)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-treeset.md)
  - [@ohos.util.Vector (线性容器Vector)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-vector.md)
  - [@ohos.worker (启动一个Worker)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-worker.md)
  - [@ohos.xml (xml解析与生成)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-xml.md)

- 系统基础能力
  - [@ohos.hilog (HiLog日志打印)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-hilog.md)