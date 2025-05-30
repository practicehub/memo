⚠️ 注意：自2024年起，WSL2的网桥（bridged）网络模式已被官方弃用。

官方说明：https://aka.ms/wslnetworking

建议优先使用端口转发或自动同步脚本等替代方案。

以下内容仅供参考，实际效果可能受限于WSL2当前版本。

实际情况：

  部分WSL2版本和环境下，bridged模式仍可通过.wslconfig配置使用，但未来可能彻底失效，兼容性和稳定性无法保证，建议仅做临时或测试用途。

### 1. 安装Hyper-V（如未安装）
```
#   控制面板 -> 程序和功能 -> 启用或关闭Windows功能 -> 勾选“Hyper-V”及其子项
```

### 2. 创建Hyper-V外部虚拟交换机
```
#   - 打开“Hyper-V管理器”
#   - 选择“虚拟交换机管理器”
#   - 新建“外部”类型虚拟交换机，绑定到你的物理网卡
#   - 记下虚拟交换机名称（如“WSLBridge”）
```

### 3. 配置WSL2使用该虚拟交换机
```
#   - 关闭所有WSL2实例
#   - 编辑 C:\Users\<你的用户名>\.wslconfig 文件（如无则新建）
#   - 添加如下内容（将WSLBridge替换为你的虚拟交换机名）：

[wsl2]
networkingMode=bridged
vmSwitch=WSLBridge
```

### 4. 重启WSL2虚拟机
```
#   - 在PowerShell或者Command窗口中运行：
wsl --shutdown
#   - 然后重新启动WSL2
```

### 5. 在WSL2中配置静态IP
```
#   - 进入WSL2，编辑 /etc/netplan/01-netcfg.yaml（或 /etc/netplan/50-cloud-init.yaml，文件名可能不同）
#   - 示例内容如下（请根据实际网卡名和网络配置调整）：

network:
  version: 2
  ethernets:
    eth0:
      dhcp4: no
      addresses: [192.168.1.100/24]
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8,8.8.4.4]
```

### - 应用配置：
```bash
sudo netplan apply
```

### 6. 检查网络
```
#   - 用 ifconfig 或 ip addr 查看IP是否生效
#   - 测试网络连通性
```

```
# 注意事项：
# - 不是所有网卡/网络环境都支持桥接，部分无线网卡可能不支持。
# - 桥接后WSL2与主机在同一网段，可直接访问。
# - 若遇到网络不通，检查防火墙和网卡配置。
```

### 7. [NVIDIA CUDA 安装](https://docs.nvidia.com/cuda/wsl-user-guide/index.html)

### 8. [AnythingLLM 安装](https://docs.anythingllm.com/installation-docker/available-images)
