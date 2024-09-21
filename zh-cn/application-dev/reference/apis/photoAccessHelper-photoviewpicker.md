# 使用Picker选择媒体库资源

用户有时需要分享图片、视频等用户文件，开发者可以通过特定接口拉起系统图库，用户自行选择待分享的资源，然后最终分享出去。此接口本身无需申请权限，目前适用于界面UIAbility，使用窗口组件触发。具体使用方式如下：

1. 导入选择器模块和文件管理模块。

   ```ts
   import { photoAccessHelper } from '@kit.MediaLibraryKit';
   import { fileIo } from '@kit.CoreFileKit';
   import { BusinessError } from '@kit.BasicServicesKit';
   ```

2. 创建图片-音频类型文件选择选项实例。

   ```ts
   const photoSelectOptions = new photoAccessHelper.PhotoSelectOptions();
   ```

3. 选择媒体文件类型和选择媒体文件的最大数目。
   以下示例以图片选择为例，媒体文件类型请参见[PhotoViewMIMETypes](js-apis-photoAccessHelper.md#photoviewmimetypes)。

   ```ts
   photoSelectOptions.MIMEType = photoAccessHelper.PhotoViewMIMETypes.IMAGE_TYPE; // 过滤选择媒体文件类型为IMAGE
   ```

4. 创建图库选择器实例，调用[PhotoViewPicker.select](js-apis-photoAccessHelper.md#select)接口拉起图库界面进行文件选择。文件选择成功后，返回[PhotoSelectResult](js-apis-photoAccessHelper.md#photoselectresult)结果集。

   ```ts
   let uris: Array<string> = [];
   const photoViewPicker = new photoAccessHelper.PhotoViewPicker();
   photoViewPicker.select(photoSelectOptions).then((photoSelectResult: photoAccessHelper.PhotoSelectResult) => {
     uris = photoSelectResult.photoUris;
     console.info('photoViewPicker.select to file succeed and uris are:' + uris);
   }).catch((err: BusinessError) => {
     console.error(`Invoke photoViewPicker.select failed, code is ${err.code}, message is ${err.message}`);
   })
   ```

   select返回的uri权限是只读权限，可以根据结果集中uri进行读取文件数据操作。注意不能在picker的回调里直接使用此uri进行打开文件操作，需要定义一个全局变量保存uri，使用类似一个按钮去触发打开文件。可参考[指定URI读取文件数据](#指定uri读取文件数据)。

## 指定URI读取文件数据

1. 待界面从图库返回后，再通过类似一个按钮调用其他函数，使用[fileIo.openSync](js-apis-file-fs.md#fsopensync)接口，通过uri打开这个文件得到fd。这里需要注意接口权限参数是fileIo.OpenMode.READ_ONLY。

   ```ts
   let uri: string = '';
   let file = fileIo.openSync(uri, fileIo.OpenMode.READ_ONLY);
   console.info('file fd: ' + file.fd);
   ```

2. 通过fd使用[fileIo.readSync](js-apis-file-fs.md#readSync)接口读取这个文件内的数据，读取完成后关闭fd。

   ```ts
   let buffer = new ArrayBuffer(4096);
   let readLen = fileIo.readSync(file.fd, buffer);
   console.info('readSync data to file succeed and buffer size is:' + readLen);
   fileIo.closeSync(file);
   ```
