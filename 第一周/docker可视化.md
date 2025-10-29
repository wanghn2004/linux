##### Docker Portainer CE 中文版

```
docker pull swr.cn-north-4.myhuaweicloud.com/ddn-k8s/ghcr.io/outlovecn/portainer-cn:main
```

##### 创建数据卷

```
docker volume create portainer_data
```

##### 步骤 3：启动 Portainer

```
docker run -d \
  --name portainer \
  -p 7000:9000 \  # 主机端口7000映射到容器内9000端口
  --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer:main
```

##### 步骤 4：访问 Portainer Web 界面

```
http://localhost:9000/
```

##### 无法拉取镜像

如果在拉取 Docker 镜像时遇到问题，请编辑 `/etc/docker/daemon.json` 文件并添加以下内容（如果不存在就创建）：

```
# 编辑
sudo vi /etc/docker/daemon.json 
#添加如下代码
{
  "registry-mirrors": [
    "https://docker.m.daocloud.io",
    "https://noohub.ru",
    "https://huecker.io",
    "https://dockerhub.timeweb.cloud",
    "https://docker.rainbond.cc",
    "https://dockerhub.icu",
    "https://docker.chenby.cn",
    "https://docker.1panel.live",
    "https://docker.awsl9527.cn",
    "https://docker.anyhub.us.kg",
    "https://dhub.kubesre.xyz",
    "https://docker.1ms.run",
    "https://docker.xuanyuan.me"
  ]
}
                        
```

完成编辑后，重新加载 `daemon.json` 文件并重启 Docker：

```
sudo systemctl daemon-reload
sudo systemctl restart docker
                        
```