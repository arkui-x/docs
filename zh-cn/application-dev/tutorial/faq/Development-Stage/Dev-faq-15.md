# ArkUI-X Android界面在输入法展开收起时出现界面闪烁

**【问题现象】**

在点击Search等编辑框，输入法展开，收起时，界面出现闪烁。

**【问题原因】**

AndroidManifest中Activity如果设置了adjustResize属性，Android原生逻辑在输入法展开收起时，会尝试调整Activity的高度；ArkUI-X的逻辑也需要监听输入法收起，调整编辑框位置。重复逻辑导致界面出现闪烁。

**【解决方法】**

对于ArkUI-X的跨平台页面的Activity，不需要设置android:windowSoftInputMode，删除即可。
![image](../figures/dev-faq-15.png)<br/>
