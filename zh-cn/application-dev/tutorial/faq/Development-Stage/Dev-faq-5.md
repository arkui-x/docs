# ArkUI-X for Android如何设置窗口内导航栏、状态栏属性

HarmonyOS NEXT上Window.setWindowSystemBarProperties设置窗口内导航栏、状态栏属性，该能力当前不支持跨平台

**【替代方案】**可以在android原生代码中设置状态栏，导航栏颜色，示例代码如下：

```java

public static void setWindowBarColor(Activity activity, boolean isTextColorBlack) {
        Window window = activity.getWindow();
        if (window != null) {
            window.clearFlags(WindowManager.LayoutParams.FLAG_TRANSLUCENT_STATUS);
            int flag = getFlag(isTextColorBlack);
            window.setStatusBarColor(Color.TRANSPARENT);
            window.getDecorView().setSystemUiVisibility(flag);
            window.addFlags(WindowManager.LayoutParams.FLAG_DRAWS_SYSTEM_BAR_BACKGROUNDS);
        }
    }
    private static int getFlag(boolean isTextColorBlack) {
        int flag = View.SYSTEM_UI_FLAG_LAYOUT_STABLE;
        if (isTextColorBlack) {
            // 设置状态栏透明字体为黑色，导航栏字体颜色为黑色、背景颜色
            flag |= View.SYSTEM_UI_FLAG_LIGHT_STATUS_BAR;
            flag |= View.SYSTEM_UI_FLAG_LIGHT_NAVIGATION_BAR;
        } else {
            // 设置状态栏透明字体为白色，导航栏字体颜色为白色、背景颜色
            flag &= ~View.SYSTEM_UI_FLAG_LIGHT_STATUS_BAR;
            flag &= ~View.SYSTEM_UI_FLAG_LIGHT_NAVIGATION_BAR;
        }
        return flag;
    }

```
