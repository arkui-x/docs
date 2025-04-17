# @ohos.arkui.Prefetcher (Prefetching)

配合LazyForEach，为List、Grid、Waterfall和Swiper等容器组件滑动浏览时提供内容预加载能力，提升用户浏览体验。

>  **说明：**
>
>  本模块首批接口从API version 12开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。

## 导入模块

```ts
import { BasicPrefetcher, IDataSourcePrefetching, IPrefetcher } from '@kit.ArkUI';
```

## IPrefetcher

实现此接口以提供预取能力。

**支持平台**：Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

### setDataSource

setDataSource(dataSource: IDataSourcePrefetching): void;

设置支持预取的DataSource以绑定到Prefetcher

**支持平台**：Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**参数：**

| 参数名     | 类型                                           | 必填 | 说明                   |
| ---------- | ---------------------------------------------- | ---- | ---------------------- |
| dataSource | [IDataSourcePrefetching](#idatasourceprefetching) | 是   | 支持预取能力的数据源。 |

```typescript
class MyPrefetcher implements IPrefetcher {
  private dataSource?: IDataSourcePrefetching;

  setDataSource(dataSource: IDataSourcePrefetching): void {
    this.dataSource = dataSource;
  }

  visibleAreaChanged(minVisible: number, maxVisible: number): void {
    this.dataSource?.prefetch(minVisible);
  }
}
```

### visibleAreaChanged

visibleAreaChanged(minVisible: number, maxVisible: number): void;

当可见区域边界发生改变时调用此方法。支持与 `List`、`Grid`、`Waterfall`和 `Swiper`组件配合使用

**支持平台**：Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**参数：**

| 参数名     | 类型   | 必填 | 说明                 |
| ---------- | ------ | ---- | -------------------- |
| minVisible | number | 是   | 列表可见区域的上界。 |
| maxVisible | number | 是   | 列表可见区域的下界。 |

```typescript
class MyPrefetcher implements IPrefetcher {
  private dataSource?: IDataSourcePrefetching;

  setDataSource(dataSource: IDataSourcePrefetching): void {
    this.dataSource = dataSource;
  }

  visibleAreaChanged(minVisible: number, maxVisible: number): void {
    this.dataSource?.prefetch(minVisible);
  }
}
```

## BasicPrefetcher

BasicPrefetcher是IPrefetcher的基础实现。它提供了一种智能数据预取算法，以根据屏幕上可见区域的实时变化和预取持续时间的变化来决定应预取哪些数据项。它还可以根据用户的滚动操作来确定哪些预取请求应该被取消。

**支持平台**：Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

### constructor

constructor(dataSource?: IDataSourcePrefetching);

传入支持预取的DataSource以绑定到Prefetcher

**支持平台**：Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**参数：**

| 参数名     | 类型                                           | 必填 | 说明                   |
| ---------- | ---------------------------------------------- | ---- | ---------------------- |
| dataSource | [IDataSourcePrefetching](#idatasourceprefetching) | 否   | 支持预取能力的数据源。 |

### setDataSource

setDataSource(dataSource: IDataSourcePrefetching): void;

设置支持预取的DataSource以绑定到Prefetcher

**支持平台**：Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**参数：**

| 参数名     | 类型                                           | 必填 | 说明                   |
| ---------- | ---------------------------------------------- | ---- | ---------------------- |
| dataSource | [IDataSourcePrefetching](#idatasourceprefetching) | 是   | 支持预取能力的数据源。 |

### visibleAreaChanged

visibleAreaChanged(minVisible: number, maxVisible: number): void;

当可见区域边界发生改变时调用此方法。支持与 `List`、`Grid`、`Waterfall`和 `Swiper`组件配合使用

**支持平台**：Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**参数：**

| 参数名     | 类型   | 必填 | 说明                 |
| ---------- | ------ | ---- | -------------------- |
| minVisible | number | 是   | 列表可见区域的上界。 |
| maxVisible | number | 是   | 列表可见区域的下界。 |

## IDataSourcePrefetching

继承自[IDataSource](../../../application-dev/reference/arkui-ts/arkts-rendering-control-lazyforeach.md)。实现该接口，提供具备预取能力的DataSource。

**支持平台**：Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

### prefetch

prefetch(index: number): Promise\<void\> | void;

从数据集中预取指定的元素。该方法可以为同步，也可为异步。

**支持平台**：Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**参数：**

| 参数名 | 类型   | 必填 | 说明               |
| ------ | ------ | ---- | ------------------ |
| index  | number | 是   | 预取数据项索引值。 |

### cancel

cancel?(index: number): Promise\<void\> | void;

取消从数据集中预取指定的元素。该方法可以为同步，也可为异步。

**支持平台**：Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**参数：**

| 参数名 | 类型   | 必填 | 说明                   |
| ------ | ------ | ---- | ---------------------- |
| index  | number | 是   | 取消预取数据项索引值。 |

列表内容移出屏幕时（比如列表快速滑动场景下），预取算法判断屏幕以外的Item可以被取消预取时，该方法即会被调用。例如，如果HTTP框架支持请求取消，则可以在此处取消在prefetch中发起的网络请求。

## 示例

下面示例展示了Prefetcher的预加载能力。该示例采用分页的方式，配合LazyForEach实现懒加载效果，并通过添加延时来模拟加载过程。

```typescript
import { BasicPrefetcher, IDataSourcePrefetching } from '@kit.ArkUI';
import { image } from '@kit.ImageKit';

const ITEMS_ON_SCREEN = 8;

@Entry
@Component
struct PrefetcherDemoComponent {
  private page: number = 1;
  private pageSize: number = 50;
  private breakPoint: number = 25;
  private readonly fetchDelayMs: number = 500;
  private readonly dataSource = new MyDataSource(this.page, this.pageSize, this.fetchDelayMs);
  private readonly prefetcher = new BasicPrefetcher(this.dataSource);

  build() {
    Column() {
      List() {
        LazyForEach(this.dataSource, (item: PictureItem, index: number) => {
          ListItem() {
            PictureItemComponent({ info: item })
              .height(`${100 / ITEMS_ON_SCREEN}%`)
          }
          .onAppear(() => {
            if (index >= this.breakPoint) {
              this.dataSource.getHttpData(++this.page, this.pageSize);
              this.breakPoint = this.dataSource.totalCount() - this.pageSize / 2;
            }
          })
        }, (item: PictureItem) => item.title)
      }
      .onScrollIndex((start: number, end: number) => {
        this.prefetcher.visibleAreaChanged(start, end);
      })
    }
  }
}

@Component
struct PictureItemComponent {
  @ObjectLink info: PictureItem;

  build() {
    Row() {
      Image(this.info.imagePixelMap)
        .objectFit(ImageFit.Contain)
        .width('40%')
      Text(this.info.title)
        .width('60%')
    }
  }
}

@Observed
class PictureItem {
  readonly color: number;
  title: string;
  imagePixelMap: image.PixelMap | undefined;
  key: string;

  constructor(color: number, title: string) {
    this.color = color;
    this.title = title;
    this.key = title;
  }
}

type ItemIndex = number;
type TimerId = number;

class MyDataSource implements IDataSourcePrefetching {
  private readonly items: PictureItem[];
  private readonly fetchDelayMs: number;
  private readonly fetches: Map<ItemIndex, TimerId> = new Map();
  private readonly listeners: DataChangeListener[] = [];

  constructor(pageNum: number, pageSize: number, fetchDelayMs: number) {
    this.items = [];
    this.fetchDelayMs = fetchDelayMs;
    this.getHttpData(pageNum, pageSize);
  }

  async prefetch(index: number): Promise<void> {
    const item = this.items[index];
    if (item.imagePixelMap) {
      return;
    }

    // 模拟高耗时操作
    return new Promise<void>(resolve => {
      const timeoutId = setTimeout(async () => {
        this.fetches.delete(index);
        const bitmap = create10x10Bitmap(item.color);
        const imageSource: image.ImageSource = image.createImageSource(bitmap);
        item.imagePixelMap = await imageSource.createPixelMap();
        resolve();
      }, this.fetchDelayMs);

      this.fetches.set(index, timeoutId)
    });
  }

  cancel(index: number): void {
    const timerId = this.fetches.get(index);
    if (timerId) {
      this.fetches.delete(index);
      clearTimeout(timerId);
    }
  }

  // 模拟分页方式加载数据
  getHttpData(pageNum: number, pageSize:number): void {
    const newItems: PictureItem[] = [];
    for (let i = (pageNum - 1) * pageSize; i < pageNum * pageSize; i++) {
      const item = new PictureItem(getRandomColor(), `Item ${i}`);
      newItems.push(item);
    }
    const startIndex = this.items.length;
    this.items.splice(startIndex, 0, ...newItems);
    this.notifyBatchUpdate([
      {
        type: DataOperationType.ADD,
        index: startIndex,
        count: newItems.length,
        key: newItems.map((item) => item.title)
      }
    ]);
  }

  private notifyBatchUpdate(operations: DataOperation[]) {
    this.listeners.forEach((listener: DataChangeListener) => {
      listener.onDatasetChange(operations);
    });
  }

  totalCount(): number {
    return this.items.length;
  }

  getData(index: number): PictureItem {
    return this.items[index];
  }

  registerDataChangeListener(listener: DataChangeListener): void {
    if (this.listeners.indexOf(listener) < 0) {
      this.listeners.push(listener);
    }
  }

  unregisterDataChangeListener(listener: DataChangeListener): void {
    const pos = this.listeners.indexOf(listener);
    if (pos >= 0) {
      this.listeners.splice(pos, 1);
    }
  }
}

function getRandomColor(): number {
  const maxColorCode = 256;
  const r = Math.floor(Math.random() * maxColorCode);
  const g = Math.floor(Math.random() * maxColorCode);
  const b = Math.floor(Math.random() * maxColorCode);

  return (r * 256 + g) * 256 + b;
}

function create10x10Bitmap(color: number): ArrayBuffer {
  const height = 10;
  const width = 10;

  const fileHeaderLength = 14;
  const bitmapInfoLength = 40;
  const headerLength = fileHeaderLength + bitmapInfoLength;
  const pixelSize = (width * 3 + 2) * height;

  let length = pixelSize + headerLength;

  const buffer = new ArrayBuffer(length);
  const view16 = new Uint16Array(buffer);

  view16[0] = 0x4D42;
  view16[1] = length & 0xffff;
  view16[2] = length >> 16;
  view16[5] = headerLength;

  let offset = 7;
  view16[offset++] = bitmapInfoLength & 0xffff;
  view16[offset++] = bitmapInfoLength >> 16;
  view16[offset++] = width & 0xffff;
  view16[offset++] = width >> 16;
  view16[offset++] = height & 0xffff;
  view16[offset++] = height >> 16;
  view16[offset++] = 1;
  view16[offset++] = 24;

  const b = color & 0xff;
  const g = (color >> 8) & 0xff;
  const r = color >> 16;
  offset = headerLength;
  const view8 = new Uint8Array(buffer);
  for (let y = 0; y < height; y++) {
    for (let x = 0; x < width; x++) {
      view8[offset++] = b;
      view8[offset++] = g;
      view8[offset++] = r;
    }
    offset += 2;
  }

  return buffer;
}
```

演示效果如下：

![Prefetcher-Demo](./figures/prefetcher-demo.gif)
