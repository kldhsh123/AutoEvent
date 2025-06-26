# 😈 修改
## 您可以轻松学习小游戏的结构并可以创建自己的小游戏
## 或者您可以在示例文件夹中查看源代码 [这里](https://github.com/KoT0XleB/AutoEvent/tree/main/AutoEvent-NWApi/Games/Example)
## 构建完成后，您可以将它们放在AutoEvent/Events文件夹中加载。
```csharp
namespace AutoEvent.Events.Example
{
    public class ExampleEvent : Event, IEventMap, IEventSound
    {

        // 设置事件的信息。
        public override string Name { get; set; } = "Example";
        public override string Description { get; set; } = "基于战斗事件的示例事件。";
        public override string Author { get; set; } = "KoT0XleB";
        // `ev run` 命令的事件名称。
        public override string CommandName { get; set; } = "example";
        
        // 确保将此设置为true。否则您必须通过Exiled或NWApi手动注册您的插件。
        // 将事件添加到AutoEvent.Events以手动注册它。
        public override bool AutoLoad { get; protected set; } = true;
        

        // 如果用户选择，用户可以使用此预设而不是默认配置。
        // 用户还可以制作自己的配置预设。它们存储在事件配置文件夹内的单独预设文件夹中。
        [EventConfigPreset]
        public ExampleConfig BigPeople => Presets.BigPeople();
        
        [EventConfigPreset]
        public ExampleConfig SingleLoadout => Presets.SingleLoadout();
        
        [EventConfig]
        public ExampleConfig Config { get; set; }

        // 只要事件继承IEventMap，就可以继承地图信息。
        // MapInfo.Map是地图的示意图对象。
        public MapInfo MapInfo { get; set; } = new MapInfo()
            { MapName = "Battle", 
                Position = new Vector3(6f, 1030f, -43.5f), 
                MapRotation = new Quaternion(), 
                Scale = new Vector3(1,1,1), 
                // 如果设置为false，您可以通过base.SpawnMap()手动生成地图；
                SpawnAutomatically = true};

        // 只要事件继承IEventSound，就可以继承声音信息。
        public SoundInfo SoundInfo { get; set; } = new SoundInfo()
            { SoundName = "MetalGearSolid.ogg", 
                Volume = 10, 
                Loop = false, 
                // 如果设置为false，您可以通过base.StartAudio()手动启动音频地图；
                StartAutomatically = true
            };
        
        // 在此处定义字段/属性。确保在OnStart()或OnRegisteringEvents()中设置它们
        // 定义此事件或其处理程序类可能使用的属性。
        private EventHandler EventHandler { get; set; }
        private ExampleTranslate Translation { get; set; }
        
        // 定义仅在此事件类内部使用的字段。
        private List<GameObject> _workstations;
        
        // 只有在运行GameMode事件时才会注册事件。
        protected override void RegisterEvents()
        {
            EventHandler = new EventHandler();
            EventManager.RegisterEvents(EventHandler);
            Servers.TeamRespawn += EventHandler.OnTeamRespawn;
            Servers.SpawnRagdoll += EventHandler.OnSpawnRagdoll;
            Servers.PlaceBullet += EventHandler.OnPlaceBullet;
            Servers.PlaceBlood += EventHandler.OnPlaceBlood;
            Players.DropItem += EventHandler.OnDropItem;
            Players.DropAmmo += EventHandler.OnDropAmmo;
        }

        // 当GameMode事件完成时，事件被取消注册。
        protected override void UnregisterEvents()
        {
            EventManager.UnregisterEvents(EventHandler);
            Servers.TeamRespawn -= EventHandler.OnTeamRespawn;
            Servers.SpawnRagdoll -= EventHandler.OnSpawnRagdoll;
            Servers.PlaceBullet -= EventHandler.OnPlaceBullet;
            Servers.PlaceBlood -= EventHandler.OnPlaceBlood;
            Players.DropItem -= EventHandler.OnDropItem;
            Players.DropAmmo -= EventHandler.OnDropAmmo;
            EventHandler = null;
        }

        // 定义您希望在事件启动/运行时发生的事情。
        protected override void OnStart()
        {
            int count = 0;
            foreach (Player player in Player.GetPlayers())
            {
                if (count % 2 == 0)
                {
                    Extensions.SetRole(player, RoleTypeId.NtfSergeant, RoleSpawnFlags.None);
                    player.Position = RandomClass.GetSpawnPosition(MapInfo.Map, true);
                }
                else
                {
                    Extensions.SetRole(player, RoleTypeId.ChaosConscript, RoleSpawnFlags.None);
                    player.Position = RandomClass.GetSpawnPosition(MapInfo.Map, false);
                }

                count++;
                
                // 您可以使用单个装备或装备列表。
                // 列表很好，因为它允许用户为每个装备添加机会。
                // 只会分配一个装备。
                player.GiveLoadout(Config.Loadouts, LoadoutFlags.IgnoreGodMode);
                Timing.CallDelayed(0.1f, () => { player.CurrentItem = player.Items.First(); });
            }

        }

        // 如果您想要开始倒计时，请重写此协程。
        // 协程自动运行。事件逻辑在此完成之前不会运行。
        protected override IEnumerator<float> BroadcastStartCountdown()
        {
            // 倒计时到开始时间。我们为此使用20秒计时器。
            for (int time = 20; time > 0; time--)
            {
                Extensions.Broadcast(Translation.BattleTimeLeft.Replace("{time}", $"{time}"), 5);
                yield return Timing.WaitForSeconds(1f);
            }

            yield break;
        }

        // 这在开始倒计时完成后执行。这对于移除开始障碍、生成玩家等很有用...
        protected override void CountdownFinished()
        {
            // 倒计时结束后，我们需要摧毁墙壁，并添加工作站。
            _workstations = new List<GameObject>();
            foreach (var gameObject in MapInfo.Map.AttachedBlocks)
            {
                switch (gameObject.name)
                {
                    case "Wall": { GameObject.Destroy(gameObject); } break;
                    case "Workstation": { _workstations.Add(gameObject); } break;
                }
            }
        }


        // 建议不要重写此方法，但可以根据需要重写。
        /* protected override IEnumerator<float> RunGameCoroutine()
        {
            // 注意事物被调用的结构。这是一个抽象的while循环。
            // 对于调试，查看调试命令。它对测试事件很有帮助。
            while (!IsRoundDone() || DebugLogger.AntiEnd)
            {
                if (KillLoop)
                {
                    yield break;
                }
                try
                {
                    ProcessFrame();                
                }
                catch (Exception e)
                {
                    DebugLogger.LogDebug($"在Event.ProcessFrame()捕获异常。", LogLevel.Warn, true);
                    DebugLogger.LogDebug($"{e}", LogLevel.Debug);
                }

                EventTime += TimeSpan.FromSeconds(FrameDelayInSeconds);
                yield return Timing.WaitForSeconds(this.FrameDelayInSeconds);
            }
            yield break;
        }*/

        // 使用此方法确定回合是否完成。如果为false，则每秒调用一次ProcessFrame()，或者调用FrameDelayInSeconds设置的任何值。
        protected override bool IsRoundDone()
        {
            // 当任一团队没有更多玩家时回合结束。
            return !EndConditions.TeamHasMoreThanXPlayers(Team.FoundationForces,0) ||
                   !EndConditions.TeamHasMoreThanXPlayers(Team.ChaosInsurgency,0);
        }

        // 每次ProcessFrame()之间的时间间隔
        protected override float FrameDelayInSeconds { get; set; } = 1f;
        // 用于触发事件。
        protected override void ProcessFrame()
        {
            // 当回合未完成时，这将每秒调用一次。您可以通过更改FrameDelayInSeconds使调用持续时间更快/更慢。
            // 当回合仍在进行时，广播当前回合统计信息。
            var text = Translation.BattleCounter;
            text = text.Replace("{FoundationForces}", $"{Player.GetPlayers().Count(r => r.Team == Team.FoundationForces)}");
            text = text.Replace("{ChaosForces}", $"{Player.GetPlayers().Count(r => r.Team == Team.ChaosInsurgency)}");
            text = text.Replace("{time}", $"{EventTime.Minutes:00}:{EventTime.Seconds:00}");

            Extensions.Broadcast(text, 1);
        }

        // 这仅在事件完成时执行。如果事件停止。将调用OnStop。
        protected override void OnFinished()
        {
            // 回合完成后，广播获胜团队（在这种情况下是mtf或chaos）。
            // 如果回合停止，则不会调用此方法。而是使用OnStop广播获胜者，或者因为回合停止而没有人获胜。
            if (Player.GetPlayers().Count(r => r.Team == Team.FoundationForces) == 0)
            {
                Extensions.Broadcast(Translation.BattleCiWin.Replace("{time}", $"{EventTime.Minutes:00}:{EventTime.Seconds:00}"), 3);
            }
            else // if (Player.GetPlayers().Count(r => r.Team == Team.ChaosInsurgency) == 0)
            {
                Extensions.Broadcast(Translation.BattleMtfWin.Replace("{time}", $"{EventTime.Minutes:00}:{EventTime.Seconds:00}"), 10);
            }        
        }

        // 可用于广播事件正在停止。也可用于停止额外的协程。
        protected override void OnStop()
        {
            base.OnStop();
        }

        // 清理地图、取消生成玩家等之前等待多长时间...
        protected override float PostRoundDelay { get; set; } = 10f;

        
        // 总是被调用。地图将自动取消生成，音频将自动停止。
        protected override void OnCleanup()
        {
            // 完成回合或停止回合10秒后，将调用此方法。
            // 如果10秒太长，您可以更改PostRoundDelay使其更快或更短。
            // 我们可以清理我们生成的额外工作站。
            // 地图将为我们清理，以及物品、尸体和声音。
            foreach (var bench in _workstations)
                GameObject.Destroy(bench);
        }


    }
}
```
