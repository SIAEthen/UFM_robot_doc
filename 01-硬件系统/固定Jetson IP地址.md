### 安装netplan

```sudo apt install netplan.io```

### 具体操作步骤

```sudo vim /etc/netplan/99-static.yaml  # 新建或修改YAML文件```

写入下面内容

```

network:

  version: 2

  renderer: networkd

  ethernets:

    eth0:                         # 网卡名（通过`ip addr`确认）

      addresses:

        - 192.168.1.100/24        # 静态IP/子网掩码

      routes:

        - to: default

          via: 192.168.1.1        # 网关

      nameservers:

        addresses: [8.8.8.8, 1.1.1.1]  # DNS服务器[1,6](@ref)

```

应用配置

```sudo netplan apply  # 生效配置```