# 基础信息

|      |      |
|------|------|
| 名称 | TextInputDialogUtils |
| 编码语言 | .java |
| 代码路径 | termux-app/termux-shared/src/main/java/com/termux/shared/termux/interact/TextInputDialogUtils.java |
| 包名 | com.termux.shared.termux.interact |
| 依赖项 | ['android.app.Activity', 'android.app.AlertDialog', 'android.content.DialogInterface', 'android.text.Selection', 'android.util.TypedValue', 'android.view.KeyEvent', 'android.view.ViewGroup.LayoutParams', 'android.widget.EditText', 'android.widget.LinearLayout'] |
| 概述说明 | 文本输入对话框工具类，支持设置初始文本和按钮回调。 |

# 说明

TextInputDialogUtils是一个工具类，用于创建文本输入对话框。它包含一个TextSetListener接口用于回调文本设置事件。textInput方法接收Activity、标题文本、初始文本、按钮文本及对应监听器参数。该方法构建一个包含EditText的对话框，设置单行输入、初始文本和光标位置。对话框布局采用Material Design规范的内边距。支持设置正、中、负三个按钮的点击事件，其中正按钮绑定键盘回车事件。对话框不可通过外部触摸取消，创建后立即显示。

# 类列表 Class Summary

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| TextInputDialogUtils | class | 工具类提供文本输入对话框功能，支持设置初始文本和按钮回调。 |



## 类 TextInputDialogUtils

|      |      |
|------|------|
| 访问范围 | public final |
| 类型 | class |
| 名称 | TextInputDialogUtils |
| 说明 | 工具类提供文本输入对话框功能，支持设置初始文本和按钮回调。 |


### UML类图

```mermaid
classDiagram
    class TextInputDialogUtils {
        <<final>>
        +textInput(Activity activity, int titleText, String initialText, int positiveButtonText, TextSetListener onPositive, int neutralButtonText, TextSetListener onNeutral, int negativeButtonText, TextSetListener onNegative, OnDismissListener onDismiss) void
    }

    class TextSetListener {
        <<Interface>>
        +onTextSet(String text) void
    }

    TextInputDialogUtils --> TextSetListener : 依赖
    TextInputDialogUtils --> AlertDialog.Builder : 创建
    TextInputDialogUtils --> EditText : 创建
    TextInputDialogUtils --> LinearLayout : 创建
```

```mermaid
flowchart TD
    A["开始textInput()"] --> B["创建EditText和设置初始文本"]
    B --> C["配置输入框的IME动作和监听器"]
    C --> D["计算布局padding值"]
    D --> E["创建LinearLayout并添加EditText"]
    E --> F["构建AlertDialog.Builder"]
    F --> G{"onNeutral不为空?"}
    G -- 是 --> H["设置Neutral按钮"]
    G -- 否 --> I{"onNegative为空?"}
    I -- 是 --> J["设置默认Cancel按钮"]
    I -- 否 --> K["设置Negative按钮"]
    H --> L
    J --> L
    K --> L
    L{"onDismiss不为空?"}
    L -- 是 --> M["设置OnDismiss监听器"]
    L -- 否 --> N["创建并显示对话框"]
    M --> N
    N --> O["结束"]
```

```mermaid
sequenceDiagram
    participant A as Activity
    participant T as TextInputDialogUtils
    participant E as EditText
    participant L as LinearLayout
    participant B as AlertDialog.Builder
    participant D as AlertDialog

    A->>T: 调用textInput()
    T->>E: 创建并配置EditText
    T->>L: 创建并配置LinearLayout
    T->>B: 创建Builder并设置参数
    B->>D: create()
    T->>D: show()
    D-->>A: 显示对话框
    opt 用户输入
        A->>E: 输入文本
        E->>T: 触发监听器
    end
    opt 按钮点击
        A->>D: 点击按钮
        D->>T: 回调对应监听器
    end
    D->>A: 对话框关闭
```

这段代码实现了一个文本输入对话框工具类，主要包含一个静态方法textInput()用于创建和显示自定义文本输入对话框。类图展示了工具类与接口、Android组件的关系；流程图详细描述了对话框构建过程的条件分支；时序图则说明了从Activity调用到对话框显示和交互的完整过程。该工具支持设置初始文本、三种按钮及其回调，以及对话框关闭监听，通过Builder模式灵活构建对话框界面。


### 内部方法调用关系图

```mermaid
graph TD
    A["TextInputDialogUtils工具类"]
    B["接口: TextSetListener"]
    C["方法: textInput"]
    D["创建EditText"]
    E["设置初始文本"]
    F["设置键盘动作监听"]
    G["计算布局边距"]
    H["创建LinearLayout"]
    I["构建AlertDialog"]
    J["设置按钮监听"]
    K["显示对话框"]

    A --> B
    A --> C
    C --> D
    D --> E
    D --> F
    C --> G
    C --> H
    C --> I
    I --> J
    I --> K
```

```mermaid
sequenceDiagram
    participant A as 调用者
    participant B as TextInputDialogUtils
    participant C as EditText
    participant D as AlertDialog

    A->>B: 调用textInput方法
    B->>C: 创建EditText实例
    B->>C: 设置初始文本/监听器
    B->>D: 创建AlertDialog.Builder
    B->>D: 配置标题/布局/按钮
    B->>D: 设置dismiss监听
    B->>D: 创建并显示对话框
    D-->>A: 用户操作触发回调
```

这段代码实现了一个Android文本输入对话框工具类，主要功能是通过链式调用构建一个可定制的文本输入对话框。流程图展示了从创建输入框到最终显示对话框的完整过程，时序图则详细描述了调用者与工具类、UI组件之间的交互顺序。代码支持设置初始文本、三种按钮(positive/neutral/negative)的回调以及对话框关闭监听，并正确处理了键盘输入事件和布局边距计算等细节。

### 字段列表 Field List

| 名称  | 类型  | 说明 |
|-------|-------|------|

### 方法列表 Method List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| textInput | void | 创建带输入框的对话框，支持确认、中立、取消按钮及文本设置回调。 |




