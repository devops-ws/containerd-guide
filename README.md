# containerd-guide

## 安装
```shell
hd i containerd
hd i containernetworking/plugins
```

[配置文件](https://github.com/containerd/containerd/blob/main/docs/man/containerd-config.toml.5.md)：`/etc/containerd/config.tom`

## 启动
```shell
containerd
```

## 拉取镜像
```shell
crictl pull alpine 
```

## 设置镜像
下面的配置文件将多个 Registry 映射到了本地部署的 Harbor 上：

* docker.io -> harbor/cache
* quay.io -> habor/quay.io
* ghcr.io -> harbor/ghcr.io

```toml
# vim /etc/containerd/config.toml
version = 2

[plugins."io.containerd.grpc.v1.cri".registry]
  [plugins."io.containerd.grpc.v1.cri".registry.mirrors]
    [plugins."io.containerd.grpc.v1.cri".registry.mirrors."10.121.218.184:30002"]
      endpoint = ["http://10.121.218.184:30002"]
    [plugins."io.containerd.grpc.v1.cri".registry.mirrors."docker.io"]
      endpoint = ["http://10.121.218.184:30002/v2/cache", "https://qtzsrp4m.mirror.aliyuncs.com/v2"]
    [plugins."io.containerd.grpc.v1.cri".registry.mirrors."quay.io"]
      endpoint = ["http://10.121.218.184:30002/v2/quay.io"]
    [plugins."io.containerd.grpc.v1.cri".registry.mirrors."ghcr.io"]
      endpoint = ["http://10.121.218.184:30002/v2/ghcr.io"]
    [plugins."io.containerd.grpc.v1.cri".registry.mirrors."k8s.gcr.io"]
      endpoint = ["https://registry.aliyuncs.com/v2/google_containers"]

[plugins."io.containerd.grpc.v1.cri".registry.configs."10.121.218.184:30002".tls]
  insecure_skip_verify = true

[plugins."io.containerd.grpc.v1.cri".registry.configs]
  [plugins."io.containerd.grpc.v1.cri".registry.configs."10.121.218.184:30002".auth]
    username = "robot_readonly"
    password = 'npcCnfZicoeZTupZnX39ew9cfIvldyZV'
```

## 树莓派
```shell
hd install runc --arch armhf
hd i containernetworking/plugins
hd i k9s
```
