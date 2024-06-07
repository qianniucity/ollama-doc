Ollama 对GPU 支持信息
=======================

**Nvidia**
--------------

Ollama 支持计算能力 5.0 及以上的 Nvidia GPU。

要检查您的显卡是否受支持，请查看您的计算兼容性：https://developer.nvidia.com/cuda-gpus

::

    | ------------------ | ------------------- | ----------------------------------------------------------------------------------------------------------- |
    | Compute Capability | Family              | Cards                                                                                                       |
    | ------------------ | ------------------- | ----------------------------------------------------------------------------------------------------------- |
    | 9.0                | NVIDIA              | `H100`                                                                                                      |
    | 8.9                | GeForce RTX 40xx    | `RTX 4090` `RTX 4080` `RTX 4070 Ti` `RTX 4060 Ti`                                                           |
    |                    | NVIDIA Professional | `L4` `L40` `RTX 6000`                                                                                       |
    | 8.6                | GeForce RTX 30xx    | `RTX 3090 Ti` `RTX 3090` `RTX 3080 Ti` `RTX 3080` `RTX 3070 Ti` `RTX 3070` `RTX 3060 Ti` `RTX 3060`         |
    |                    | NVIDIA Professional | `A40` `RTX A6000` `RTX A5000` `RTX A4000` `RTX A3000` `RTX A2000` `A10` `A16` `A2`                          |
    | 8.0                | NVIDIA              | `A100` `A30`                                                                                                |
    | 7.5                | GeForce GTX/RTX     | `GTX 1650 Ti` `TITAN RTX` `RTX 2080 Ti` `RTX 2080` `RTX 2070` `RTX 2060`                                    |
    |                    | NVIDIA Professional | `T4` `RTX 5000` `RTX 4000` `RTX 3000` `T2000` `T1200` `T1000` `T600` `T500`                                 |
    |                    | Quadro              | `RTX 8000` `RTX 6000` `RTX 5000` `RTX 4000`                                                                 |
    | 7.0                | NVIDIA              | `TITAN V` `V100` `Quadro GV100`                                                                             |
    | 6.1                | NVIDIA TITAN        | `TITAN Xp` `TITAN X`                                                                                        |
    |                    | GeForce GTX         | `GTX 1080 Ti` `GTX 1080` `GTX 1070 Ti` `GTX 1070` `GTX 1060` `GTX 1050`                                     |
    |                    | Quadro              | `P6000` `P5200` `P4200` `P3200` `P5000` `P4000` `P3000` `P2200` `P2000` `P1000` `P620` `P600` `P500` `P520` |
    |                    | Tesla               | `P40` `P4`                                                                                                  |
    | 6.0                | NVIDIA              | `Tesla P100` `Quadro GP100`                                                                                 |
    | 5.2                | GeForce GTX         | `GTX TITAN X` `GTX 980 Ti` `GTX 980` `GTX 970` `GTX 960` `GTX 950`                                          |
    |                    | Quadro              | `M6000 24GB` `M6000` `M5000` `M5500M` `M4000` `M2200` `M2000` `M620`                                        |
    |                    | Tesla               | `M60` `M40`                                                                                                 |
    | 5.0                | GeForce GTX         | `GTX 750 Ti` `GTX 750` `NVS 810`                                                                            |
    |                    | Quadro              | `K2200` `K1200` `K620` `M1200` `M520` `M5000M` `M4000M` `M3000M` `M2000M` `M1000M` `K620M` `M600M` `M500M`  |



**GPU 选择**

如果您的系统中有多个  NVIDIA GPU，并且您想限制 Ollama 只使用其中的一部分，您可以设置 CUDA_VISIBLE_DEVICES 为 GPU 的逗号分隔列表。虽然可以使用数字 ID，但由于排序可能会变化，所以使用 UUID 更为可靠。您可以通过运行 nvidia-smi -L 来查找 GPU 的 UUID。如果您想忽略 GPU 并强制使用 CPU，请使用无效的 GPU ID（例如，“-1”）。

**笔记本电脑的挂起与恢复**

在 Linux 上，经过挂起/恢复周期后，有时 Ollama 可能会无法识别您的 NVIDIA GPU，并改为在 CPU 上运行。您可以通过以下命令重新加载 NVIDIA UVM 驱动来解决这个问题：

::

    sudo rmmod nvidia_uvm && sudo modprobe nvidia_uvm


**AMD Radeon**
-----------------

Ollama 支持以下 AMD GPU：


::
    
    | -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
    | Family         | Cards and accelerators                                                                                                               |
    | -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
    | AMD Radeon RX  | `7900 XTX` `7900 XT` `7900 GRE` `7800 XT` `7700 XT` `7600 XT` `7600` `6950 XT` `6900 XTX` `6900XT` `6800 XT` `6800` `Vega 64` `Vega 56`    |
    | AMD Radeon PRO | `W7900` `W7800` `W7700` `W7600` `W7500` `W6900X` `W6800X Duo` `W6800X` `W6800` `V620` `V420` `V340` `V320` `Vega II Duo` `Vega II` `VII` `SSG` |
    | AMD Instinct   | `MI300X` `MI300A` `MI300` `MI250X` `MI250` `MI210` `MI200` `MI100` `MI60` `MI50`                                                               |


**覆盖**

Ollama 利用 AMD ROCm 库，但它并不支持所有 AMD GPU。在某些情况下，您可以强制系统尝试使用接近的 LLVM 版本。例如，Radeon RX 5400 是 gfx1034（也称为 10.3.4），但 ROCm 目前不支持此版本。最接近的支持是 gfx1030。您可以通过设置环境变量 HSA_OVERRIDE_GFX_VERSION="10.3.0"，来尝试在不受支持的 AMD GPU 上运行。如果您有不受支持的 AMD GPU，您可以尝试使用下面列出的支持类型。

目前已知支持的 GPU 类型包括以下 LLVM 版本。此表显示了映射到这些 LLVM 版本的一些示例 GPU：

::

    |-----------------|---------------------|
    | LLVM Target | An Example GPU |
    |-----------------|---------------------|
    | gfx900 | Radeon RX Vega 56 |
    | gfx906 | Radeon Instinct MI50 |
    | gfx908 | Radeon Instinct MI100 |
    | gfx90a | Radeon Instinct MI210 |
    | gfx940 | Radeon Instinct MI300 |
    | gfx941 | |
    | gfx942 | |
    | gfx1030 | Radeon PRO V620 |
    | gfx1100 | Radeon PRO W7900 |
    | gfx1101 | Radeon PRO W7700 |
    | gfx1102 | Radeon RX 7600 |


AMD 正在努力通过即将发布的 ROCm v6 增强版来扩大对 GPU 系列的支持，这将增加更多 GPU 的支持。

**GPU 选择**

如果您的系统中安装了多个 AMD GPU，并且您想限制 Ollama 只使用其中的一部分，您可以将 HIP_VISIBLE_DEVICES 设置为 GPU 的逗号分隔列表。您可以使用 rocminfo 命令查看设备列表。如果您想忽略 GPU 并强制使用 CPU，请使用无效的 GPU ID（例如，“-1”）。

**容器权限**

在某些 Linux 发行版中，SELinux 可能会阻止容器访问 AMD GPU 设备。您可以在宿主系统上运行 **sudo setsebool container_use_devices=1** 来允许容器使用设备。

Metal（苹果 GPU）

Ollama 通过 Metal API 支持苹果设备上的 GPU 加速。