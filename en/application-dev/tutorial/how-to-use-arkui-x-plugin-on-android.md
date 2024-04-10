# How to Use ArkUI-X Plugins on Android

ArkUI-X provides APIs for managing the lifecycle of ArkUI-X plugins, which are used to expand the capabilities of ArkUI applications. This topic describes how to use the ArkUI-X plugins on the Android platform.

### Creating an ArkUI-X Plugin

To create an ArkUI-X plugin that can run on Android devices, implement the **IArkUIXPlugin** API.

```java
// plugin.java
import ohos.ace.adapter.IArkUIXPlugin;
import ohos.ace.adapter.PluginContext;
import ohos.ace.adapter.capability.bridge.BridgePlugin;

public class myPlugin implements IArkUIXPlugin {
    public Bridge myBridge = null;
    @Override
    public void onRegistry(PluginContext pluginContext) {
        // Create a plugin and initialize it.
        myBridge = new MyBridge(pluginContext.getContext(), "MyTestBridge", 
                                      pluginContext.getBridgeManager());
    }
    @Override
    public void onUnRegistry(PluginContext pluginContext) {
        // Release the plugin.
        if (myBridge != null) {
            myBridge = null;
        }
    }
}
```

### Adding an ArkUI-X Plugin

Call **addPlugin<sup>11+</sup>** in **StageActivity** to add the object of the **IArkUIXPlugin** implementation class (whose complete package name is in the form of a string) to the StageActivity. Declare **addPlugin** as follows:

```java
class StageActivity extents Activity implements KeyboardHeightObserver {
    // Add an ArkUI-X plugin to the list for registration.
    // The pluginName parameter specifies the name of the package that implements the plugin.
	public void addPlugin(String pluginName); 
}
```

Use **onRegistry()** in the **onCreate()** callback of the StageActivity to instruct the application to create a plugin. Use **onUnRegistry()** in the **onDestroy()** callback of the StageActivity to instruct the application to destroy the plugin.

**NOTE: Call addPlugin() in prior to onCreate() of the superclass.**

```java
// EntryEntryAbilityActivity,java
import android.os.Bundle;
import ohos.stage.ability.adapter.StageActivity;

public class EntryEntryAbilityActivity extends StageActivity {
    @Override
    public void onCreate(Bundle savedInstanceState) {
        // The plugin must be added by using addPlugin() before super.onCreate() because the plugin will be called in onCreate() of the superclass.
        addPlugin("com.example.pluginlifecycle.plugin");
        setInstanceName("com.example.pluginlifecycle:entry:EntryAbility:");

        super.onCreate(savedInstanceState);
    }
}
```

### Example

For details about bridge, see [Bridge](how-to-use-bridge-on-android.md).

```java
// plugin.java

import ohos.ace.adapter.IArkUIXPlugin;
import ohos.ace.adapter.PluginContext;
import ohos.ace.adapter.capability.bridge.BridgePlugin;

public class plugin implements IArkUIXPlugin {
   public Bridge myBridge = null;
    // Called in onCreate() of the EntryEntryAbilityActivity.
    @Override
    public void onRegistry(PluginContext pluginContext) {
       // Create a plugin and initialize it.
       myBridge = new MyBridge(pluginContext.getContext(), "MyTestBridge", 
                                      pluginContext.getBridgeManager());
    }
    
    // Called in onDestroy() of the EntryEntryAbilityActivity.
    @Override
    public void onUnRegistry(PluginContext pluginContext) {
		// Release the plugin.
        if (myBridge != null) {
            myBridge = null;
        }
    }
}
```

Sample code for plugin registration:

```java
// EntryEntryAbilityActivity,java
import android.os.Bundle;
import ohos.stage.ability.adapter.StageActivity;

public class EntryEntryAbilityActivity extends StageActivity {
    @Override
    public void onCreate(Bundle savedInstanceState) {
        addPlugin("com.example.pluginlifecycle.plugin");
        setInstanceName("com.example.pluginlifecycle:entry:EntryAbility:");
        super.onCreate(savedInstanceState);
    }
}
```
