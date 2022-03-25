## Terminal Beauty

#### 1 安装 `oh-my-posh` 和 `posh-git`

`oh-my-posh` 是 PowerShell 主题管理工具

`posh-git` 可以实现类似 `oh-my-zsh` 一样的 `Git` 命令增强工具（命令别名和显示分支信息等）

```sh
Install-Module posh-git
```

```shell
Install-Module oh-my-posh
```

#### 2 配置 PowerShell

开启脚本执行权限

```shell
set-ExecutionPolicy RemoteSigned
```

查看PowerShell的主目录

```shell
$pshome
```

查看PowerShell 配置文件的路径

```shell
$profile
```

确定是否已经在系统上创建了PowerShell 配置文件

```shell
test-path $profile
```

根据路径打开文件并编辑，内容如下

```
Import-Module posh-git
Import-Module oh-my-posh
Set-PoshPrompt -Theme Paradox
```

这里可能会出现乱码的问题，需要安装对应的字体，https://www.nerdfonts.com/下载Cousine Nerd Font字体并安装

![terminal_beauty_1](/img/terminal_beauty_1.png)

在【设置】->【打开JSON文件】，替换键为`profiles`的值

```json
"profiles": {
    "defaults": {
        "acrylicOpacity": 0.59999999999999998,
        "font": {
            "face": "Cousine Nerd Font"
        },
        "useAcrylic": true
    },
    "list": [
        {
            "commandline": "powershell.exe",
            "guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
            "hidden": false,
            "name": "Windows PowerShell"
        },
        {
            "commandline": "cmd.exe",
            "guid": "{0caa0dad-35be-5f56-a8ff-afceeeaa6101}",
            "hidden": false,
            "name": "\u547d\u4ee4\u63d0\u793a\u7b26"
        },
        {
            "guid": "{b453ae62-4e3d-5e58-b989-0a998ec441b8}",
            "hidden": false,
            "name": "Azure Cloud Shell",
            "source": "Windows.Terminal.Azure"
        }
    ]
},
```





