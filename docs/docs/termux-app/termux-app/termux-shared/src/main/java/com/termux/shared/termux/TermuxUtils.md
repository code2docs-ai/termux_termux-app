# 基础信息

|      |      |
|------|------|
| 名称 | TermuxUtils |
| 编码语言 | .java |
| 代码路径 | termux-app/termux-shared/src/main/java/com/termux/shared/termux/TermuxUtils.java |
| 包名 | com.termux.shared.termux |
| 依赖项 | ['android.content.ComponentName', 'android.content.Context', 'android.content.Intent', 'android.content.pm.ApplicationInfo', 'android.content.pm.PackageManager', 'android.content.pm.ResolveInfo', 'android.net.Uri', 'androidx.annotation.NonNull', 'androidx.annotation.Nullable', 'com.termux.shared.R', 'com.termux.shared.android.AndroidUtils', 'com.termux.shared.data.DataUtils', 'com.termux.shared.file.FileUtils', 'com.termux.shared.reflection.ReflectionUtils', 'com.termux.shared.shell.command.runner.app.AppShell', 'com.termux.shared.termux.file.TermuxFileUtils', 'com.termux.shared.logger.Logger', 'com.termux.shared.markdown.MarkdownUtils', 'com.termux.shared.shell.command.ExecutionCommand', 'com.termux.shared.errors.Error', 'com.termux.shared.android.PackageUtils', 'com.termux.shared.termux.TermuxConstants.TERMUX_APP', 'com.termux.shared.termux.shell.command.environment.TermuxShellEnvironment', 'org.apache.commons.io.IOUtils', 'java.io.IOException', 'java.io.InputStream', 'java.nio.charset.Charset', 'java.util.List', 'java.util.regex.Pattern'] |
| 概述说明 | Termux工具类，提供应用信息获取、上下文管理、调试日志等功能。 |

# 说明

TermuxUtils 是一个工具类，提供与 Termux 应用及其插件相关的功能。主要功能包括：获取 Termux 及其插件应用的上下文（Context）；检查 Termux 及其插件是否安装并可用；获取应用信息并生成 Markdown 格式字符串；发送广播通知 Termux 已打开；获取 APT 信息、日志和调试信息；识别 APK 发布来源（如 F-Droid、GitHub 等）。此外，还支持通过反射获取 Termux APK 中的字段值，并提供了多种应用信息模式（AppInfoMode）以灵活获取不同范围的应用信息。

# 类列表 Class Summary

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| TermuxUtils | class | Termux工具类，提供应用信息获取、上下文管理及调试功能。 |



## 类 TermuxUtils

|      |      |
|------|------|
| 访问范围 | public |
| 类型 | class |
| 名称 | TermuxUtils |
| 说明 | Termux工具类，提供应用信息获取、上下文管理及调试功能。 |


### UML类图

```mermaid
classDiagram
    class TermuxUtils {
        -String LOG_TAG
        +enum AppInfoMode {
            TERMUX_PACKAGE,
            TERMUX_AND_PLUGIN_PACKAGE,
            TERMUX_AND_PLUGIN_PACKAGES,
            TERMUX_PLUGIN_PACKAGES,
            TERMUX_AND_CALLING_PACKAGE
        }
        +Context getTermuxPackageContext(Context context)
        +Context getTermuxPackageContextWithCode(Context context)
        +Context getTermuxAPIPackageContext(Context context)
        +Context getTermuxBootPackageContext(Context context)
        +Context getTermuxFloatPackageContext(Context context)
        +Context getTermuxStylingPackageContext(Context context)
        +Context getTermuxTaskerPackageContext(Context context)
        +Context getTermuxWidgetPackageContext(Context context)
        +Context getContextForPackageOrExitApp(Context context, String packageName, boolean exitAppOnError)
        +String isTermuxAppInstalled(Context context)
        +String isTermuxAPIAppInstalled(Context context)
        +String isTermuxAppAccessible(Context currentPackageContext)
        +Object getTermuxAppAPKBuildConfigClassField(Context currentPackageContext, String fieldName)
        +Object getTermuxAppAPKClassField(Context currentPackageContext, String clazzName, String fieldName)
        +boolean isUriDataForTermuxOrPluginPackage(Uri data)
        +boolean isUriDataForTermuxPluginPackage(Uri data)
        +void sendTermuxOpenedBroadcast(Context context)
        +String getAppInfoMarkdownString(Context currentPackageContext, AppInfoMode appInfoMode)
        +String getAppInfoMarkdownString(Context currentPackageContext, AppInfoMode appInfoMode, String callingPackageName)
        +String getTermuxPluginAppsInfoMarkdownString(Context currentPackageContext)
        +String getAppInfoMarkdownString(Context currentPackageContext, boolean returnTermuxPackageInfoToo)
        +String getAppInfoMarkdownStringInner(Context context)
        +String getReportIssueMarkdownString(Context context)
        +String getImportantLinksMarkdownString(Context context)
        +String geAPTInfoMarkdownString(Context context)
        +String getTermuxDebugMarkdownString(Context context)
        +String getLogcatDumpMarkdownString(Context context)
        +String getAPKRelease(String signingCertificateSHA256Digest)
        +String getTermuxAppPID(Context context)
    }

    class PackageUtils {
        <<Interface>>
        +Context getContextForPackage(Context context, String packageName)
        +Context getContextForPackageOrExitApp(Context context, String packageName, boolean exitAppOnError, String githubRepoUrl)
        +String isAppInstalled(Context context, String appName, String packageName)
        +String getAppNameForPackage(Context context)
        +String getPackageNameForPackage(Context context)
        +ApplicationInfo getApplicationInfoForPackage(Context context, String packageName)
        +String getPackagePID(Context context, String packageName)
        +String getSigningCertificateSHA256DigestForPackage(Context context)
    }

    class TermuxConstants {
        <<Interface>>
        +String TERMUX_PACKAGE_NAME
        +String TERMUX_API_PACKAGE_NAME
        +String TERMUX_BOOT_PACKAGE_NAME
        +String TERMUX_FLOAT_PACKAGE_NAME
        +String TERMUX_STYLING_PACKAGE_NAME
        +String TERMUX_TASKER_PACKAGE_NAME
        +String TERMUX_WIDGET_PACKAGE_NAME
        +List~String~ TERMUX_PLUGIN_APP_PACKAGE_NAMES_LIST
        +String TERMUX_PREFIX_DIR_PATH
        +String TERMUX_FILES_DIR_PATH
        +String BROADCAST_TERMUX_OPENED
        +String TERMUX_GITHUB_REPO_URL
    }

    class TermuxFileUtils {
        <<Interface>>
        +Error isTermuxPrefixDirectoryAccessible(boolean createIfNotExists, boolean setPermissions)
        +Error isTermuxFilesDirectoryAccessible(Context context, boolean createIfNotExists, boolean setPermissions)
        +String getTermuxFilesStatMarkdownString(Context context)
    }

    class ReflectionUtils {
        <<Interface>>
        +Object invokeField(Class~?~ clazz, String fieldName, Object object)
    }

    class AndroidUtils {
        <<Interface>>
        +String getAppInfoMarkdownString(Context context)
        +String getAppInfoMarkdownString(Context context, String packageName)
        +void appendPropertyToMarkdown(StringBuilder markdownString, String propertyName, String propertyValue)
    }

    class MarkdownUtils {
        <<Interface>>
        +String getLinkMarkdownString(String text, String url)
        +String getMarkdownCodeForString(String string, boolean addNewline)
    }

    class AppShell {
        <<Interface>>
        +AppShell execute(Context context, ExecutionCommand executionCommand, ShellEnvironment shellEnvironment, ShellTermuxSession termuxSession, boolean isSynchronous)
    }

    class ExecutionCommand {
        +String commandLabel
        +String stdout
        +String stderr
        +Integer exitCode
        +boolean isSuccessful()
    }

    TermuxUtils --> PackageUtils : 依赖
    TermuxUtils --> TermuxConstants : 依赖
    TermuxUtils --> TermuxFileUtils : 依赖
    TermuxUtils --> ReflectionUtils : 依赖
    TermuxUtils --> AndroidUtils : 依赖
    TermuxUtils --> MarkdownUtils : 依赖
    TermuxUtils --> AppShell : 依赖
    AppShell --> ExecutionCommand : 依赖
```

这段代码展示了TermuxUtils类的完整结构，它是一个Android工具类，主要用于处理与Termux应用及其插件相关的各种操作。类中包含枚举类型AppInfoMode用于定义不同的应用信息获取模式，以及大量静态方法用于获取不同Termux组件的上下文、检查应用安装状态、生成Markdown格式的应用信息报告等。该类与多个接口类（如PackageUtils、TermuxConstants等）存在依赖关系，共同构成了Termux应用生态系统的核心工具集。


### 内部方法调用关系图

```mermaid
graph TD
    A["类TermuxUtils"]
    B["枚举: AppInfoMode"]
    C["常量: LOG_TAG"]
    D["方法: getTermuxPackageContext"]
    E["方法: getTermuxPackageContextWithCode"]
    F["方法: getTermuxAPIPackageContext"]
    G["方法: getTermuxBootPackageContext"]
    H["方法: getTermuxFloatPackageContext"]
    I["方法: getTermuxStylingPackageContext"]
    J["方法: getTermuxTaskerPackageContext"]
    K["方法: getTermuxWidgetPackageContext"]
    L["方法: getContextForPackageOrExitApp"]
    M["方法: isTermuxAppInstalled"]
    N["方法: isTermuxAPIAppInstalled"]
    O["方法: isTermuxAppAccessible"]
    P["方法: getTermuxAppAPKBuildConfigClassField"]
    Q["方法: getTermuxAppAPKClassField"]
    R["方法: isUriDataForTermuxOrPluginPackage"]
    S["方法: isUriDataForTermuxPluginPackage"]
    T["方法: sendTermuxOpenedBroadcast"]
    U["方法: getAppInfoMarkdownString"]
    V["方法: getTermuxPluginAppsInfoMarkdownString"]
    W["方法: getAppInfoMarkdownStringInner"]
    X["方法: getReportIssueMarkdownString"]
    Y["方法: getImportantLinksMarkdownString"]
    Z["方法: geAPTInfoMarkdownString"]
    AA["方法: getTermuxDebugMarkdownString"]
    AB["方法: getLogcatDumpMarkdownString"]
    AC["方法: getAPKRelease"]
    AD["方法: getTermuxAppPID"]

    A --> B
    A --> C
    A --> D
    A --> E
    A --> F
    A --> G
    A --> H
    A --> I
    A --> J
    A --> K
    A --> L
    A --> M
    A --> N
    A --> O
    A --> P
    A --> Q
    A --> R
    A --> S
    A --> T
    A --> U
    A --> V
    A --> W
    A --> X
    A --> Y
    A --> Z
    A --> AA
    A --> AB
    A --> AC
    A --> AD
```

该流程图展示了TermuxUtils类的完整结构，包含枚举定义、常量和方法调用关系。类主要提供Termux应用相关的工具方法，包括获取不同包名的上下文、检查应用安装状态、生成Markdown格式的应用信息、发送广播等。方法按功能分组，清晰地展现了类对Termux生态系统的全面支持能力，特别是对插件应用和API调用的处理逻辑。

### 字段列表 Field List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| LOG_TAG = "TermuxUtils" | String | Termux工具类的日志标签常量。 |

### 方法列表 Method List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| getReportIssueMarkdownString | String | 生成报告问题的链接，包括邮件、Reddit和GitHub问题提交地址。 |
| isTermuxAPIAppInstalled | String | 检查Termux API应用是否安装。 |
| getTermuxPackageContextWithCode | Context | 获取Termux包上下文，含代码。 |
| getTermuxFloatPackageContext | Context | 获取Termux浮动窗口包上下文的方法。 |
| sendTermuxOpenedBroadcast | void | 发送Termux打开广播，适配Oreo限制，遍历接收器并显式发送。 |
| getTermuxAPIPackageContext | Context | 获取Termux API包上下文的方法，调用PackageUtils工具类实现。 |
| getTermuxAppAPKClassField | Object | 获取Termux应用APK类字段值，异常返回null。 |
| getAppInfoMarkdownString | String | 获取应用信息的静态方法，支持模式和上下文参数。 |
| getTermuxBootPackageContext | Context | 获取Termux Boot包上下文的方法。 |
| getTermuxPackageContext | Context | 获取Termux包上下文的方法，调用PackageUtils工具类实现。 |
| isUriDataForTermuxPluginPackage | boolean | 检查URI是否以Termux插件包前缀开头。 |
| getAppInfoMarkdownStringInner | String | 获取应用信息并生成Markdown字符串，包含包管理、文件访问状态及签名证书摘要。 |
| getTermuxDebugMarkdownString | String | 获取Termux调试信息，合并文件状态和日志数据。 |
| getTermuxAppAPKBuildConfigClassField | Object | 获取Termux应用APK构建配置类的指定字段值。 |
| getTermuxWidgetPackageContext | Context | 获取Termux小部件包上下文的方法。 |
| getAppInfoMarkdownString | String | 获取应用信息并生成Markdown格式字符串，包含当前应用和Termux应用信息。 |
| geAPTInfoMarkdownString | String | 获取APT信息并生成Markdown字符串。 |
| getTermuxTaskerPackageContext | Context | 获取Termux Tasker包上下文的方法。 |
| getTermuxStylingPackageContext | Context | 获取Termux样式包上下文的方法。 |
| getAPKRelease | String | 根据签名证书SHA256摘要返回APK发布渠道，支持F-Droid、GitHub、Google Play和Termux Devs。 |
| isTermuxAppAccessible | String | 检查Termux应用是否可访问，包括安装状态、包上下文和目录权限，返回错误信息或null。 |
| getContextForPackageOrExitApp | Context | 获取指定包名的上下文，失败时可选退出应用。 |
| isUriDataForTermuxOrPluginPackage | boolean | 检查URI是否为Termux或其插件包。 |
| getAppInfoMarkdownString | String | 获取应用信息的静态方法，根据模式返回不同格式的应用详情。 |
| getImportantLinksMarkdownString | String | 生成Termux重要链接的Markdown字符串，包含GitHub、邮件、Reddit和Wiki等。 |
| getTermuxPluginAppsInfoMarkdownString | String | 获取Termux插件应用信息并生成Markdown字符串。 |
| isTermuxAppInstalled | String | 检查Termux应用是否安装 |
| getLogcatDumpMarkdownString | String | 获取logcat日志并生成Markdown格式输出，限制3000行防止内存溢出。 |
| getTermuxAppPID | String | 获取Termux应用进程ID的静态方法。 |




