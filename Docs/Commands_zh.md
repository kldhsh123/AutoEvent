# 命令

| 命令                              | 描述                                                    |
|--------------------------------------|----------------------------------------------------------------|
| `ev list`                            | 显示可用小游戏列表。                          |
| `ev run [参数]`                  | 通过 `ev_list` 命令中的参数启动小游戏 |
| `ev stop`                            | 终止卡住的小游戏（只是杀死所有玩家）           |
| `ev volume [0 - 200]`                | 更改全局音量百分比。                              |
| `ev language list`                   | 列出所有可用翻译。                            |
| `ev language load [语言]`        | 加载翻译。它将替换当前翻译。 |

----

# 描述 :frog:

``ev list`` => *要查看所有小游戏，请在管理员控制台中输入命令：*
![image](https://github.com/user-attachments/assets/5b9b0c05-2d87-4c3a-bb0f-9d2a6946c8f9)
- 此命令将显示所有小游戏。
- 例如，``[escape] <= 在 SCP-173 后面以超音速逃离设施！``
- 方括号表示可以在命令 ``ev run`` 中使用的参数

![image](https://github.com/user-attachments/assets/ed3b3784-8d5e-458b-b172-5b3fcfed0e9b)

---
``ev run [名称]`` => *要启动小游戏，请输入命令：*
- 此命令通过指定的参数启动小游戏。
- 例如，``ev run escape``

![image](https://github.com/user-attachments/assets/c280bc27-4afa-4c8d-9fb8-96d9777b2ef4)

---
``ev stop`` => *如果小游戏在游戏过程中卡住，请输入命令：*
- 此命令将通过杀死服务器上的所有玩家来停止小游戏。
