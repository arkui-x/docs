# OpenHarmony接口定义跨平台支持列表
- UI界面
  - [@ohos.mediaquery (媒体查询)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-mediaquery.md)
  - [@ohos.promptAction (弹窗)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-promptAction.md)
  - [@ohos.router (页面路由)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-router.md)
  - [@ohos.animator](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-animator.md)
  - [@ohos.arkui.componentSnapshot]()
  - [@ohos.font]()
  - [@ohos.measure]()
  - [@ohos.matrix4]()
  - [@ohos.curves]()
- 平台桥接
  - [@arkui-x.bridge](#https://gitee.com/arkui-x/docs/tree/master/zh-cn/application-dev/reference/apis/js-apis-bridge.md)

- Ability框架
  - Stage模型能力的接口(推荐)
    - [@ohos.app.ability.abilityLifecycleCallback (AbilityLifecycleCallback)](js-apis-app-ability-abilityLifecycleCallback.md)
    - [@ohos.app.ability.AbilityConstant (AbilityConstant)](js-apis-app-ability-abilityLifecycleCallback.md)
    - [@ohos.app.ability.AbilityStage (AbilityStage)](js-apis-app-ability-abilityStage.md)
    - [@ohos.app.ability.UIAbility (UIAbility)](js-apis-app-ability-uiAbility.md)
    - [@ohos.app.ability.common (应用上下文Context)](js-apis-application-want.md)
  - 通用能力的接口(推荐)
    - [@ohos.app.ability.Configuration (Configuration)](js-apis-app-ability-configuration.md)
    - [@ohos.app.ability.ConfigurationConstant (ConfigurationConstant)](js-apis-app-ability-configurationConstant.md)
    - [@ohos.app.ability.Want (Want)](js-apis-application-want.md)
  - 接口依赖的元素及定义
    - ability
    - app
    - application
      - [AbilityStageContext](js-apis-inner-application-abilityStageContext.md)
      - [ApplicationContext](js-apis-inner-application-applicationContext.md)
      - [BaseContext](js-apis-inner-application-baseContext.md)
      - [Context](js-apis-inner-application-context.md)
      - [ProcessInformation](js-apis-inner-application-processInformation.md)
      - [UIAbilityContext](js-apis-inner-application-uiAbilityContext.md)

- 包管理
  - [@ohos.bundle.bundleManager (bundleManager模块)](js-apis-bundleManager.md)
  - bundleManager
    - [abilityInfo](js-apis-bundleManager-abilityInfo.md)
    - [applicationInfo](js-apis-bundleManager-applicationInfo.md)
    - [hapModuleInfo](js-apis-bundleManager-hapModuleInfo.md)
    - [metadata](js-apis-bundleManager-metadata.md)

- 图形图像
  - [@ohos.window (窗口)]()
  - [@ohos.display (屏幕属性)]()

- 媒体
  - [@ohos.multimedia.image (图片处理)]()

- 资源管理
  - [@ohos.i18n (国际化-I18n)]()
  - [@ohos.intl (国际化-Intl)]()
  - [@ohos.resourceManager (资源管理)]()

- 网络管理
  - [@ohos.request (上传下载)]()
  - [@ohos.net.webSocket (WebSocket连接)]()
  - [@ohos.net.socket (Socket连接)]()
  - [@ohos.net.http (数据请求)]()
  - [@ohos.net.connection (网络连接管理)]()

- 文件管理
  - [@ohos.file.fs (文件管理)]()

- 设备管理
  - [@ohos.deviceInfo (设备信息)]()

- 数据管理
  - [@ohos.data.preferences (用户首选项)]()
  - [@ohos.data.relationalStore (关系型数据库)]()

- 安全
  - [@ohos.abilityAccessCtrl (程序访问控制管理)]()

- 语言基础类库
  - [@ohos.buffer (Buffer)]()
  - [@ohos.convertxml (xml转换JavaScript)]()
  - [@ohos.process (获取进程相关的信息)]()
  - [@ohos.taskpool (启动任务池)]()
  - [@ohos.uri (URI字符串解析)]()
  - [@ohos.url (URL字符串解析)]()
  - [@ohos.util (util工具函数)]()
  - [@ohos.util.ArrayList (线性容器ArrayList)]()
  - [@ohos.util.Deque (线性容器Deque)]()
  - [@ohos.util.HashMap (非线性容器HashMap)]()
  - [@ohos.util.HashSet (非线性容器HashSet)]()
  - [@ohos.util.LightWeightMap (非线性容器LightWeightMap)]()
  - [@ohos.util.LightWeightSet (非线性容器LightWeightSet)]()
  - [@ohos.util.LinkedList (线性容器LinkedList)]()
  - [@ohos.util.List (线性容器List)]()
  - [@ohos.util.PlainArray (非线性容器PlainArray)]()
  - [@ohos.util.Queue (线性容器Queue)]()
  - [@ohos.util.Stack (线性容器Stack)]()
  - [@ohos.util.TreeMap (非线性容器TreeMap)]()
  - [@ohos.util.TreeSet (非线性容器TreeSet)]()
  - [@ohos.util.Vector (线性容器Vector)]()
  - [@ohos.worker (启动一个Worker)]()
  - [@ohos.xml (xml解析与生成)]()

- 系统基础能力
  - [@ohos.hilog (HiLog日志打印)]()