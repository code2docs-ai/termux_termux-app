# 基础信息

|      |      |
|------|------|
| 名称 | BellHandler |
| 编码语言 | .java |
| 代码路径 | termux-app/termux-shared/src/main/java/com/termux/shared/termux/terminal/io/BellHandler.java |
| 包名 | com.termux.shared.termux.terminal.io |
| 依赖项 | ['android.content.Context', 'android.os.Build', 'android.os.Handler', 'android.os.Looper', 'android.os.SystemClock', 'android.os.VibrationEffect', 'android.os.Vibrator', 'com.termux.shared.logger.Logger'] |
| 概述说明 | 单例振动处理器，控制振动频率和时长。 |

# 说明

这是一个实现单例模式的振动器管理类BellHandler，通过双重检查锁定确保线程安全。主要功能包括：使用Vibrator服务创建短振动效果（50毫秒），支持Android O及以上版本的振动API；通过Handler在主线程调度振动，确保两次振动间隔至少150毫秒；记录上次振动时间防止重复触发，并处理三星设备在Android 8上的异常情况。类提供同步方法doBell()来触发受控的振动序列。

# 类列表 Class Summary

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| BellHandler | class | 单例振动处理器，双检锁实现，控制振动频率和延迟。 |



## 类 BellHandler

|      |      |
|------|------|
| 访问范围 | public |
| 类型 | class |
| 名称 | BellHandler |
| 说明 | 单例振动处理器，双检锁实现，控制振动频率和延迟。 |


### UML类图

```mermaid
classDiagram
    class BellHandler {
        -static BellHandler instance
        -static Object lock
        -static String LOG_TAG
        -static long DURATION
        -static long MIN_PAUSE
        -Handler handler
        -long lastBell
        -Runnable bellRunnable
        +static BellHandler getInstance(Context context)
        -BellHandler(Vibrator vibrator)
        +synchronized void doBell()
        -long now()
    }

    class Context {
        <<Interface>>
    }

    class Vibrator {
        <<Interface>>
        +void vibrate(long milliseconds)
        +void vibrate(VibrationEffect vibe)
    }

    class VibrationEffect {
        <<Interface>>
        +static VibrationEffect createOneShot(long milliseconds, int amplitude)
    }

    class Handler {
        +void postDelayed(Runnable r, long delayMillis)
    }

    class SystemClock {
        +static long uptimeMillis()
    }

    BellHandler --> Context : 依赖
    BellHandler --> Vibrator : 依赖
    BellHandler --> Handler : 组合
    BellHandler --> SystemClock : 调用
    Vibrator --> VibrationEffect : 使用
```

这段代码展示了一个单例模式实现的BellHandler类，主要用于管理设备振动功能。通过双重检查锁定确保线程安全，使用Handler实现延时振动，并处理Android不同版本间的振动API差异。类图清晰地呈现了与系统服务(Context/Vibrator)、时间工具(SystemClock)和线程调度器(Handler)的交互关系，同时体现了对VibrationEffect接口的版本适配逻辑。


### 内部方法调用关系图

```mermaid
graph TD
    A["类BellHandler"]
    B["静态属性: BellHandler instance"]
    C["静态属性: Object lock"]
    D["静态属性: String LOG_TAG"]
    E["静态方法: getInstance(Context context)"]
    F["构造方法: BellHandler(Vibrator vibrator)"]
    G["方法: doBell()"]
    H["方法: now()"]
    I["内部Runnable: bellRunnable"]
    J["属性: Handler handler"]
    K["属性: long lastBell"]
    L["常量: long DURATION"]
    M["常量: long MIN_PAUSE"]

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

    E -->|"instance == null?"| E1["同步块synchronized(lock)"]
    E1 -->|"双重检查instance == null"| E2["创建新实例BellHandler(vibrator)"]
    E -->|"返回instance"| E3["结束"]

    G --> G1["获取当前时间now()"]
    G1 --> G2["计算timeSinceLastBell"]
    G2 -->|"timeSinceLastBell < 0"| G3["忽略操作"]
    G2 -->|"timeSinceLastBell < MIN_PAUSE"| G4["handler.postDelayed(bellRunnable)"]
    G2 -->|"其他情况"| G5["立即执行bellRunnable.run()"]
    G4 --> G6["更新lastBell"]
    G5 --> G6
```

```mermaid
sequenceDiagram
    participant Client
    participant BellHandler
    participant Vibrator
    participant Handler

    Client->>BellHandler: getInstance(context)
    alt 首次调用
        BellHandler->>BellHandler: 同步锁检查
        BellHandler->>Context: getSystemService(VIBRATOR_SERVICE)
        Context-->>BellHandler: Vibrator实例
        BellHandler->>BellHandler: 构造BellHandler(vibrator)
    end
    BellHandler-->>Client: 返回instance

    Client->>BellHandler: doBell()
    BellHandler->>BellHandler: now()
    BellHandler->>BellHandler: 计算timeSinceLastBell
    alt 需要延迟执行
        BellHandler->>Handler: postDelayed(bellRunnable)
        Handler-->>BellHandler: 确认
    else 立即执行
        BellHandler->>bellRunnable: run()
        bellRunnable->>Vibrator: vibrate()
    end
    BellHandler->>BellHandler: 更新lastBell
```

这段代码实现了一个单例模式的振动控制器BellHandler，通过双重检查锁确保线程安全。核心功能doBell()会根据上次振动时间智能调度：时间间隔过短时延迟执行，避免频繁振动；否则立即触发。内部通过Handler实现延迟任务，并针对不同Android版本适配振动API。流程图展示了类结构和主要方法调用关系，时序图则详细描述了获取实例和触发振动的完整交互过程。特别注意对三星设备Android 8的异常处理，体现了良好的健壮性设计。

### 字段列表 Field List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| lock = new Object() | Object | 私有静态锁对象 |
| bellRunnable | Runnable | 私有终态Runnable钟声任务 |
| DURATION = 50 | long | 定义私有静态长整型常量DURATION，值为50。 |
| instance = null | BellHandler | 单例模式中的私有静态实例变量 |
| lastBell = 0 | long | 私有长整型变量lastBell初始化为0。 |
| handler = new Handler(Looper.getMainLooper()) | Handler | 主线程Handler初始化 |
| LOG_TAG = "BellHandler" | String | 私有常量LOG_TAG值为"BellHandler"。 |
| MIN_PAUSE = 3 * DURATION | long | 私有静态长整型常量MIN_PAUSE值为3倍DURATION |

### 方法列表 Method List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| getInstance | BellHandler | 单例模式获取BellHandler实例，双重检查锁确保线程安全。 |
| doBell | void | 同步方法doBell控制铃声触发，根据上次铃声时间决定立即执行或延迟调度。 |
| now | long | 获取系统启动后的毫秒时间。 |




