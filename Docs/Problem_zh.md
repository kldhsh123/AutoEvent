# 问题解决 :trollface: - *如何解决我的问题？*
## 1) 您没有使用此命令的权限：
![image](https://github.com/user-attachments/assets/b96bbf64-e981-4f9a-8200-eb1aab1b8014)
- 在此处阅读如何为您的角色设置权限：[(点击我)](https://github.com/kldhsh123/AutoEvent/blob/main/Docs/Installation_zh.md)
---
## 2) 我只有一个小游戏。我应该怎么办？
![image](https://github.com/user-attachments/assets/c40ac4d8-7753-4627-bf39-d514d53c3b98)
- 您没有安装 MapEditorReborn，所以您只有不需要地图的小游戏。
- 安装 MapEditorReborn 插件，以便插件可以加载地图：[(Github MapEditorReborn)](https://github.com/Michal78900/MapEditorReborn/releases/latest)

![image](https://github.com/user-attachments/assets/0ed636a3-9d08-4034-bc28-150a6646186b)

- 我不知道如何安装 MapEditorReborn。我应该怎么办？

![image](https://github.com/user-attachments/assets/6f292d36-b87c-4ab6-aa49-899e4480ea2b)

---
## 3) 未检测到 MapEditorReborn。在您安装 MapEditorReborn 之前，AutoEvent 将不会加载。
- 这与第 2 点中的问题相同。
- 安装 MapEditorReborn，以便插件可以加载地图：[(Github MapEditorReborn)](https://github.com/Michal78900/MapEditorReborn/releases/latest)

![image](https://github.com/user-attachments/assets/b0355d75-31bc-43b8-980d-11d39e8bcc1c)
---
## 4) 您安装了旧版本的 'MapEditorReborn'，无法运行此小游戏
![image](https://github.com/user-attachments/assets/e66573f4-1899-43a7-9724-01d3c9cd97ec)
- 它说的就是字面意思。您有旧版本的 MapEditorReborn。有新功能或新版本的 SCP 已发布。因此，出现错误。
- 安装新版本的 MapEditorReborn，以便插件可以加载示意图：[(Github MapEditorReborn)](https://github.com/Michal78900/MapEditorReborn/releases/latest)

![image](https://github.com/user-attachments/assets/b0355d75-31bc-43b8-980d-11d39e8bcc1c)
- 如果问题仍然存在，请从 [(Discord MER)](https://discord.gg/JwAfeSd79u) 下载最新版本（或测试版本）：
![image](https://github.com/user-attachments/assets/224dbb89-4974-4e9c-bc8b-6df4149dda9f)

---
## 5) 您需要下载地图（某个地图）来运行此小游戏。
![image](https://github.com/user-attachments/assets/1a71fb4f-08b3-4411-a693-25ac9aae26f6)
- 它说的就是字面意思。此地图在您的服务器上不存在，因此无法运行小游戏。
- 您需要从[最新版本](https://github.com/kldhsh123/AutoEvent/releases/latest)下载 *``Schematics.tar.gz``*。

![image](https://github.com/user-attachments/assets/469eab25-2f94-4414-87dc-7402a5068aaf)
- 将 *``Schematics.tar.gz``* 解压到 ``EXILED/Configs/AutoEvent/Schematics`` 文件夹。

![image](https://github.com/user-attachments/assets/1797ee0b-ed3d-42a5-9fea-546bdf8bca12)

![image](https://github.com/user-attachments/assets/02185f33-dbee-4b56-ae6d-73b7910cd0ef)
---
## 6) 如果您启动小游戏并出现在地图上，但出现问题。

![image](https://github.com/user-attachments/assets/934b43a1-8802-48be-9c95-b84fe25103b9)
- 如果之前的错误是指您没有正确安装某些东西，所有责任都在您作为插件用户，现在这个错误是指我作为插件开发者。
- 写一个 issue 详细说明问题：
![image](https://github.com/user-attachments/assets/2a47ffca-c06e-42d1-9516-71d7018abfbd)
- 我会在找到空闲时间后尽快修复问题。
---
## 7) 找不到小游戏（某个游戏）：

![image](https://github.com/user-attachments/assets/7c828cec-1c5c-4f50-a4d1-9e22ebd961e7)

- 在控制台中输入 ev list 命令：

![image](https://github.com/user-attachments/assets/a25398ca-15d1-452f-b555-7a4ad5522db1)
- 在方括号中找到命令名称：

![image](https://github.com/user-attachments/assets/432b6513-ca13-496c-858a-95a7b2b90866)
- 在 ev run 中输入此命令：

![image](https://github.com/user-attachments/assets/fff98a27-b4ac-47e4-8610-a05c3f0f40a6)
----
## 启用调试模式 (debug-output.log)：
Autoevent 有 2 种记录调试输出的方法。默认情况下，两种记录模式都未开启。可以通过各自的配置选项启用它们。
- 方法 1 - 控制台记录 (`debug`)：
   - 控制台记录将所有错误直接记录到控制台。
- 方法 2 - 调试文件记录 (`auto_log_debug`)：
   - 调试文件记录将所有错误存储到基础 autoevent 目录中的调试文件。
     - 对于 NWApi：`~/.config/SCP Secret Laboratory/PluginAPI/plugins/global/AutoEvent/debug-output.log`
     - 对于 Exiled：`~/.config/EXILED/Configs/AutoEvent/debug-output.log`
