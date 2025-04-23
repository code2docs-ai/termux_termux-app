# 基础信息

|      |      |
|------|------|
| 名称 | TerminalToolbarViewPager |
| 编码语言 | .java |
| 代码路径 | termux-app/app/src/main/java/com/termux/app/terminal/io/TerminalToolbarViewPager.java |
| 包名 | com.termux.app.terminal.io |
| 依赖项 | ['android.view.LayoutInflater', 'android.view.View', 'android.view.ViewGroup', 'android.widget.EditText', 'androidx.annotation.NonNull', 'androidx.viewpager.widget.PagerAdapter', 'androidx.viewpager.widget.ViewPager', 'com.termux.R', 'com.termux.app.TermuxActivity', 'com.termux.shared.termux.extrakeys.ExtraKeysView', 'com.termux.terminal.TerminalSession'] |
| 概述说明 | 终端工具栏分页适配器，管理额外按键和文本输入视图切换。 |

# 说明

TerminalToolbarViewPager包含PageAdapter和OnPageChangeListener两个内部类。PageAdapter继承自PagerAdapter，管理两个页面：第一个页面加载额外按键视图并应用全屏修复，第二个页面提供文本输入框，支持发送文本到当前会话。OnPageChangeListener处理页面切换时的焦点变化，切换到按键页时聚焦终端视图，切换到输入页时聚焦文本输入框。适配器还保存并恢复输入文本状态。

# 类列表 Class Summary

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| TerminalToolbarViewPager | class | 终端工具栏分页适配器，处理按键和文本输入视图切换及交互逻辑。 |



## 类 TerminalToolbarViewPager

|      |      |
|------|------|
| 访问范围 | public |
| 类型 | class |
| 名称 | TerminalToolbarViewPager |
| 说明 | 终端工具栏分页适配器，处理按键和文本输入视图切换及交互逻辑。 |


### UML类图

```mermaid
classDiagram
    class TerminalToolbarViewPager {
        <<static>> 
    }

    class PageAdapter {
        -TermuxActivity mActivity
        -String mSavedTextInput
        +PageAdapter(TermuxActivity activity, String savedTextInput)
        +int getCount()
        +boolean isViewFromObject(View view, Object object)
        +Object instantiateItem(ViewGroup collection, int position)
        +void destroyItem(ViewGroup collection, int position, Object view)
    }
    TerminalToolbarViewPager *-- PageAdapter : 包含

    class OnPageChangeListener {
        -TermuxActivity mActivity
        -ViewPager mTerminalToolbarViewPager
        +OnPageChangeListener(TermuxActivity activity, ViewPager viewPager)
        +void onPageSelected(int position)
    }
    TerminalToolbarViewPager *-- OnPageChangeListener : 包含

    class TermuxActivity {
        <<Interface>>
        +TerminalSession getCurrentSession()
        +TerminalView getTerminalView()
        +Properties getProperties()
        +ExtraKeysViewClient getTermuxTerminalExtraKeys()
        +void setExtraKeysView(ExtraKeysView extraKeysView)
        +TerminalSessionClient getTermuxTerminalSessionClient()
    }
    PageAdapter --> TermuxActivity : 依赖
    OnPageChangeListener --> TermuxActivity : 依赖

    class ViewPager {
        <<Framework>>
    }
    OnPageChangeListener --> ViewPager : 依赖

    class ExtraKeysView {
        +void setExtraKeysViewClient(ExtraKeysViewClient client)
        +void setButtonTextAllCaps(boolean allCaps)
        +void reload(ExtraKeysInfo extraKeysInfo, int defaultHeight)
    }
    PageAdapter --> ExtraKeysView : 创建

    class EditText {
        <<Framework>>
        +void setText(CharSequence text)
        +void setOnEditorActionListener(OnEditorActionListener listener)
    }
    PageAdapter --> EditText : 创建

    class TerminalSession {
        <<Interface>>
        +boolean isRunning()
        +void write(String data)
    }
    PageAdapter --> TerminalSession : 依赖

    class FullScreenWorkAround {
        <<Utility>>
        +static void apply(Context context)
    }
    PageAdapter ..> FullScreenWorkAround : 调用
```

该类图展示了TerminalToolbarViewPager及其两个嵌套类PageAdapter和OnPageChangeListener的结构关系。PageAdapter继承自PagerAdapter，负责管理两个页面视图（ExtraKeysView和文本输入框）的创建与销毁，并与TermuxActivity交互获取会话和配置信息。OnPageChangeListener处理页面切换时的焦点变化逻辑，依赖ViewPager和TermuxActivity。整体设计实现了终端工具栏的视图分页功能，涉及Android框架组件和自定义视图的协同工作。


### 内部方法调用关系图

```mermaid
graph TD
    A["类TerminalToolbarViewPager"]
    B["内部类PageAdapter"]
    C["内部类OnPageChangeListener"]
    D["属性: TermuxActivity mActivity"]
    E["属性: String mSavedTextInput"]
    F["构造方法: PageAdapter(TermuxActivity, String)"]
    G["方法: int getCount()"]
    H["方法: boolean isViewFromObject(View, Object)"]
    I["方法: Object instantiateItem(ViewGroup, int)"]
    J["方法: void destroyItem(ViewGroup, int, Object)"]
    K["构造方法: OnPageChangeListener(TermuxActivity, ViewPager)"]
    L["方法: void onPageSelected(int)"]
    M["流程: 实例化布局(position=0)"]
    N["流程: 实例化布局(position=1)"]
    O["流程: 处理文本输入事件"]

    A --> B
    A --> C
    B --> D
    B --> E
    B --> F
    B --> G
    B --> H
    B --> I
    B --> J
    C --> K
    C --> L
    I --> M
    I --> N
    N --> O
```

```mermaid
sequenceDiagram
    participant A as PageAdapter
    participant B as TermuxActivity
    participant C as ViewGroup
    participant D as ExtraKeysView
    participant E as EditText

    A->>B: 获取TermuxTerminalExtraKeys()
    A->>B: 获取Properties()
    A->>C: addView(layout)
    alt position=0
        A->>D: setExtraKeysViewClient()
        A->>D: setButtonTextAllCaps()
        A->>B: setExtraKeysView()
        A->>D: reload()
        opt 全屏模式检查
            A->>B: isUsingFullScreen()
            A->>B: isUsingFullScreenWorkAround()
            A->>B: FullScreenWorkAround.apply()
        end
    else position=1
        A->>E: setText(mSavedTextInput)
        A->>E: setOnEditorActionListener()
        E->>B: 获取CurrentSession()
        E->>B: write(textToSend)
        E->>B: removeFinishedSession()
        E->>E: setText('')
    end
```

这段代码实现了一个终端工具栏视图分页器，包含两个主要功能模块：PageAdapter处理分页视图的创建和管理，OnPageChangeListener处理页面切换事件。PageAdapter根据position不同加载不同布局（0为额外按键视图，1为文本输入视图），并处理相应的交互逻辑。当position为0时初始化并配置ExtraKeysView，为1时设置EditText的输入监听器，将输入内容发送到当前会话。OnPageChangeListener在页面切换时自动切换焦点到对应控件，确保用户交互的连贯性。

### 字段列表 Field List

| 名称  | 类型  | 说明 |
|-------|-------|------|

### 方法列表 Method List

| 名称  | 类型  | 说明 |
|-------|-------|------|




