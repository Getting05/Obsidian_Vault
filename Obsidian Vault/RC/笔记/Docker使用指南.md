cd /usr

为了docker环境中也能使用NVDIA显卡，要现在宿主机中作如下配置
```bash
# 宿主机执行，添加 NVIDIA Docker 软件源
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

# 更新源并安装 nvidia-docker2
sudo apt update
sudo apt install -y nvidia-docker2

# 重启 Docker 服务，使配置生效
sudo systemctl restart docker
```


手动拉取镜像

```bash
sudo docker pull osrf/ros:foxy-desktop
```

```bash
# 允许本地X11转发
xhost +local:
```

以下是小鱼ros创建docker环境的命令，他没有支持nvdia的显卡。
```bash
sudo docker run -dit \
--name=[your_container_name] \
--privileged  \
-v /dev:/dev \
-v /home/[your_username]:/home/[your_username] \
-v /tmp/.X11-unix:/tmp/.X11-unix  \
-e DISPLAY=unix$DISPLAY \
-w /home/[your_username] \
--net=host \
fishros2/ros:noetic-desktop-full
```

以下是支持Nvdia的版本：
```bash
sudo docker run -dit \
  --name=foxy \
  --privileged \
  --gpus all \
  -v /dev:/dev \
  -v /home/getting:/home/getting \
  -v /tmp/.X11-unix:/tmp/.X11-unix \
  -e DISPLAY=unix$DISPLAY \
  --net=host \
  -w /home/getting \
  osrf/ros:foxy-desktop
```



```bash
# 停止并删除旧容器
sudo docker stop foxy
sudo docker rm foxy
```


```bash
sudo tee /etc/modprobe.d/blacklist-nouveau.conf <<-'EOF'
blacklist nouveau
options nouveau modeset=0
EOF
```


docker环境中部分编译路径要手动设置：

```bash
# 将路径添加到 /etc/ld.so.conf.d/ 目录下
sudo sh -c 'echo "/usr/local/lib" > /etc/ld.so.conf.d/livox_sdk.conf'

# 更新动态链接库缓存
sudo ldconfig
```


CUDA的支持



一些方便的配置命令：
```
: >> /dev/null
echo "请输入指令控制foxy: 重启(r) 进入(e) 启动(s) 关闭(c) 删除(d) 测试(t):"
read choose
case $choose in
s) docker start foxy;;
r) docker restart foxy;;
e) docker exec -it foxy /bin/bash;;
c) docker stop foxy;;
d) docker stop foxy && docker rm foxy && sudo rm -rf /home/getting/.fishros/bin/foxy;;
t) docker exec -it foxy  /bin/bash -c "source /ros_entrypoint.sh && ros2";;
esac
newgrp docker
```




