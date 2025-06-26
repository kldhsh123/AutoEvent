# 示例 :bulb:
### 如果您想学习如何编写自己的小游戏，那么您应该研究小游戏的结构。
### 基本结构：

使用 ***Event*** 接口继承小游戏工作所需的所有重要方法和变量。
```csharp
public class Plugin : Event
{
}
```   
##### 还要继承 ***IEventMap*** 来启动示意图地图和 ***IEventSound*** 来启动音乐（请参阅下面的详细描述）。

#### 事件信息：
用户可以看到的关于事件的信息。
```csharp
public override string Name { get; set; } = "Example";
public override string Description { get; set; } = "基于战斗事件的示例事件。";
public override string Author { get; set; } = "KoT0XleB";
public override string CommandName { get; set; } = "example";
public override Version Version { get; set; } = new Version(1, 0, 0);
```        

#### 事件配置：
您可以访问的设置，这些设置将改变事件的功能。
```csharp
// 在此处输入您的配置，该配置位于您的小游戏文件夹中
[EventConfig]
public Config Config { get; set; }

// 自定义翻译尚未制作。
public Translate Translate { get; set; } = AutoEvent.Singleton.Translation.Translate;
```

#### 事件设置：
您可以访问的设置，这些设置将改变事件的功能。
```csharp
// 回合结束后清理开始前等待多长时间。默认为10秒。
public override float PostRoundDelay { get; protected set; } = 10f; 

// 如果使用NwApi或Exiled作为基础插件，请将其设置为false，并手动将您的插件添加到Event.Events（List[Events]）。
// 这可以防止双重加载您的插件程序集。
public override bool AutoLoad { get; protected set; } = true;

// 用于安全地终止while循环，而无需强制终止协程。
public override bool KillLoop { get; protected set; } = false;

// 事件在每次ProcessFrame()后等待多少秒。
protected override float FrameDelayInSeconds { get; set; } = 1f;
```


#### 事件变量：
您可以使用的变量，这些变量由框架自动管理。
```csharp
// 调用ProcessFrame()的主事件线程的协程句柄。
protected override CoroutineHandle GameCoroutine { get; set; }

// 插件启动时的DateTime（UTC）。
public override DateTime StartTime { get; protected set; }
        
// 自插件启动以来经过的时间。（默认）
public override TimeSpan EventTime { get; protected set; }
```

#### 事件API方法
这里提供了所有必要的工作方法。随意组合它们。
```csharp
// 用于为插件注册事件。
protected override void RegisterEvents() { }

// 用于为插件取消注册事件。
protected override void UnregisterEvents() { }

// 事件启动时调用。
protected override void OnStart();

// 在协程中启动后调用。可以用作倒计时协程。
protected override IEnumerator<float> BroadcastStartCountdown()

// BroadcastStartCountdown完成后调用。可用于移除墙壁或给玩家物品。
protected override void CountdownFinished()

// 用于确定事件是否应该结束。
// 如果回合结束则返回true。如果回合应该继续运行则返回false。
protected abstract bool IsRoundDone();

// 它按照FrameDelayInSeconds中设置的每秒调用次数。
protected override void ProcessFrame() { }

// 事件完成时调用。如果事件通过OnStop停止，则不会调用此方法，因为事件从未真正正确完成。
protected abstract void OnFinished();

// 如果事件被强制停止则调用。如果调用此方法，则不会调用OnFinished。
protected override void OnStop() { }

// 事件完成/停止后清理发生时的可重写类。
protected virtual void OnCleanup() { }
```

#### 事件地图和音乐
使用IEventMap和IEventSound继承重要变量。
```csharp
public class Plugin : Event, IEventSound, IEventMap
{
    public MapInfo MapInfo { get; set; } = new MapInfo()
    { 
        MapName = "SchematicName", // 输入您的示意图名称
        Position = new Vector3(0, 0, 0), // 您的示意图的位置
        IsStatic = true // 始终使用true来优化tps
    };
    public SoundInfo SoundInfo { get; set; } = new SoundInfo()
    { 
        SoundName = "MusicName.ogg",  // 输入您的音乐名称
        Volume = 10, // 建议为音乐设置5到10的值。
        Loop = true // 它使音乐无限循环
    };
}
```

#### 加载器自定义小游戏
##### 如果您使用自定义小游戏加载器，创建小游戏并不那么困难：
###### 在Visual Studio中使用类库.NetFramework 4.8启动项目
###### 将编译的dll小游戏粘贴到文件夹中：
###### Exiled -> ``EXILED\Configs\AutoEvent\Events`` 
###### NWApi -> ``PluginAPI\plugins\global\AutoEvent\Events``
##### 不要忘记在配置中设置 ``IsDebug = true`` 并检查您的小游戏的启动。
