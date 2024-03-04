# my-nri 插件示例


## Containerd启用 NRI
[plugins."io.containerd.nri.v1.nri"] 中 disable 改为 true

```yaml
[plugins."io.containerd.nri.v1.nri"]
    disable = false
    disable_connections = false
    plugin_config_path = "/etc/nri/conf.d"
    plugin_path = "/opt/nri/plugins"
    plugin_registration_timeout = "5s"
    plugin_request_timeout = "2s"
    socket_path = "/var/run/nri/nri.sock"
```

## 启用 my-nri

### 准备配置文件
配置文件 `/etc/nri/conf.d/02-mynri.conf`

```shell
tee /etc/nri/conf.d/02-my-nri.conf <<- EOF
node_name: mytestNode
EOF
```


### 准备插件

build 
```
git clone https://github.com/zhaojizhuang/containerd-prectice.git
cd containerd-prectice/my-nri/

go build
mv my-nri /opt/nri/plugins/02-my-nri
```

### 重启 Containerd
```shell
systemctl restart containerd
```

### 验证
> 下面是通过 crictl 启动 容器
> 
>  📢 **注意**: 通过 crictl 启动时需停止 kubelet，避免 kubelet 删除相关

```shell
crictl run container-config.json pod-config.json
crictl ps |grep busybox
# 查看是否生效
crictl exec -it <container id> env
```


