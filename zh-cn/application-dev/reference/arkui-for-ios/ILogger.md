# ILogger

本模块是日志记录的通用接口，支持日志拦截能力，用户在注册LogInterface后，能够将框架日志以及TypeScript日志通过接口进行输出。

## d

```objective-c
- (void)d:(NSString*)tag msg:(NSString*)msg;
```

**描述：**

用于打印调试日志。

**参数：** 

| Name | 类型   | 必填 | 描述           |
| ---- | ------ | ---- | -------------- |
| tag  | String | 是   | 消息标签。     |
| msg  | String | 是   | 要打印的消息。 |

**返回值：** 

无

**示例：** 

无

## i

```objective-c
- (void)i:(NSString*)tag msg:(NSString*)msg;
```

**描述：**

用于打印信息日志。

**参数：** 

| Name | 类型   | 必填 | 描述           |
| ---- | ------ | ---- | -------------- |
| tag  | String | 是   | 消息标签。     |
| msg  | String | 是   | 要打印的消息。 |

**返回值：** 

无

**示例：** 

无

## w

```objective-c
- (void)w:(NSString*)tag msg:(NSString*)msg;
```

**描述：**

用于打印警告日志。

**参数：** 

| Name | 类型   | 必填 | 描述           |
| ---- | ------ | ---- | -------------- |
| tag  | String | 是   | 消息标签。     |
| msg  | String | 是   | 要打印的消息。 |

**返回值：** 

无

**示例：** 

无

## e

```objective-c
- (void)e:(NSString*)tag msg:(NSString*)msg;
```

**描述：**

用于打印错误日志。

**参数：** 

| Name | 类型   | 必填 | 描述           |
| ---- | ------ | ---- | -------------- |
| tag  | String | 是   | 消息标签。     |
| msg  | String | 是   | 要打印的消息。 |

**返回值：** 

无

**示例：** 

无

## f

```objective-c
- (void)f:(NSString*)tag msg:(NSString*)msg;
```

**描述：**

用于打印致命日志。

**参数：** 

| Name | 类型   | 必填 | 描述           |
| ---- | ------ | ---- | -------------- |
| tag  | String | 是   | 消息标签。     |
| msg  | String | 是   | 要打印的消息。 |

**返回值：** 

无

**示例：** 

无

### 类型定义

#### **LogLevel**

**描述：** 

枚举类型，用于设置拦截日志等级。

| 枚举值    | 描述     | **引用方式** |
| --------- | -------- | ------------ |
| LOG_DEBUG | 调试日志 | LOG_DEBUG    |
| LOG_INFO  | 信息日志 | LOG_INFO     |
| LOG_WARN  | 警告日志 | LOG_WARN     |
| LOG_ERROR | 错误日志 | LOG_ERROR    |
| LOG_FATAL | 错误日志 | LOG_FATAL    |

## 完整示例

[完整示例](../../tutorial/how-to-use-arkui-x-loginterface-on-ios.md)