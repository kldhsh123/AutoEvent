# 安装指南

## Exiled 设置
## 1. 下载和设置 :moyai:
### *您需要下载最新版本：*

- [``AutoEvent.dll``](https://github.com/kldhsh123/AutoEvent/releases/latest) 移动到 => ``EXILED/Plugins``

- [``MapEditorReborn.dll``](https://github.com/kldhsh123/AutoEvent/tree/main/releases/download/v.9.11.1/MapEditorReborn.dll) 移动到 => ``EXILED/Plugins``

- [``Music.tar.gz``](https://github.com/kldhsh123/AutoEvent/releases/latest) 解压文件到 => ``EXILED/Configs/AutoEvent/Music``

- [``Schematics.tar.gz``](https://github.com/kldhsh123/AutoEvent/releases/latest) 解压文件到 => ``EXILED/Configs/AutoEvent/Schematics``

- 确保配置具有以下目录，并且服务器可以访问它们。
```yml
# 示意图目录的位置。默认情况下，它位于 AutoEvent 文件夹中。
schematics_directory_path: /home/container/.config/EXILED/Configs/AutoEvent/Schematics
# 音乐目录的位置。默认情况下，它位于 AutoEvent 文件夹中。
music_directory_path: /home/container/.config/EXILED/Configs/AutoEvent/Music
```
- ***有时这些设置无法在配置中正确自动生成，因此请在联系我们之前仔细检查它们是否有效。***


## 2. 权限设置 :gem:
### *在 ``EXILED/Configs/permissions.yml`` 中为您的角色授予权限：*

```
owner:
  inheritance: [ ]
  permissions:
    - ev.*
```
可用权限树：
```
ev.*           - 所有 AutoEvent 命令的主权限。
  - ev.list    - 查看可用事件。
  - ev.run      - 运行事件。
  - ev.stop     - 停止事件。
  - ev.volume   - 更改所有事件的音量。
  - ev.language - 更改翻译语言。
```
