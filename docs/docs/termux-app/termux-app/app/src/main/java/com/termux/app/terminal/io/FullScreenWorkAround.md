# 基础信息

|      |      |
|------|------|
| 名称 | FullScreenWorkAround |
| 编码语言 | .java |
| 代码路径 | termux-app/app/src/main/java/com/termux/app/terminal/io/FullScreenWorkAround.java |
| 包名 | com.termux.app.terminal.io |
| 依赖项 | ['android.graphics.Rect', 'android.view.View', 'android.view.ViewGroup', 'com.termux.app.TermuxActivity'] |
| 概述说明 | 安卓全屏适配类，动态调整布局高度以适应软键盘显示。 |

# 说明

该代码描述了一个用于处理全屏模式下键盘显示问题的辅助类FullScreenWorkAround。它通过监听布局变化动态调整内容视图高度，确保键盘弹出时内容区域不会遮挡。类中包含计算可用高度、导航栏高度处理以及根据键盘状态调整布局参数的逻辑。主要方法包括初始化设置、布局变化监听和高度计算功能，适用于TermuxActivity场景。

# 类列表 Class Summary

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| FullScreenWorkAround | class | 安卓全屏适配类，动态调整布局高度以适应软键盘显示。 |



## 类 FullScreenWorkAround

|      |      |
|------|------|
| 访问范围 | public |
| 类型 | class |
| 名称 | FullScreenWorkAround |
| 说明 | 安卓全屏适配类，动态调整布局高度以适应软键盘显示。 |


### UML类图

```mermaid
classDiagram
    class FullScreenWorkAround {
        -View mChildOfContent
        -int mUsableHeightPrevious
        -ViewGroup$LayoutParams mViewGroupLayoutParams
        -int mNavBarHeight
        +static apply(TermuxActivity activity) void
        -FullScreenWorkAround(TermuxActivity activity)
        -possiblyResizeChildOfContent() void
        -getNavBarHeight() int
        -computeUsableHeight() int
    }

    class TermuxActivity {
        +getNavBarHeight() int
    }

    class View {
        <<Interface>>
        +getWindowVisibleDisplayFrame(Rect outRect) void
        +getRootView() View
        +getLayoutParams() ViewGroup$LayoutParams
        +requestLayout() void
        +getChildAt(int index) View
    }

    class ViewGroup {
        <<Interface>>
    }

    class Rect {
        +int top
        +int bottom
    }

    FullScreenWorkAround --> TermuxActivity : 依赖
    FullScreenWorkAround --> View : 依赖
    FullScreenWorkAround --> Rect : 依赖
    ViewGroup <|-- View
    View <|-- ViewGroup
```

这段代码描述了一个用于处理全屏模式下软键盘显示问题的工具类FullScreenWorkAround。该类通过监听视图布局变化，动态调整内容区域高度来适应软键盘的显示/隐藏状态。主要功能包括：获取导航栏高度、计算可用显示区域、根据键盘状态调整布局参数等。类图中展示了与TermuxActivity、View/ViewGroup和Rect等Android核心类的依赖关系，体现了该工具类在Android视图系统中的协调作用。


### 内部方法调用关系图

```mermaid
graph TD
    A["类FullScreenWorkAround"]
    B["属性: View mChildOfContent"]
    C["属性: int mUsableHeightPrevious"]
    D["属性: LayoutParams mViewGroupLayoutParams"]
    E["属性: int mNavBarHeight"]
    F["公开方法: apply(TermuxActivity)"]
    G["私有构造方法: FullScreenWorkAround(TermuxActivity)"]
    H["私有方法: possiblyResizeChildOfContent()"]
    I["私有方法: getNavBarHeight()"]
    J["私有方法: computeUsableHeight()"]
    K["初始化: activity.findViewById"]
    L["获取子视图: content.getChildAt(0)"]
    M["获取布局参数: getLayoutParams()"]
    N["注册布局监听: addOnGlobalLayoutListener"]
    O["计算可用高度: Rect计算逻辑"]
    P["高度比较逻辑"]
    Q["调整布局参数: height赋值"]
    R["请求布局更新: requestLayout()"]

    A --> B
    A --> C
    A --> D
    A --> E
    A --> F
    A --> G
    G --> K
    G --> L
    G --> M
    G --> N
    A --> H
    H --> O
    H --> P
    P -->|键盘可见| Q
    P -->|键盘隐藏| Q
    H --> R
    A --> I
    A --> J
    J --> O
```

流程图描述：该流程图展示了FullScreenWorkAround类的完整工作流程，从初始化时获取Activity的content视图和布局参数，到注册全局布局监听器。当布局变化时触发possiblyResizeChildOfContent方法，通过计算当前可用高度与键盘状态的比较，动态调整子视图高度参数并请求重新布局。特别处理了导航栏高度和键盘可见区域的精确计算，确保软键盘弹出时输入区域能正确适配屏幕空间。

### 字段列表 Field List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| mUsableHeightPrevious | int | 私有整型变量，记录之前可用高度。 |
| mViewGroupLayoutParams | ViewGroup.LayoutParams | 私有视图组布局参数变量 |
| mNavBarHeight | int | 私有整型变量mNavBarHeight，存储导航栏高度。 |
| mChildOfContent | View | 私有视图成员变量mChildOfContent |

### 方法列表 Method List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| apply | void | TermuxActivity全屏适配方法。 |
| possiblyResizeChildOfContent | void | 调整子视图高度以适应键盘显示或隐藏。 |
| getNavBarHeight | int | 获取导航栏高度的方法，返回私有变量mNavBarHeight。 |
| computeUsableHeight | int | 计算窗口可见区域高度。 |




