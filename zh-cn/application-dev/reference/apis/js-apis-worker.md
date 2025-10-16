# @ohos.worker (启动一个Worker)

Worker是与主线程并行的独立线程。创建Worker的线程称之为宿主线程，Worker自身的线程称之为Worker线程。创建Worker传入的url文件在Worker线程中执行，可以处理耗时操作但不可以直接操作UI。

Worker主要作用是为应用程序提供一个多线程的运行环境，可满足应用程序在执行过程中与主线程分离，在后台线程中运行一个脚本操作耗时操作，极大避免类似于计算密集型或高延迟的任务阻塞主线程的运行。由于Worker一旦被创建则不会主动被销毁，若不处于任务状态一直运行，在一定程度上会造成资源的浪费，应及时关闭空闲的Worker。

Worker的上下文对象和主线程的上下文对象是不同的，Worker线程不支持UI操作。

> **说明：**<br/>
> 本模块首批接口从API version 7 开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```js
import worker from '@ohos.worker';
```


## 属性

**系统能力：** SystemCapability.Utils.Lang

| 名称                              | 类型                                                      | 可读 | 可写 | 说明                                                         |
| --------------------------------- | --------------------------------------------------------- | ---- | ---- | ------------------------------------------------------------ |
| workerPort<sup>9+</sup>           | [ThreadWorkerGlobalScope](#threadworkerglobalscope9)      | 是   | 是   | worker线程用于与宿主线程通信的对象。                         |

## WorkerOptions

Worker构造函数的选项，用于为Worker添加其他信息。

**系统能力：** SystemCapability.Utils.Lang

| 名称 | 类型 | 只读 | 可选 | 说明 |
| ---- | -------- | ---- | ---- | -------------- |
| type | 'classic' \| 'module' | 否   | 是 | Worker执行脚本的模式类型，暂不支持module类型，默认值为"classic"。<br/>**原子化服务API：** 从API version 11开始，该接口支持在原子化服务中使用。 |
| name | string   | 否   | 是 | Worker的名称，默认值为undefined。<br/>**原子化服务API：** 从API version 11开始，该接口支持在原子化服务中使用。|
| shared | boolean | 否   | 是 | 表示Worker共享功能，此接口暂不支持。 <br/>**原子化服务API：** 从API version 12开始，该接口支持在原子化服务中使用。|
| priority<sup>22+</sup> | [ThreadWorkerPriority](#threadworkerpriority22) | 否   | 是 | 表示Worker线程优先级。默认值为MEDIUM。 <br/>**原子化服务API：** 从API version 22开始，该接口支持在原子化服务中使用。|

## ThreadWorkerPriority<sup>22+</sup>

Worker线程的优先级枚举。

**系统能力：** SystemCapability.Utils.Lang

| 名称 | 值 | 说明 |
| -------- | -------- | -------- |
| HIGH   | 0    | 适用于打开文档等用户触发并且可以看到进展的任务，任务在几秒钟之内完成。对应QOS_USER_INITIATED。<br>**原子化服务API**：从API version 22开始，该接口支持在原子化服务中使用。 |
| MEDIUM | 1 | 任务完成需要几秒钟。是[ThreadWorkerPriority](#threadworkerpriority22)的默认值。对应QOS_DEFAULT。<br>**原子化服务API**：从API version 22开始，该接口支持在原子化服务中使用。 |
| LOW | 2 | 适用于下载等不需要立即看到响应效果的任务，任务完成需要几秒到几分钟。对应QOS_UTILITY。<br>**原子化服务API**：从API version 22开始，该接口支持在原子化服务中使用。 |
| IDLE | 3 | 适用于数据同步等用户不可见的后台任务，任务完成需要几分钟甚至几小时。对应QOS_BACKGROUND。<br>**原子化服务API**：从API version 22开始，该接口支持在原子化服务中使用。 |
| DEADLINE<sup>22+</sup> | 4 | 适用于页面加载等越快越好的关键任务，任务几乎是瞬间完成的。对应QOS_DEADLINE_REQUEST。<br>**原子化服务API**：从API version 22开始，该接口支持在原子化服务中使用。 |
| VIP<sup>22+</sup> | 5 | 适用于UI线程、动画渲染等用户交互任务，任务是即时的。对应QOS_USER_INTERACTIVE。<br>**原子化服务API**：从API version 22开始，该接口支持在原子化服务中使用。 |


## ThreadWorker<sup>9+</sup>

使用以下方法前，均需先构造ThreadWorker实例，ThreadWorker类继承[WorkerEventTarget](#workereventtarget9)。

### constructor<sup>9+</sup>

constructor(scriptURL: string, options?: WorkerOptions)

ThreadWorker构造函数。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名    | 类型                            | 必填 | 说明                                                         |
| --------- | ------------------------------- | ---- | ------------------------------------------------------------ |
| scriptURL | string                          | 是   | Worker执行脚本的路径。<br/>在FA和Stage模型下，DevEco Studio新建Worker工程路径分别存在以下两种情况：<br/>(a) worker脚本所在目录与pages目录同级。<br/>(b) worker脚本所在目录与pages目录不同级。 |
| options   | [WorkerOptions](#workeroptions) | 否   | Worker构造的选项。                                           |

**返回值：**

| 类型         | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| ThreadWorker | 执行ThreadWorker构造函数生成的ThreadWorker对象，失败则返回undefined。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 10200003 | Worker initialization failed. |
| 10200007 | The worker file path is invalid. |



**示例：**

```js
import worker from '@ohos.worker';
// worker线程创建

// FA模型-目录同级（entry模块下，workers目录与pages目录同级）
const workerFAModel01 = new worker.ThreadWorker("workers/worker.ts", {name:"first worker in FA model"});
// FA模型-目录不同级（entry模块下，workers目录与pages目录的父目录同级）
const workerFAModel02 = new worker.ThreadWorker("../workers/worker.ts");

// Stage模型-目录同级（entry模块下，workers目录与pages目录同级）
const workerStageModel01 = new worker.ThreadWorker('entry/ets/workers/worker.ts', {name:"first worker in Stage model"});
// Stage模型-目录不同级（entry模块下，workers目录是pages目录的子目录）
const workerStageModel02 = new worker.ThreadWorker('entry/ets/pages/workers/worker.ts');

// 理解Stage模型scriptURL的"entry/ets/workers/worker.ts"：
// entry: 为module.json5文件中module的name属性对应的值，ets: 表明当前使用的语言。
// scriptURL与worker文件所在的workers目录层级有关，与new worker所在文件无关。

// Stage模型工程esmodule编译场景下，支持新增的scriptURL规格：@bundle:bundlename/entryname/ets/workerdir/workerfile
// @bundle:为固定标签，bundlename为当前应用包名，entryname为当前模块名，ets为当前使用语言
// workerdir为worker文件所在目录，workerfile为worker文件名
// Stage模型-目录同级（entry模块下，workers目录与pages目录同级），假设bundlename是com.example.workerdemo
const workerStageModel03 = new worker.ThreadWorker('@bundle:com.example.workerdemo/entry/ets/workers/worker');
// Stage模型-目录不同级（entry模块下，workers目录是pages目录的子目录），假设bundlename是com.example.workerdemo
const workerStageModel04 = new worker.ThreadWorker('@bundle:com.example.workerdemo/entry/ets/pages/workers/worker');
```

同时，需在工程的模块级build-profile.json5文件的buildOption属性中添加配置信息，主要分为下面两种情况：

(1) 目录同级

FA模型:

```json
  "buildOption": {
    "sourceOption": {
      "workers": [
        "./src/main/ets/entryability/workers/worker.ts"
      ]
    }
  }
```

Stage模型:

```json
  "buildOption": {
    "sourceOption": {
      "workers": [
        "./src/main/ets/workers/worker.ts"
      ]
    }
  }
```

(2) 目录不同级

FA模型:

```json
  "buildOption": {
    "sourceOption": {
      "workers": [
        "./src/main/ets/workers/worker.ts"
      ]
    }
  }
```

Stage模型:

```json
  "buildOption": {
    "sourceOption": {
      "workers": [
        "./src/main/ets/pages/workers/worker.ts"
      ]
    }
  }
```

### postMessage<sup>9+</sup>

postMessage(message: Object, transfer: ArrayBuffer[]): void

宿主线程通过转移对象所有权的方式向Worker线程发送消息。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名   | 类型          | 必填 | 说明                                                         |
| -------- | ------------- | ---- | ------------------------------------------------------------ |
| message  | Object        | 是   | 发送至Worker的数据，该数据对象必须是可序列化，序列化支持类型见[其他说明](#序列化支持类型)。 |
| transfer | ArrayBuffer[] | 是   | 表示可转移的ArrayBuffer实例对象数组，该数组中对象的所有权会被转移到Worker线程，在宿主线程中将会变为不可用，仅在Worker线程中可用，数组不可传入null。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                |
| -------- | ----------------------------------------- |
| 10200004 | The worker instance is not running.       |
| 10200006 | An exception occurred during serialization. |

**示例：**

```js
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts");

var buffer = new ArrayBuffer(8);
workerInstance.postMessage(buffer, [buffer]);
```

### postMessage<sup>9+</sup>

postMessage(message: Object, options?: PostMessageOptions): void

宿主线程通过转移对象所有权或者拷贝数据的方式向Worker线程发送消息。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名  | 类型                                      | 必填 | 说明                                                         |
| ------- | ----------------------------------------- | ---- | ------------------------------------------------------------ |
| message | Object                                    | 是   | 发送至Worker的数据，该数据对象必须是可序列化，序列化支持类型见[其他说明](#序列化支持类型)。 |
| options | [PostMessageOptions](#postmessageoptions) | 否   | 当填入该参数时，与传入ArrayBuffer[]的作用一致，该数组中对象的所有权会被转移到Worker线程，在宿主线程中将会变为不可用，仅在Worker线程中可用。<br>若不填入该参数，默认设置为 undefined，通过拷贝数据的方式传输信息到Worker线程。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                |
| -------- | ----------------------------------------- |
| 10200004 | The worker instance is not running.       |
| 10200006 | An exception occurred during serialization. |

**示例：**

```js
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts");

workerInstance.postMessage("hello world");

var buffer = new ArrayBuffer(8);
workerInstance.postMessage(buffer, [buffer]);
```

### on<sup>9+</sup>

on(type: string, listener: WorkerEventListener): void

向Worker添加一个事件监听，该接口与[addEventListener<sup>9+</sup>](#addeventlistener9)接口功能一致。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名   | 类型                                         | 必填 | 说明                   |
| -------- | -------------------------------------------- | ---- | ---------------------- |
| type     | string                                       | 是   | 监听的事件类型。       |
| listener | [WorkerEventListener](#workereventlistener9) | 是 | 回调的事件。回调事件。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                   |
| -------- | -------------------------------------------- |
| 10200004 | Worker instance is not running.              |
| 10200005 | The invoked API is not supported in workers. |

**示例：**

```js
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts");
workerInstance.on("alert", (e)=>{
    console.log("alert listener callback");
})
```


### once<sup>9+</sup>

once(type: string, listener: WorkerEventListener): void

向Worker添加一个事件监听，事件监听只执行一次便自动删除。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名   | 类型                                         | 必填 | 说明                   |
| -------- | -------------------------------------------- | ---- | ---------------------- |
| type     | string                                       | 是   | 监听的事件类型。       |
| listener | [WorkerEventListener](#workereventlistener9) | 是 | 回调的事件。回调事件。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                   |
| -------- | -------------------------------------------- |
| 10200004 | Worker instance is not running.              |
| 10200005 | The invoked API is not supported in workers. |

**示例：**

```js
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts");
workerInstance.once("alert", (e)=>{
    console.log("alert listener callback");
})
```


### off<sup>9+</sup>

off(type: string, listener?: WorkerEventListener): void

删除类型为type的事件监听，该接口与[removeEventListener<sup>9+</sup>](#removeeventlistener9)接口功能一致。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名   | 类型                                         | 必填 | 说明                         |
| -------- | -------------------------------------------- | ---- | ---------------------------- |
| type     | string                                       | 是   | 需要删除的事件类型。         |
| listener | [WorkerEventListener](#workereventlistener9) | 否 | 回调的事件。删除的回调事件。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                   |
| -------- | -------------------------------------------- |
| 10200004 | Worker instance is not running.              |
| 10200005 | The invoked API is not supported in workers. |

**示例：**

```js
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts");
//使用on接口、once接口或addEventListener接口创建“alert”事件，使用off接口删除事件。
workerInstance.off("alert");
```


### terminate<sup>9+</sup>

terminate(): void

销毁Worker线程，终止Worker接收消息。

**系统能力：** SystemCapability.Utils.Lang

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                      |
| -------- | ------------------------------- |
| 10200004 | Worker instance is not running. |

**示例：**

```js
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts");
workerInstance.terminate();
```


### onexit<sup>9+</sup>

onexit?: (code: number) =&gt; void

Worker对象的onexit属性表示Worker销毁时被调用的事件处理程序，处理程序在宿主线程中执行。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名 | 类型   | 必填 | 说明               |
| ------ | ------ | ---- | ------------------ |
| code   | number | 是   | Worker退出的code。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                   |
| -------- | -------------------------------------------- |
| 10200004 | The worker instance is not running.          |
| 10200005 | The called API is not supported in the worker thread. |

**示例：**

```js
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts");
workerInstance.onexit = function(e) {
    console.log("onexit");
}

//onexit被执行两种方式：
// main thread：
workerInstance.terminate();

// worker线程：
//parentPort.close()
```


### onerror<sup>9+</sup>

onerror?: (err: ErrorEvent) =&gt; void

Worker对象的onerror属性表示Worker在执行过程中发生异常被调用的事件处理程序，处理程序在宿主线程中执行。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名 | 类型                      | 必填 | 说明       |
| ------ | ------------------------- | ---- | ---------- |
| err    | [ErrorEvent](#errorevent) | 是   | 异常数据。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                   |
| -------- | -------------------------------------------- |
| 10200004 | The worker instance is not running.          |
| 10200005 | The called API is not supported in the worker thread. |

**示例：**

```js
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts");
workerInstance.onerror = function(e) {
    console.log("onerror");
}
```


### onmessage<sup>9+</sup>

onmessage?: (event: MessageEvents) =&gt; void

Worker对象的onmessage属性表示宿主线程接收到来自其创建的Worker通过parentPort.postMessage接口发送的消息时被调用的事件处理程序，处理程序在宿主线程中执行。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名 | 类型                             | 必填 | 说明                   |
| ------ | -------------------------------- | ---- | ---------------------- |
| event  | [MessageEvents](#messageevents9) | 是   | 收到的Worker消息数据。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                   |
| -------- | -------------------------------------------- |
| 10200004 | The worker instance is not running.          |
| 10200005 | The called API is not supported in the worker thread. |

**示例：**

```js
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts");
workerInstance.onmessage = function(e) {
    // e : MessageEvents, 用法如下：
    // let data = e.data;
    console.log("onmessage");
}
```


### onmessageerror<sup>9+</sup>

onmessageerror?: (event: MessageEvents) =&gt; void

Worker对象的onmessageerror属性表示当Worker对象接收到一条无法被序列化的消息时被调用的事件处理程序，处理程序在宿主线程中执行。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名 | 类型                             | 必填 | 说明       |
| ------ | -------------------------------- | ---- | ---------- |
| event  | [MessageEvents](#messageevents9) | 是   | 异常数据。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                   |
| -------- | -------------------------------------------- |
| 10200004 | The worker instance is not running.          |
| 10200005 | The called API is not supported in the worker thread. |

**示例：**

```js
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts");
workerInstance.onmessageerror= function(e) {
    console.log("onmessageerror");
}
```

### addEventListener<sup>9+</sup>

addEventListener(type: string, listener: WorkerEventListener): void

向Worker添加一个事件监听，该接口与[on<sup>9+</sup>](#on9)接口功能一致。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名   | 类型                                         | 必填 | 说明             |
| -------- | -------------------------------------------- | ---- | ---------------- |
| type     | string                                       | 是   | 监听的事件类型。 |
| listener | [WorkerEventListener](#workereventlistener9) | 是   | 回调的事件。     |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                   |
| -------- | -------------------------------------------- |
| 10200004 | The worker instance is not running.          |
| 10200005 | The called API is not supported in the worker thread. |

**示例：**

```js
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts");
workerInstance.addEventListener("alert", (e)=>{
    console.log("alert listener callback");
})
```


### removeEventListener<sup>9+</sup>

removeEventListener(type: string, callback?: WorkerEventListener): void

删除Worker的事件监听，该接口与[off<sup>9+</sup>](#off9)接口功能一致。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名   | 类型                                         | 必填 | 说明                         |
| -------- | -------------------------------------------- | ---- | ---------------------------- |
| type     | string                                       | 是   | 需要删除的监听事件类型。     |
| callback | [WorkerEventListener](#workereventlistener9) | 否 | 回调的事件。删除的回调事件。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                      |
| -------- | ------------------------------- |
| 10200004 | The worker instance is not running. |

**示例：**

```js
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts");
workerInstance.addEventListener("alert", (e)=>{
    console.log("alert listener callback");
})
workerInstance.removeEventListener("alert");
```


### dispatchEvent<sup>9+</sup>

dispatchEvent(event: Event): boolean

分发定义在Worker的事件。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名 | 类型            | 必填 | 说明             |
| ------ | --------------- | ---- | ---------------- |
| event  | [Event](#event) | 是   | 需要分发的事件。 |

**返回值：**

| 类型    | 说明                            |
| ------- | ------------------------------- |
| boolean | 分发的结果，false表示分发失败。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                      |
| -------- | ------------------------------- |
| 10200004 | The worker instance is not running. |

**示例：**

```js
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts");

workerInstance.dispatchEvent({type:"eventType", timeStamp:0}); //timeStamp暂未支持。
```

分发事件（dispatchEvent）可与监听接口（on、once、addEventListener）搭配使用，示例如下：

```js
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts");

//用法一:
workerInstance.on("alert_on", (e)=>{
    console.log("alert listener callback");
})
workerInstance.once("alert_once", (e)=>{
    console.log("alert listener callback");
})
workerInstance.addEventListener("alert_add", (e)=>{
    console.log("alert listener callback");
})

//once接口创建的事件执行一次便会删除。
workerInstance.dispatchEvent({type:"alert_once", timeStamp:0});//timeStamp暂未支持。
//on接口创建的事件可以一直被分发，不能主动删除。
workerInstance.dispatchEvent({type:"alert_on", timeStamp:0});
workerInstance.dispatchEvent({type:"alert_on", timeStamp:0});
//addEventListener接口创建的事件可以一直被分发，不能主动删除。
workerInstance.dispatchEvent({type:"alert_add", timeStamp:0});
workerInstance.dispatchEvent({type:"alert_add", timeStamp:0});

//用法二:
//event类型的type支持自定义，同时存在"message"/"messageerror"/"error"特殊类型，如下所示
//当type = "message"，onmessage接口定义的方法同时会执行。
//当type = "messageerror"，onmessageerror接口定义的方法同时会执行。
//当type = "error"，onerror接口定义的方法同时会执行。
//若调用removeEventListener接口或者off接口取消事件时，能且只能取消使用addEventListener/on/once创建的事件。

workerInstance.addEventListener("message", (e)=>{
    console.log("message listener callback");
})
workerInstance.onmessage = function(e) {
    console.log("onmessage : message listener callback");
}
//调用dispatchEvent分发“message”事件，addEventListener和onmessage中定义的方法都会被执行。
workerInstance.dispatchEvent({type:"message", timeStamp:0});
```


### removeAllListener<sup>9+</sup>

removeAllListener(): void

删除Worker所有的事件监听。

**系统能力：** SystemCapability.Utils.Lang

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                      |
| -------- | ------------------------------- |
| 10200004 | The worker instance is not running. |

**示例：**

```js
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts");
workerInstance.addEventListener("alert", (e)=>{
    console.log("alert listener callback");
})
workerInstance.removeAllListener();
```

## WorkerEventTarget<sup>9+</sup>

### addEventListener<sup>9+</sup>

addEventListener(type: string, listener: WorkerEventListener): void

向Worker添加一个事件监听，该接口与[on<sup>9+</sup>](#on9)接口功能一致。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名   | 类型                                         | 必填 | 说明             |
| -------- | -------------------------------------------- | ---- | ---------------- |
| type     | string                                       | 是   | 监听的事件类型。 |
| listener | [WorkerEventListener](#workereventlistener9) | 是   | 回调的事件。     |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                   |
| -------- | -------------------------------------------- |
| 10200004 | Worker instance is not running.              |
| 10200005 | The invoked API is not supported in workers. |

**示例：**

```js
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts");
workerInstance.addEventListener("alert", (e)=>{
    console.log("alert listener callback");
})
```


### removeEventListener<sup>9+</sup>

removeEventListener(type: string, callback?: WorkerEventListener): void

删除Worker的事件监听，该接口与[off<sup>9+</sup>](#off9)接口功能一致。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名   | 类型                                         | 必填 | 说明                         |
| -------- | -------------------------------------------- | ---- | ---------------------------- |
| type     | string                                       | 是   | 需要删除的监听事件类型。     |
| callback | [WorkerEventListener](#workereventlistener9) | 否 | 回调的事件。删除的回调事件。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                      |
| -------- | ------------------------------- |
| 10200004 | Worker instance is not running. |

**示例：**

```js
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts");
workerInstance.addEventListener("alert", (e)=>{
    console.log("alert listener callback");
})
workerInstance.removeEventListener("alert");
```


### dispatchEvent<sup>9+</sup>

dispatchEvent(event: Event): boolean

分发定义在Worker的事件。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名 | 类型            | 必填 | 说明             |
| ------ | --------------- | ---- | ---------------- |
| event  | [Event](#event) | 是   | 需要分发的事件。 |

**返回值：**

| 类型    | 说明                            |
| ------- | ------------------------------- |
| boolean | 分发的结果，false表示分发失败。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                      |
| -------- | ------------------------------- |
| 10200004 | Worker instance is not running. |

**示例：**

```js
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts");

workerInstance.dispatchEvent({type:"eventType", timeStamp:0}); //timeStamp暂未支持。
```

分发事件（dispatchEvent）可与监听接口（on、once、addEventListener）搭配使用，示例如下：

```js
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts");

//用法一:
workerInstance.on("alert_on", (e)=>{
    console.log("alert listener callback");
})
workerInstance.once("alert_once", (e)=>{
    console.log("alert listener callback");
})
workerInstance.addEventListener("alert_add", (e)=>{
    console.log("alert listener callback");
})

//once接口创建的事件执行一次便会删除。
workerInstance.dispatchEvent({type:"alert_once", timeStamp:0});//timeStamp暂未支持。
//on接口创建的事件可以一直被分发，不能主动删除。
workerInstance.dispatchEvent({type:"alert_on", timeStamp:0});
workerInstance.dispatchEvent({type:"alert_on", timeStamp:0});
//addEventListener接口创建的事件可以一直被分发，不能主动删除。
workerInstance.dispatchEvent({type:"alert_add", timeStamp:0});
workerInstance.dispatchEvent({type:"alert_add", timeStamp:0});

//用法二:
//event类型的type支持自定义，同时存在"message"/"messageerror"/"error"特殊类型，如下所示
//当type = "message"，onmessage接口定义的方法同时会执行。
//当type = "messageerror"，onmessageerror接口定义的方法同时会执行。
//当type = "error"，onerror接口定义的方法同时会执行。
//若调用removeEventListener接口或者off接口取消事件时，能且只能取消使用addEventListener/on/once创建的事件。

workerInstance.addEventListener("message", (e)=>{
    console.log("message listener callback");
})
workerInstance.onmessage = function(e) {
    console.log("onmessage : message listener callback");
}
//调用dispatchEvent分发“message”事件，addEventListener和onmessage中定义的方法都会被执行。
workerInstance.dispatchEvent({type:"message", timeStamp:0});
```


### removeAllListener<sup>9+</sup>

removeAllListener(): void

删除Worker所有的事件监听。

**系统能力：** SystemCapability.Utils.Lang

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                      |
| -------- | ------------------------------- |
| 10200004 | Worker instance is not running. |

**示例：**

```js
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts");
workerInstance.addEventListener("alert", (e)=>{
    console.log("alert listener callback");
})
workerInstance.removeAllListener();
```


## ThreadWorkerGlobalScope<sup>9+</sup>

Worker线程用于与宿主线程通信的类，通过postMessage接口发送消息给宿主线程、close接口销毁Worker线程。ThreadWorkerGlobalScope类继承[GlobalScope<sup>9+</sup>](#globalscope9)。

### postMessage<sup>9+</sup>

postMessage(messageObject: Object, transfer: ArrayBuffer[]): void;

Worker线程通过转移对象所有权的方式向宿主线程发送消息。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名   | 类型          | 必填 | 说明                                                         |
| -------- | ------------- | ---- | ------------------------------------------------------------ |
| message  | Object        | 是   | 发送至宿主线程的数据，该数据对象必须是可序列化，序列化支持类型见[其他说明](#序列化支持类型)。 |
| transfer | ArrayBuffer[] | 是   | 表示可转移的ArrayBuffer实例对象数组，该数组中对象的所有权会被转移到宿主线程，在Worker线程中将会变为不可用，仅在宿主线程中可用，数组不可传入null。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                |
| -------- | ----------------------------------------- |
| 10200004 | The worker instance is not running.       |
| 10200006 | An exception occurred during serialization. |

**示例：**

```js
// main thread
import worker from '@ohos.worker';
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts");
workerInstance.postMessage("hello world");
workerInstance.onmessage = function(e) {
    // let data = e.data;
    console.log("receive data from worker.ts");
}
```

```js
// worker.ts
import worker from '@ohos.worker';
const workerPort = worker.workerPort;
workerPort.onmessage = function(e){
    // let data = e.data;
    var buffer = new ArrayBuffer(8);
    workerPort.postMessage(buffer, [buffer]);
}
```

### postMessage<sup>9+</sup>

postMessage(messageObject: Object, options?: PostMessageOptions): void

Worker线程通过转移对象所有权或者拷贝数据的方式向宿主线程发送消息。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名  | 类型                                      | 必填 | 说明                                                         |
| ------- | ----------------------------------------- | ---- | ------------------------------------------------------------ |
| message | Object                                    | 是   | 发送至宿主线程的数据，该数据对象必须是可序列化，序列化支持类型见[其他说明](#序列化支持类型)。 |
| options | [PostMessageOptions](#postmessageoptions) | 否   | 当填入该参数时，与传入ArrayBuffer[]的作用一致，该数组中对象的所有权会被转移到宿主线程，在Worker线程中将会变为不可用，仅在宿主线程中可用。<br/>若不填入该参数，默认设置为 undefined，通过拷贝数据的方式传输信息到宿主线程。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                |
| -------- | ----------------------------------------- |
| 10200004 | The worker instance is not running.       |
| 10200006 | An exception occurred during serialization. |

**示例：**

```js
// main thread
import worker from '@ohos.worker';
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts");
workerInstance.postMessage("hello world");
workerInstance.onmessage = function(e) {
    // let data = e.data;
    console.log("receive data from worker.ts");
}
```

```js
// worker.ts
import worker from '@ohos.worker';
const workerPort = worker.workerPort;
workerPort.onmessage = function(e){
    // let data = e.data;
    workerPort.postMessage("receive data from main thread");
}
```


### close<sup>9+</sup>

close(): void

销毁Worker线程，终止Worker接收消息。

**系统能力：** SystemCapability.Utils.Lang

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                      |
| -------- | ------------------------------- |
| 10200004 | The worker instance is not running. |

**示例：**

```js
// main thread
import worker from '@ohos.worker';
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts");
```

```js
// worker.ts
import worker from '@ohos.worker';
const workerPort = worker.workerPort;
workerPort.onmessage = function(e) {
    workerPort.close()
}
```


### onmessage<sup>9+</sup>

onmessage?: (this: ThreadWorkerGlobalScope, ev: MessageEvents) =&gt; void

ThreadWorkerGlobalScope的onmessage属性表示Worker线程收到来自其宿主线程通过postMessage接口发送的消息时被调用的事件处理程序，处理程序在Worker线程中执行。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名 | 类型                                                 | 必填 | 说明                     |
| ------ | ---------------------------------------------------- | ---- | ------------------------ |
| this   | [ThreadWorkerGlobalScope](#threadworkerglobalscope9) | 是   | 指向调用者对象。         |
| ev     | [MessageEvents](#messageevents9)                     | 是   | 收到宿主线程发送的数据。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                   |
| -------- | -------------------------------------------- |
| 10200004 | The worker instance is not running.          |
| 10200005 | The called API is not supported in the worker thread. |

**示例：**

```js
// main thread
import worker from '@ohos.worker';
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts");
workerInstance.postMessage("hello world");
```

```js
// worker.ts
import worker from '@ohos.worker';
const workerPort = worker.workerPort;
workerPort.onmessage = function(e) {
    console.log("receive main thread message");
}
```


### onmessageerror<sup>9+</sup>

onmessageerror?: (this: ThreadWorkerGlobalScope, ev: MessageEvents) =&gt; void

ThreadWorkerGlobalScope的onmessageerror属性表示当Worker对象接收到一条无法被反序列化的消息时被调用的事件处理程序，处理程序在Worker线程中执行。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名 | 类型                             | 必填 | 说明       |
| ------ | -------------------------------- | ---- | ---------- |
| this   | [ThreadWorkerGlobalScope](#threadworkerglobalscope9) | 是   | 指向调用者对象。         |
| ev     | [MessageEvents](#messageevents9) | 是   | 异常数据。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                   |
| -------- | -------------------------------------------- |
| 10200004 | The worker instance is not running.          |
| 10200005 | The called API is not supported in the worker thread. |

**示例：**

```js
// main thread
import worker from '@ohos.worker';
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts");
```

```js
// worker.ts
import worker from '@ohos.worker';
const parentPort = worker.workerPort;
parentPort.onmessageerror = function(e) {
    console.log("worker.ts onmessageerror")
}
```


## WorkerEventListener<sup>9+</sup>

(event: Event): void | Promise&lt;void&gt;

事件监听类。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名 | 类型            | 必填 | 说明           |
| ------ | --------------- | ---- | -------------- |
| event  | [Event](#event) | 是   | 回调的事件类。 |

**返回值：**

| 类型                                  | 说明                            |
| ------------------------------------- | ------------------------------- |
| void&nbsp;\|&nbsp;Promise&lt;void&gt; | 无返回值或者以Promise形式返回。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                   |
| -------- | -------------------------------------------- |
| 10200004 | Worker instance is not running.              |
| 10200005 | The invoked API is not supported in workers. |

**示例：**

```js
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts");
workerInstance.addEventListener("alert", (e)=>{
    console.log("alert listener callback");
})
```


## GlobalScope<sup>9+</sup>

Worker线程自身的运行环境，GlobalScope类继承[WorkerEventTarget](#workereventtarget9)。

### 属性

**系统能力：** SystemCapability.Utils.Lang

| 名称 | 类型                                                         | 可读 | 可写 | 说明                                  |
| ---- | ------------------------------------------------------------ | ---- | ---- | ------------------------------------- |
| name | string                                                       | 是   | 否   | Worker的名字，new&nbsp;Worker时指定。 |
| self | [GlobalScope](#globalscope9)&nbsp;&amp;&nbsp;typeof&nbsp;globalThis | 是   | 否   | GlobalScope本身。                     |


### onerror<sup>9+</sup>

onerror?: (ev: ErrorEvent) =&gt; void

GlobalScope的onerror属性表示Worker在执行过程中发生异常被调用的事件处理程序，处理程序在Worker线程中执行。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名 | 类型                      | 必填 | 说明       |
| ------ | ------------------------- | ---- | ---------- |
| ev     | [ErrorEvent](#errorevent) | 是   | 异常数据。 |

**示例：**

```js
// main thread
import worker from '@ohos.worker';
const workerInstance = new worker.ThreadWorker("entry/ets/workers/worker.ts")
```

```js
// worker.ts
import worker from '@ohos.worker';
const workerPort = worker.workerPort
workerPort.onerror = function(e){
    console.log("worker.ts onerror")
}
```

## MessageEvents<sup>9+</sup>

消息类，持有Worker线程间传递的数据。

**系统能力：** SystemCapability.Utils.Lang

| 名称 | 类型 | 可读 | 可写 | 说明               |
| ---- | ---- | ---- | ---- | ------------------ |
| data | any  | 是   | 否   | 线程间传递的数据。 |


## PostMessageOptions

明确数据传递过程中需要转移所有权对象的类，传递所有权的对象必须是ArrayBuffer，发送它的上下文中将会变为不可用，仅在接收方可用。

**系统能力：** SystemCapability.Utils.Lang

| 名称     | 类型     | 可读 | 可写 | 说明                              |
| -------- | -------- | ---- | ---- | --------------------------------- |
| transfer | Object[] | 是   | 是   | ArrayBuffer数组，用于传递所有权。该数组中不可传入null。 |


## Event

事件类。

**系统能力：** SystemCapability.Utils.Lang

| 名称      | 类型   | 可读 | 可写 | 说明                                         |
| --------- | ------ | ---- | ---- | -------------------------------------------- |
| type      | string | 是   | 否   | 指定事件的类型。                             |
| timeStamp | number | 是   | 否   | 事件创建时的时间戳（精度为毫秒），暂未支持。 |


## ErrorEvent

错误事件类，用于表示Worker执行过程中出现异常的详细信息，ErrorEvent类继承[Event](#event)。

**系统能力：** SystemCapability.Utils.Lang

| 名称     | 类型   | 可读 | 可写 | 说明                 |
| -------- | ------ | ---- | ---- | -------------------- |
| message  | string | 是   | 否   | 异常发生的错误信息。 |
| filename | string | 是   | 否   | 出现异常所在的文件。 |
| lineno   | number | 是   | 否   | 异常所在的行数。     |
| colno    | number | 是   | 否   | 异常所在的列数。     |
| error    | Object | 是   | 否   | 异常类型。           |


## MessageEvent\<T\>

消息类，持有Worker线程间传递的数据。

**系统能力：** SystemCapability.Utils.Lang

| 名称 | 类型 | 可读 | 可写 | 说明               |
| ---- | ---- | ---- | ---- | ------------------ |
| data | T    | 是   | 否   | 线程间传递的数据。 |


## 其他说明

### 序列化支持类型
| Type               | 备注                                   | 是否支持 |
| ------------------ | -------------------------------------- | -------- |
| All Primitive Type | 不包括symbol                           | 是       |
| Date               |                                        | 是       |
| String             |                                        | 是       |
| RegExp             |                                        | 是       |
| Array              |                                        | 是       |
| Map                |                                        | 是       |
| Set                |                                        | 是       |
| Object             | 只支持Plain Object，不支持带function的 | 是       |
| ArrayBuffer        | 提供transfer能力                       | 是       |
| TypedArray         |                                        | 是       |

特例：传递通过自定义class创建出来的object时，不会发生序列化错误，但是自定义class的属性（如Function）无法通过序列化传递。
> **说明：**<br/>
> 以API version 9的FA工程为例。

```js
// main thread
import worker from '@ohos.worker';
const workerInstance = new worker.ThreadWorker("workers/worker.ts");
workerInstance.postMessage("message from main thread to worker");
workerInstance.onmessage = function(d) {
  // 当worker线程传递obj2时，data即为obj2。data没有Init、SetName的方法
  let data = d.data;
}
```
```js
// worker.ts
import worker from '@ohos.worker';
const workerPort = worker.workerPort;
class MyModel {
    name = "undefined"
    Init() {
        this.name = "MyModel"
    }
}
workerPort.onmessage = function(d) {
    console.log("worker.ts onmessage");
    let data = d.data;
    let func1 = function() {
        console.log("post message is function");
    }
    let obj1 = {
        "index": 2,
        "name1": "zhangshan",
        setName() {
            this.index = 3;
        }
    }
    let obj2 = new MyModel();
    // workerPort.postMessage(func1); 传递func1发生序列化错误
    // workerPort.postMessage(obj1);  传递obj1发生序列化错误
    workerPort.postMessage(obj2);     // 传递obj2不会发生序列化错误
}
workerPort.onmessageerror = function(e) {
    console.log("worker.ts onmessageerror");
}
workerPort.onerror = function(e) {
    console.log("worker.ts onerror");
}
```

### 内存模型
Worker基于Actor并发模型实现。在Worker的交互流程中，JS主线程可以创建多个Worker子线程，各个Worker线程间相互隔离，并通过序列化传递对象，等到Worker线程完成计算任务，再把结果返回给主线程。 

Actor并发模型的交互原理：各个Actor并发地处理主线程任务，每个Actor内部都有一个消息队列及单线程执行模块，消息队列负责接收主线程及其他Actor的请求，单线程执行模块则负责串行地处理请求、向其他Actor发送请求以及创建新的Actor。由于Actor采用的是异步方式，各个Actor之间相互隔离没有数据竞争，因此Actor可以高并发运行。

### 注意事项
- Worker存在数量限制，当前支持最多同时存在8个Worker。
- 在API version 8及之前的版本，当Worker数量超出限制时，会抛出错误Error "Too many workers, the number of workers exceeds the maximum."。
- 从API version 9开始，当Worker数量超出限制时，会抛出错误BusinessError "Worker initialization failure, the number of workers exceeds the maximum."。
- 主动销毁Worker可以调用新创建Worker对象的terminate()或parentPort.close()方法。
- 自API version 9版本开始，若Worker处于已经销毁或正在销毁等非运行状态时，调用其功能接口，会抛出相应的BusinessError。
- Worker的创建和销毁耗费性能，建议管理已创建的Worker并重复使用。
- 创建Worker工程时，在Worker线程的文件中（比如本文中worker.ts）不能导入任何有关构建UI的方法（比如ets文件等），否则会导致Worker的功能失效。排查方式：解压生成的Hap包，在创建Worker线程的文件目录中找到"worker.js"，全局搜索"View"关键字。如果存在该关键字，说明在worker.js中打包进去了构建UI的方法，会导致Worker的功能失效，建议在创建Worker线程的文件中修改 "import “xxx” from src"中src的目录层级。
- 线程间通信时传递的数据量最大限制为16M。

## 完整示例
> **说明：**<br/>
> 以API version 9的工程为例。<br> API version 8及之前的版本仅支持FA模型，如需使用，注意更换构造Worker的接口和创建worker线程中与主线程通信的对象的两个方法。
### FA模型

```js
// main thread(同级目录为例)
import worker from '@ohos.worker';
// 主线程中创建Worker对象
const workerInstance = new worker.ThreadWorker("workers/worker.ts");

// 主线程向worker线程传递信息
workerInstance.postMessage("123");

// 主线程接收worker线程信息
workerInstance.onmessage = function(e) {
    // data：worker线程发送的信息
    let data = e.data;
    console.log("main thread onmessage");

    // 销毁Worker对象
    workerInstance.terminate();
}

// 在调用terminate后，执行回调onexit
workerInstance.onexit = function() {
    console.log("main thread terminate");
}
```
```js
// worker.ts
import worker from '@ohos.worker';

// 创建worker线程中与主线程通信的对象
const workerPort = worker.workerPort

// worker线程接收主线程信息
workerPort.onmessage = function(e) {
    // data：主线程发送的信息
    let data = e.data;
    console.log("worker.ts onmessage");

    // worker线程向主线程发送信息
    workerPort.postMessage("123")
}

// worker线程发生error的回调
workerPort.onerror= function(e) {
    console.log("worker.ts onerror");
}
```
build-profile.json5 配置 :
```json
  "buildOption": {
    "sourceOption": {
      "workers": [
        "./src/main/ets/entryability/workers/worker.ts"
      ]
    }
  }
```
### Stage模型
```js
// main thread（以不同目录为例）
import worker from '@ohos.worker';

// 主线程中创建Worker对象
const workerInstance = new worker.ThreadWorker("entry/ets/pages/workers/worker.ts");

// 主线程向worker线程传递信息
workerInstance.postMessage("123");

// 主线程接收worker线程信息
workerInstance.onmessage = function(e) {
    // data：worker线程发送的信息
    let data = e.data;
    console.log("main thread onmessage");

    // 销毁Worker对象
    workerInstance.terminate();
}
// 在调用terminate后，执行onexit
workerInstance.onexit = function() {
    console.log("main thread terminate");
}
```
```js
// worker.ts
import worker from '@ohos.worker';

// 创建worker线程中与主线程通信的对象
const workerPort = worker.workerPort

// worker线程接收主线程信息
workerPort.onmessage = function(e) {
    // data：主线程发送的信息
    let data = e.data;
    console.log("worker.ts onmessage");

    // worker线程向主线程发送信息
    workerPort.postMessage("123")
}

// worker线程发生error的回调
workerPort.onerror= function(e) {
    console.log("worker.ts onerror");
}
```
build-profile.json5 配置:
```json
  "buildOption": {
    "sourceOption": {
      "workers": [
        "./src/main/ets/pages/workers/worker.ts"
      ]
    }
  }
```
<!--no_check-->