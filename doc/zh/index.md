<!---
    Licensed to the Apache Software Foundation (ASF) under one
    or more contributor license agreements.  See the NOTICE file
    distributed with this work for additional information
    regarding copyright ownership.  The ASF licenses this file
    to you under the Apache License, Version 2.0 (the
    "License"); you may not use this file except in compliance
    with the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing,
    software distributed under the License is distributed on an
    "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    KIND, either express or implied.  See the License for the
    specific language governing permissions and limitations
    under the License.
-->

# org.apache.cordova.media-capture

这个插件提供了对设备的音频、 图像和视频捕获功能的访问。

**WARNING**：为从设备的摄像头或麦克风中收集和使用的图像、 视频或音频， 提出了重要的隐私问题。 您的应用程序的隐私策略应该讨论，应用程序如何使用这种传感器和记录的数据是否与任何其他方面共享。 此外，如果摄像机或麦克风的应用程序的使用在用户界面中不是明显的，你应该在应用程序访问的相机或麦克风之前（如果设备操作系统已经不会这样做) 提供及时通知备注。 该通知应提供以上相同的信息备注，以及获取该用户的权限 （例如，通过为**OK**和**No Thanks**提出的选择）。 请注意有些应用程序市场可能需要您的应用程序来提供及时通知备注，并从访问摄像机或麦克风之前用户获得的权限。 有关详细信息，请参阅隐私指南。

## 安装

    cordova plugin add org.apache.cordova.media-capture
    

## 支持的平台

*   亚马逊火 OS
*   Android 系统
*   黑莓 10
*   iOS
*   Windows Phone 7 和 8
*   Windows 8

## 对象

*   捕获
*   CaptureAudioOptions
*   CaptureImageOptions
*   CaptureVideoOptions
*   CaptureCallback
*   CaptureErrorCB
*   配置
*   媒体
*   MediaFileData

## 方法

*   capture.captureAudio
*   capture.captureImage
*   capture.captureVideo
*   MediaFile.getFormatData

## 属性

*   **supportedAudioModes**： 音频录音设备所支持的格式。(ConfigurationData[])

*   **supportedImageModes**：设备所支持的录制图像大小和格式。(ConfigurationData[])

*   **supportedVideoModes**：靠设备支持的录制视频分辨率和格式。(ConfigurationData[])

## capture.captureAudio

> 启动照相机的音频录音机应用程序并返回有关采集到的音频剪辑文件的信息。

    navigator.device.capture.captureAudio(
        CaptureCB captureSuccess, CaptureErrorCB captureError,  [CaptureAudioOptions options]
    );
    

### 说明

开始异步操作以捕获使用该设备的默认音频录制应用程序的音频录制。 该操作允许设备用户在单个会话中捕获多个录音。

在用户退出音频录音应用程序，或由指定的录音的最大数目 `CaptureAudioOptions.limit` 到达时捕获操作结束。 如果没有 `limit` 指定参数的值，它将默认为one (1) ，并且当用户记录单个音频剪辑之后捕获操作终止。

在捕获操作完成后， `CaptureCallback`执行与描述每个捕获音频剪辑文件 `MediaFile` 对象的数组。 在捕获音频剪辑之前，如果用户终止操作，`CaptureErrorCallback` 执行 `CaptureError` 对象，`CaptureError.CAPTURE_NO_MEDIA_FILES` 错误代码。

### 支持的平台

*   亚马逊火 OS
*   Android 系统
*   黑莓 10
*   iOS
*   Windows Phone 7 和 8
*   Windows 8

### 示例

    // capture callback
    var captureSuccess = function(mediaFiles) {
        var i, path, len;
        for (i = 0, len = mediaFiles.length; i < len; i += 1) {
            path = mediaFiles[i].fullPath;
            // do something interesting with the file
        }
    };
    
    // capture error callback
    var captureError = function(error) {
        navigator.notification.alert('Error code: ' + error.code, null, 'Capture Error');
    };
    
    // start audio capture
    navigator.device.capture.captureAudio(captureSuccess, captureError, {limit:2});
    

### iOS 的怪癖

*   iOS 没有默认的音频录音应用程序，因此提供了一个简单的用户界面。

### Windows Phone 7 和 8 怪癖

*   Windows Phone 7 没有默认的音频录音应用程序，因此提供了一个简单的用户界面。

## CaptureAudioOptions

> 该类封装了音频采集功能的配置选项。

### 属性

*   **限制**： 音频剪辑设备用户可以在单个捕获操作中记录的最大数目。值必须是大于或等于 1 （默认为 1）。

*   **持续时间**： 音频的声音剪辑，以秒为单位的最长期限。

### 示例

    // limit capture operation to 3 media files, no longer than 10 seconds each
    var options = { limit: 3, duration: 10 };
    
    navigator.device.capture.captureAudio(captureSuccess, captureError, options);
    

### 亚马逊火 OS 怪癖

*   `duration`参数不受支持。记录长度不能仅限于以编程方式。

### Android 的怪癖

*   `duration`参数不受支持。记录长度不能仅限于以编程方式。

### 黑莓 10 怪癖

*   `duration`参数不受支持。记录长度不能局限以编程方式。
*   `limit`参数不受支持，所以只有一个记录可以创建的每个调用。

### iOS 的怪癖

*   `limit`参数不受支持，所以只有一个记录可以创建的每个调用。

## capture.captureImage

> 启动摄像头应用程序并返回有关采集到的图像文件的信息。

    navigator.device.capture.captureImage(
        CaptureCB captureSuccess, CaptureErrorCB captureError, [CaptureImageOptions options]
    );
    

### 说明

使用该设备的摄像头应用程序开始一个异步操作以捕获图像。该操作允许用户在一个会话中捕获多个图像。

在捕获操作结束或者当用户关闭摄像头应用程序，或由`CaptureAudioOptions.limit`指定录音的最大数目的 到达。 如果没有 `limit` 指定的值，它将默认为one (1) ，并且当用户采集单个图像后该采集操作终止。

在采集操作完成后，它将调用 `CaptureCB` 与描述每个采集到的图像文件 `MediaFile` 对象的一个数组的回调。 如果用户在采集图像之前终止操作， `CaptureErrorCB`将与 `CaptureError` 对象特色 `CaptureError.CAPTURE_NO_MEDIA_FILES` 的错误代码来执行回调。

### 支持的平台

*   亚马逊火 OS
*   Android 系统
*   黑莓 10
*   iOS
*   Windows Phone 7 和 8
*   Windows 8

### Windows Phone 7 的怪癖

调用本机摄像头应用程序，同时通过 Zune 连接您的设备不工作，并执行错误回调。

### 示例

    // capture callback
    var captureSuccess = function(mediaFiles) {
        var i, path, len;
        for (i = 0, len = mediaFiles.length; i < len; i += 1) {
            path = mediaFiles[i].fullPath;
            // do something interesting with the file
        }
    };
    
    // capture error callback
    var captureError = function(error) {
        navigator.notification.alert('Error code: ' + error.code, null, 'Capture Error');
    };
    
    // start image capture
    navigator.device.capture.captureImage(captureSuccess, captureError, {limit:2});
    

## CaptureImageOptions

> 封装了图像采集功能的配置选项。

### 属性

*   **限制**： 用户可以在单个捕获操作中捕获的图像的最大数目。值必须是大于或等于 1 （默认为 1）。

### 示例

    // limit capture operation to 3 images
    var options = { limit: 3 };
    
    navigator.device.capture.captureImage(captureSuccess, captureError, options);
    

### iOS 的怪癖

*   **限制**参数不受支持，并只有一个图像采取每次调用的。

## capture.captureVideo

> 启动照相机的视频录制器应用程序并返回有关采集到的视频剪辑文件的信息。

    navigator.device.capture.captureVideo(
        CaptureCB captureSuccess, CaptureErrorCB captureError, [CaptureVideoOptions options]
    );
    

### 说明

开始异步操作以捕获使用该设备的视频录制应用程序的视频录制。该操作允许用户在一个会话中捕获多个录音。

在用户退出视频录制应用程序，或由指定的录音的最大数目 `CaptureVideoOptions.limit` 到达时捕获操作结束。 如果没有 `limit` 指定参数的值，它将默认为one (1)，并且当用户记录单个视频剪辑后捕获操作中断。

在捕获操作完成后，该`CaptureCB`的回调将与每个捕获视频剪辑文件所描述的`MediaFile` 对象的数组来执行。 如果用户在采集的视频剪辑之前终止操作，该 `CaptureErrorCB` 与 `CaptureError` 对象特色的一个`CaptureError.CAPTURE_NO_MEDIA_FILES` 错误代码来执行回调。

### 支持的平台

*   亚马逊火 OS
*   Android 系统
*   黑莓 10
*   iOS
*   Windows Phone 7 和 8
*   Windows 8

### 示例

    // capture callback
    var captureSuccess = function(mediaFiles) {
        var i, path, len;
        for (i = 0, len = mediaFiles.length; i < len; i += 1) {
            path = mediaFiles[i].fullPath;
            // do something interesting with the file
        }
    };
    
    // capture error callback
    var captureError = function(error) {
        navigator.notification.alert('Error code: ' + error.code, null, 'Capture Error');
    };
    
    // start video capture
    navigator.device.capture.captureVideo(captureSuccess, captureError, {limit:2});
    

### 黑莓 10 怪癖

*   科尔多瓦的黑莓 10 尝试启动**视频录像机**提供的应用程序，由 RIM，以捕获视频的录制。 这款应用程序会收到 `CaptureError.CAPTURE_NOT_SUPPORTED` 错误代码，如果应用程序未安装在设备上。

## CaptureVideoOptions

> 封装了视频采集功能的配置选项。

### 属性

*   **限制**： 该设备的用户可以在单个捕获操作中捕获的视频剪辑的最大数目。值必须是大于或等于 1 （默认为 1）。

*   **持续时间**： 视频剪辑，以秒为单位的最长期限。

### 示例

    // limit capture operation to 3 video clips
    var options = { limit: 3 };
    
    navigator.device.capture.captureVideo(captureSuccess, captureError, options);
    

### 黑莓 10 怪癖

*   不支持的**持续时间**参数，所以录制的长度不能以编程方式加以限制。

### iOS 的怪癖

*   **限制**参数不受支持。只有一个视频记录每次调用的。

## CaptureCB

> 在成功的媒体捕获操作时调用。

    function captureSuccess( MediaFile[] mediaFiles ) { ... };
    

### 说明

当一个成功的捕获操作完成后执行该函数。 在一个媒体文件已捕获的这一点上，而无论用户已退出媒体捕获应用程序，或捕获已达到限制。

每个 `MediaFile` 对象描述一个捕捉的媒体文件。

### 示例

    // capture callback
    function captureSuccess(mediaFiles) {
        var i, path, len;
        for (i = 0, len = mediaFiles.length; i < len; i += 1) {
            path = mediaFiles[i].fullPath;
            // do something interesting with the file
        }
    };
    

## CaptureError

> 用于描述多媒体采集时产生的错误。

### 属性

*   **代码**： 下面列出的预定义的错误代码之一。

### 常量

*   `CaptureError.CAPTURE_INTERNAL_ERR`： 摄像机或麦克风无法捕获的图像或声音。

*   `CaptureError.CAPTURE_APPLICATION_BUSY`： 相机或音频捕获应用程序正在服另一个捕获请求。

*   `CaptureError.CAPTURE_INVALID_ARGUMENT`： API 的使用无效 （例如，价值 `limit` 小于 1)。

*   `CaptureError.CAPTURE_NO_MEDIA_FILES`： 在用户退出之前捕获任何相机或音频捕获应用程序。

*   `CaptureError.CAPTURE_NOT_SUPPORTED`： 请求的捕获操作不受支持。

## CaptureErrorCB

> 如果在一个媒体采取操作期间发生错误则调用。

    function captureError( CaptureError error ) { ... };
    

### 说明

如果当试图发起一个媒体采集操作时发生错误，则执行此函数。 故障情形包括捕获应用程序正忙、 捕获操作已经发生，或用户在采集任何媒体文件之前取消该操作。

此函数执行一个包含适当的错误`code`的`CaptureError` 对象.

### 示例

    // capture error callback
    var captureError = function(error) {
        navigator.notification.alert('Error code: ' + error.code, null, 'Capture Error');
    };
    

## 配置

> 封装一组支持媒体采集参数的设备。

### 说明

描述靠设备支持的媒体采集模式。配置数据包含 MIME 类型和采集尺寸的视频或图像的采集。

MIME 类型应坚持[RFC2046][1]。例如：

 [1]: http://www.ietf.org/rfc/rfc2046.txt

*   `video/3gpp`
*   `video/quicktime`
*   `image/jpeg`
*   `audio/amr`
*   `audio/wav`

### 属性

*   **类型**： ASCII 编码的小写字符串表示的媒体类型。() DOMString

*   **高度**： 图像或视频以像素为单位的高度。值为零的声音剪辑。（人数）

*   **宽度**： 图像或视频以像素为单位的宽度。值为零的声音剪辑。（人数）

### 示例

    // retrieve supported image modes
    var imageModes = navigator.device.capture.supportedImageModes;
    
    // Select mode that has the highest horizontal resolution
    var width = 0;
    var selectedmode;
    for each (var mode in imageModes) {
        if (mode.width > width) {
            width = mode.width;
            selectedmode = mode;
        }
    }
    

不支持任何平台。所有配置数据数组都是空的。

## MediaFile.getFormatData

> 检索格式媒体捕获文件的信息。

    mediaFile.getFormatData （MediaFileDataSuccessCB successCallback） [MediaFileDataErrorCB errorCallback] ；
    

### 说明

此函数以异步方式尝试检索该媒体文件的格式信息。 如果成功，它将调用 `MediaFileDataSuccessCB` 回调与 `MediaFileData` 对象。 如果该尝试失败，此函数将调用 `MediaFileDataErrorCB` 回调。

### 支持的平台

*   亚马逊火 OS
*   Android 系统
*   黑莓 10
*   iOS
*   Windows Phone 7 和 8
*   Windows 8

### 亚马逊火 OS 怪癖

访问媒体文件格式信息的 API 被限制，所以并不是所有的 `MediaFileData` 属性都被支持。

### 黑莓 10 怪癖

没有关于媒体文件的信息提供了一个API，所以所有的`MediaFileData` 对象返回默认值。

### Android 的怪癖

访问媒体文件格式信息的 API 的限制，所以并不是所有 `MediaFileData` 支持的属性。

### iOS 的怪癖

访问媒体文件格式信息的 API 的限制，所以并不是所有 `MediaFileData` 支持的属性。

## 媒体

> 封装媒体采集文件的属性。

### 属性

*   **名称**： 文件的名称，不包含路径信息。() DOMString

*   **完整路径**： 文件，包括名称的完整路径。() DOMString

*   **类型**： 文件的 mime 类型 (DOMString)

*   **lastModifiedDate**： 日期和文件的上次修改时间。（日期）

*   **大小**： 文件的大小，以字节为单位。（人数）

### 方法

*   **MediaFile.getFormatData**: 检索该媒体文件的格式信息。

## MediaFileData

> 封装了多媒体文件的格式信息。

### 属性

*   **编解码器**： 实际的音频和视频内容的格式。() DOMString

*   **比特率**： 内容的平均比特率。值为零的图像。（人数）

*   **高度**： 图像或视频以像素为单位的高度。值为零的音频剪辑。（人数）

*   **宽度**： 图像或视频以像素为单位的宽度。值为零的音频剪辑。（人数）

*   **持续时间**： 以秒为单位的视频或声音剪辑的长度。值为零的图像。（人数）

### 黑莓 10 怪癖

没有 API 媒体文件提供的格式信息，所以通过`MediaFile.getFormatData` 功能 返回的`MediaFileData` 对象具有以下默认值：

*   **编解码器**： 不受支持，并且返回`null`.

*   **比特率**: 不受支持，并且返回零。

*   **高度**: 不受支持，并且返回零。

*   **宽度**: 不受支持，并且返回零。

*   **持续时间**： 不受支持，并且返回零。

### 亚马逊火 OS 怪癖

支持以下 `MediaFileData` 属性：

*   **codecs**： 不支持，并且返回`null`.

*   **bitrate**: 不支持，并且返回零。

*   **高度**： 支持： 仅图像和视频文件。

*   **宽度**： 支持： 仅图像和视频文件。

*   **持续时间**： 支持： 仅音频和视频文件

### Android 的怪癖

支持以下 `MediaFileData` 属性：

*   **编解码器**： 不受支持，并且返回`null`.

*   **比特率**: 不受支持，并且返回零。

*   **height**： 支持： 仅图像和视频文件。

*   **width**： 支持： 仅图像和视频文件。

*   **持续时间**： 支持： 仅音频和视频文件。

### iOS 的怪癖

支持以下 `MediaFileData` 属性：

*   **编解码器**： 不受支持，并且返回`null`.

*   **比特率**： 仅音频 iOS4 设备上受支持。对于图像和视频，返回零。

*   **高度**： 支持： 仅图像和视频文件。

*   **宽度**： 支持： 仅图像和视频文件。

*   **duration**： 支持： 仅音频和视频文件。