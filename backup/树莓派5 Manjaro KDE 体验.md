
最近入手了一台8G的树莓派5，主要是想测试一下作为桌面PC的体验；树莓派5的提升对比4B非常大。装上Manjaro KDE 出奇的丝滑流畅；

实际上4G的树莓派5就很流畅了，简单介绍一下我的方案：

1. 使用PCIE扩展SSD来作为系统启动盘，SD卡当场退役

2. 使用了官方的智能温控风扇，CPU温度达到60°才会转

3. 自制亚克力外壳保驾护航，现在它是功耗非常低的一台mini主机了


**性能提升对比**

| 特性 / 规格         | 树莓派4B (Raspberry Pi 4 Model B)                 | **树莓派5 (Raspberry Pi 5)**                                | **核心提升与解读**                                                                                             |
| :------------------ | :------------------------------------------------ | :---------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------- |
| **处理器 (CPU)**    | Broadcom BCM2711, 4核 Cortex-A72 (64-bit) @ 1.8GHz | **Broadcom BCM2712, 4核 Cortex-A76 (64-bit) @ 2.4GHz**     | **性能革命**: 架构更先进 (A76 vs A72)，频率更高。综合性能提升 **2-3倍**，这是最核心的飞跃。             |
| **图形处理器 (GPU)**| Broadcom VideoCore VI                             | **Broadcom VideoCore VII @ 800MHz**                         | **图形翻倍**: 游戏、视频解码、桌面特效能力显著增强，支持双4K@60Hz **HDR**。                                |
| **内存 (RAM)**      | 1GB, 2GB, 4GB, 8GB LPDDR4                         | 4GB, 8GB (首发) **LPDDR4X**-4267                             | **速度更快**: LPDDR4X 带宽更高，能更好地配合强大的CPU，减少性能瓶颈。                                          |
| **PCI Express 接口**| **无**                                            | **有 (1x PCIe 2.0 接口, 5GT/s)**                            | **划时代功能**: 可通过M.2 HAT外接 **NVMe SSD**，获得几十倍于SD卡的读写速度，彻底解决存储性能瓶颈。           |
| **南桥芯片**        | 所有I/O集成在主SoC                                | **自研 RP1 南桥芯片**                                       | **I/O性能解放**: 将USB、以太网等I/O功能分离，大幅提升了I/O吞吐能力和并发性能。                           |
| **USB 接口**        | 2x USB 3.0, 2x USB 2.0 (共享带宽)                 | 2x USB 3.0 (支持同时5Gbps), 2x USB 2.0                      | **带宽翻倍**: 同时使用两个高速USB设备不再互相影响，总带宽更高。                                              |
| **摄像头/显示器**   | 1x DSI (显示), 1x CSI (摄像头)                    | **2x MIPI (4-lane)**, 可配置为CSI或DSI                    | **功能更灵活**: 可接双摄像头、双显示屏，或一摄一显，满足更多高级应用场景。                                   |
| **MicroSD 卡槽**    | 支持 SDR50 模式 (~50 MB/s)                        | **支持 SDR104 高速模式 (~100 MB/s)**                        | **速度翻倍**: 即便只用SD卡，只要卡支持，启动和加载速度也会快很多。                                           |
| **电源**            | 5V/3A USB-C                                       | **5V/5A USB-C PD (Power Delivery)**                         | **供电要求更高**: 官方推荐27W电源，以确保为CPU和高速外设提供充足电力。                                       |
| **电源按钮**        | **无**                                            | **有 (板载实体按钮)**                                       | **操作便利**: 无需再通过命令行或拔插电源来开关机。                                                           |
| **实时时钟 (RTC)**  | **无**                                            | **有 (需外接电池)**                                         | **时间准确**: 断电后仍可保持时间，对日志记录、定时任务等场景至关重要。                                       |
| **风扇接口**        | **无 (需占用GPIO)**                               | **有 (专用4-pin PWM温控接口)**                              | **智能散热**: 可根据CPU温度自动调节风扇转速，高效且安静。                                                      |
| **无线连接**        | Wi-Fi 5 (802.11ac), Bluetooth 5.0                 | Wi-Fi 5 (802.11ac), Bluetooth 5.0 / BLE                     | **基本持平**: 无线规格没有大的变化。                                                                         |
| **以太网**          | 千兆以太网 (Gigabit Ethernet)                     | 千兆以太网 (Gigabit Ethernet)                             | **基本持平**: 接口速率相同。                                                                                 |
| **GPIO**            | 标准40-pin GPIO                                   | 标准40-pin GPIO                                           | **保持兼容**: 接口布局和功能保持兼容，但部分引脚功能因RP1芯片而有变化。                                      |

---

**刷入manjaro**

将manjaro kde 写入SSD：

1. 可以通过进入SD卡启动的默认系统，安装rpi-imager 官方刷写工具将下载的manjaro镜像写入

这里我推荐20250414日期的镜像，不过20240414的镜像似乎资源占用更低，但不在镜像列表了：

manjaro镜像地址：[https://github.com/manjaro-arm/opi5-images/releases/tag/20250414](https://github.com/manjaro-arm/opi5-images/releases/tag/20250414)

2. 或者你可以直接通过硬盘盒在另一台电脑上将manjaro镜像写入，这似乎更直接点

**树莓派前置配置**

启用PCIE：

编辑`config.txt`，将`dtparam`行改成`dtparam=pcie_gen=2`

```
sudo nano /boot/firmware/config.txt
```

设置启动顺序：

```
sudo raspi-config
```

1. 导航到 6 Advanced Options -> A5 Boot Order

2. 选择 B1 NVMe/SSD Boot (优先从NVMe/SSD启动)

3. 选择 <Ok> -> <Finish>，然后根据提示关机

4. 拔掉电源，拔出SD卡，然后装上您的NVMe SSD和M.2扩展板

5. 接通电源，树莓派现在就会自动从NVMe SSD启动了

**进入KDE桌面**

树莓派开机后直接进入桌面，设置一些相关的配置后即可，接下来就是愉快的使用时间啦！

![屏幕截图_20250714_230547.png](https://lihe.ink/2025/07/16/6877c7c0a3e5c.png)

**安装中文输入法**

安装 Fcitx5 核心组件：

```
sudo pacman -S fcitx5-im fcitx5-chinese-addons fcitx5-qt fcitx5-gtk
```

配置环境变量：

```
sudo bash -c 'cat >> /etc/environment <<EOL
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
EOL'
```

重启电脑：

```
reboot
```

现在中文输入法就配置完成了！
