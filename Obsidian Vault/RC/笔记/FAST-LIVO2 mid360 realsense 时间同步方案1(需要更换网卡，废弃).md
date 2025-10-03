https://livox-wiki-cn.readthedocs.io/zh-cn/latest/tutorials/new_product/common/time_sync.html#ptp
https://livox-wiki-cn.readthedocs.io/zh-cn/latest/tutorials/new_product/common/time_sync.html#id10

安装 `ethtool`：

```bash
sudo apt update && sudo apt install -y ethtool
```

确定本机网卡


本机网卡名： enp8s0

(base) getting@PC:~/下载/linuxptp-3.1.1$ ethtool -T enp8s0
Time stamping parameters for enp8s0:
Capabilities:
	software-transmit     (SOF_TIMESTAMPING_TX_SOFTWARE)
	software-receive      (SOF_TIMESTAMPING_RX_SOFTWARE)
	software-system-clock (SOF_TIMESTAMPING_SOFTWARE)
PTP Hardware Clock: none
Hardware Transmit Timestamp Modes: none
Hardware Receive Filter Modes: none

开启功能
```bash
sudo ptp4l -i enp8s0-l 6 -m
```


发现本机网卡不支持该功能
若需要真正的 PTP 高精度同步（如工业控制、测量等场景），必须更换为 **支持硬件时间戳的有线网卡**。常见的支持 PTP 的网卡型号：

- Intel：I210、I350、X710 等；
- Broadcom：5720、57416 等；
- 部分工业级网卡（如研华、凌华等品牌）。

尝试软件层面同步：精度只有毫秒级

使用 -S 选项强制启用软件时间戳：

```
sudo ptp4l -i enp8s0 -l 6 -m -S
```




add camera and lidar sync
https://github.com/ZhifangFu/livox_ros_driver2/tree/main



