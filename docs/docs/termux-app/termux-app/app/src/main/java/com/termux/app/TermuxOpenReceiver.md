# 基础信息

|      |      |
|------|------|
| 名称 | TermuxOpenReceiver |
| 编码语言 | .java |
| 代码路径 | termux-app/app/src/main/java/com/termux/app/TermuxOpenReceiver.java |
| 包名 | com.termux.app |
| 依赖项 | ['android.content.ActivityNotFoundException', 'android.content.BroadcastReceiver', 'android.content.ContentValues', 'android.content.Context', 'android.content.Intent', 'android.database.Cursor', 'android.database.MatrixCursor', 'android.net.Uri', 'android.os.Environment', 'android.os.ParcelFileDescriptor', 'android.provider.MediaStore', 'android.webkit.MimeTypeMap', 'com.termux.shared.termux.plugins.TermuxPluginUtils', 'com.termux.shared.data.DataUtils', 'com.termux.shared.data.IntentUtils', 'com.termux.shared.net.uri.UriUtils', 'com.termux.shared.logger.Logger', 'com.termux.shared.net.uri.UriScheme', 'com.termux.shared.termux.TermuxConstants', 'java.io.File', 'java.io.FileNotFoundException', 'java.io.IOException', 'androidx.annotation.NonNull'] |
| 概述说明 | Termux广播接收器处理文件打开和分享请求，支持URI解析、内容类型检测和权限检查。 |

# 说明

TermuxOpenReceiver是一个广播接收器，用于处理文件打开和分享请求。它检查Intent数据，验证URI有效性，并根据不同操作类型（如ACTION_VIEW或ACTION_SEND）创建相应Intent。对于非文件URI，直接启动对应应用；对于本地文件，生成内容URI并设置MIME类型，可选择使用选择器。ContentProvider辅助类提供文件查询功能，包括文件名、大小等元数据，并通过严格路径检查和权限控制确保安全性，防止未授权修改关键配置文件。

# 类列表 Class Summary

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| TermuxOpenReceiver | class | Termux广播接收器处理文件共享和内容提供逻辑。 |



## 类 TermuxOpenReceiver

|      |      |
|------|------|
| 访问范围 | public |
| 类型 | class |
| 名称 | TermuxOpenReceiver |
| 说明 | Termux广播接收器处理文件共享和内容提供逻辑。 |


### UML类图

```mermaid
classDiagram
    class TermuxOpenReceiver {
        -String LOG_TAG
        +onReceive(Context context, Intent intent) void
    }

    class TermuxOpenReceiver.ContentProvider {
        -String LOG_TAG
        +onCreate() boolean
        +query(Uri uri, String[] projection, String selection, String[] selectionArgs, String sortOrder) Cursor
        +getType(Uri uri) String
        +insert(Uri uri, ContentValues values) Uri
        +delete(Uri uri, String selection, String[] selectionArgs) int
        +update(Uri uri, ContentValues values, String selection, String[] selectionArgs) int
        +openFile(Uri uri, String mode) ParcelFileDescriptor
    }

    TermuxOpenReceiver.ContentProvider --|> android.content.ContentProvider
    TermuxOpenReceiver.ContentProvider --> TermuxOpenReceiver : 嵌套类

    class Logger {
        <<static>>
        +logError(String tag, String message) void
        +logVerbose(String tag, String message) void
        +logDebug(String tag, String message) void
    }

    class UriUtils {
        <<static>>
        +getUriFilePathWithFragment(Uri uri) String
        +getContentUri(String authority, String path) Uri
    }

    class DataUtils {
        <<static>>
        +isNullOrEmpty(String str) boolean
    }

    class TermuxPluginUtils {
        <<static>>
        +checkIfAllowExternalAppsPolicyIsViolated(Context context, String logTag) String
    }

    TermuxOpenReceiver --> Logger : 调用日志方法
    TermuxOpenReceiver --> UriUtils : 处理URI路径
    TermuxOpenReceiver --> DataUtils : 检查空路径
    TermuxOpenReceiver.ContentProvider --> TermuxPluginUtils : 安全策略检查
```

类图描述：该图展示了一个Android广播接收器TermuxOpenReceiver及其嵌套内容提供者ContentProvider的结构。接收器主要处理URI数据打开请求，通过Logger记录日志，使用UriUtils处理路径转换，依赖DataUtils进行空值检查。内容提供者继承自Android基础类，实现文件查询和打开功能，通过TermuxPluginUtils进行安全策略验证。整体设计体现了模块化分工，核心类通过工具类辅助完成特定功能。


### 内部方法调用关系图

```mermaid
graph TD
    A["TermuxOpenReceiver.onReceive"]
    B["检查intent.data"]
    C["记录intent详情"]
    D["获取contentTypeExtra和useChooser"]
    E["验证intentAction"]
    F["处理非file scheme"]
    G["获取文件路径"]
    H["验证文件可读性"]
    I["确定contentType"]
    J["构建uriToShare"]
    K["设置sendIntent"]
    L["处理useChooser"]
    M["启动activity"]
    N["错误处理"]

    A --> B
    B -->|data为null| N
    B -->|data非null| C
    C --> D
    D --> E
    E -->|无效action| N
    E -->|有效action| F
    F -->|非file scheme| M
    F -->|file scheme| G
    G -->|路径无效| N
    G -->|路径有效| H
    H -->|文件不可读| N
    H -->|文件可读| I
    I --> J
    J --> K
    K --> L
    L --> M
    M -->|失败| N
```

```mermaid
sequenceDiagram
    participant BroadcastReceiver
    participant Logger
    participant IntentUtils
    participant UriUtils
    participant DataUtils
    participant MimeTypeMap
    participant Context

    BroadcastReceiver->>Logger: logError/logVerbose
    BroadcastReceiver->>IntentUtils: getIntentString
    BroadcastReceiver->>UriUtils: getUriFilePathWithFragment
    BroadcastReceiver->>DataUtils: isNullOrEmpty
    BroadcastReceiver->>MimeTypeMap: getMimeTypeFromExtension
    BroadcastReceiver->>Context: startActivity
    Note right of BroadcastReceiver: 处理文件URI和内容类型
    Note right of BroadcastReceiver: 构建并发送intent
```

该流程图描述了TermuxOpenReceiver处理广播intent的完整流程，从接收intent开始，经过数据验证、路径处理、内容类型确定，到最后启动相应activity或处理错误。时序图展示了主要组件间的交互关系，重点突出了日志记录、工具类调用和上下文操作等关键步骤。整个过程严格检查数据有效性，确保文件可访问性，并正确处理各种内容类型和URI方案。

### 字段列表 Field List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| LOG_TAG = "TermuxOpenReceiver" | String | 私有静态终态字符串LOG_TAG值为TermuxOpenReceiver |

### 方法列表 Method List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| onReceive | void | 处理Intent数据，验证URI和文件路径，根据类型启动对应应用或分享文件。 |




