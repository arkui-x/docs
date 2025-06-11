# ArkUI-X和原生侧如何实现相互传参和取参

以安卓为例，
1、ArkUI-X支持通过原生Activity拉起ArkTS侧Ability并传递参数：
使用原生Activity拉起Ability时，需使用原生应用的startActivity方法，参数的传递需要通过Intent中的putExtra()进行设置，目前有两种方式进行参数的传递，具体参考：[原生拉ArkUI-X并传参](https://gitcode.com/arkui-x/docs/blob/master/zh-cn/application-dev/quick-start/start-with-ability-on-android.md#%E9%80%9A%E8%BF%87%E5%8E%9F%E7%94%9Factivity%E6%8B%89%E8%B5%B7ability%E5%B9%B6%E4%BC%A0%E9%80%92%E5%8F%82%E6%95%B0)

2、ArkUI-X支持ArkTS侧用启动Ability的方式拉起原生Activity：
每一个Ability对应一个StageActivity，启动Ability实际是拉起对应的StageActivity。
所以将原生Activity按照上文中Ability对应StageActivity的规则命名，可以用启动Ability的方式拉起原生Activity，具体参考：[ArkUI-X拉原生并传参](https://gitcode.com/arkui-x/docs/blob/master/zh-cn/application-dev/quick-start/start-with-ability-on-android.md#%E7%94%A8%E5%90%AF%E5%8A%A8ability%E7%9A%84%E6%96%B9%E5%BC%8F%E6%8B%89%E8%B5%B7%E5%8E%9F%E7%94%9Factivity)



注：[iOS能力参考](https://gitcode.com/arkui-x/docs/blob/master/zh-cn/application-dev/quick-start/start-with-ability-on-ios.md#%E9%80%9A%E8%BF%87ios%E5%8E%9F%E7%94%9F%E6%8B%89%E8%B5%B7ability%E5%B9%B6%E4%BC%A0%E9%80%92%E5%8F%82%E6%95%B0)