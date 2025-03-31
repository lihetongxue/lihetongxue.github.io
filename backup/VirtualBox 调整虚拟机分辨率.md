## 1. 前言

VirtualBox虚拟机安装Windows、Linux等系统后，并且安装**VBoxWindowsAdditions**增强工具及**VirtualBox Extension Pack**扩展的情况下，进入虚拟机后，分辨率选项没有对应显示器的分辨率且只有5个

> [!NOTE]
> 原因是VirtualBox限制了客户机的分辨率

## 2. 调整分辨率

> [!IMPORTANT]
> 可以使用VBoxManage命令调整VirtualBox的虚拟机分辨率。以下是调整分辨率的步骤：

**1. 打开命令提示符或终端窗口，并导航到VirtualBox安装目录的文件夹（默认情况下，它位于C:\Program Files\Oracle\VirtualBox）。
运行以下命令来列出所有可用的虚拟机：**

```
VBoxManage list vms
```

这将显示所有已经创建的虚拟机及其UUID。

**2. 使用以下命令来设置虚拟机的新分辨率：**

```
VBoxManage setextradata <虚拟机UUID> "CustomVideoMode1" "<宽度>x<高度>x<颜色深度>"
```

**3. 替换<虚拟机UUID>为你实际的虚拟机UUID，<宽度>和<高度>为所需的分辨率，<颜色深度>为所需的颜色深度（例如32）。 例如，要将分辨率设置为1920x1080，并使用32位颜色深度，可以运行以下命令：**

```
VBoxManage setextradata <虚拟机UUID> "CustomVideoMode1" "1920x1080x32"
```

**4. 最后，启动虚拟机，并在虚拟机中选择所需的分辨率。方法可能因虚拟机操作系统的不同而有所不同，因此请参考虚拟机操作系统的文档以了解如何更改分辨率。**

## 3. 结尾

**如果你没有将VirtualBox添加到系统环境变量中，则Powershell 中的命令是这样（绝对路径）：**

```
& "C:\Program Files\Oracle\VirtualBox\VBoxManage.exe" setextradata "<虚拟机UUID>" "CustomVideoMode1" "1920x1080x32"
```

> [!NOTE]
> 请注意，更改分辨率可能会影响虚拟机的性能和显示质量。如果你遇到任何问题，可以通过删除添加的CustomVideoMode规则来恢复默认分辨率。
