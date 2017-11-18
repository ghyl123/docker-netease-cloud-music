[![Docker Image](https://img.shields.io/badge/docker%20image-available-green.svg)](https://hub.docker.com/r/bestwu/netease-cloud-music/)
[![Image Layers and Size](https://images.microbadger.com/badges/image/bestwu/netease-cloud-music.svg)](http://microbadger.com/images/bestwu/netease-cloud-music)

本镜像基于[深度操作系统](https://www.deepin.org/download/)

# 准备工作

允许所有用户访问X11服务,运行命令:

```bash
    xhost +
```

## 运行

### docker-compose

```yml
version: '2'
services:
  music:
    image: bestwu/netease-cloud-music
    container_name: music
    privileged: true
    volumes:
      - /tmp/.X11-unix:/tmp/.X11-unix
      - /run/user/1000/pulse/native:/run/user/1000/pulse/native
      - $HOME/Music:/home/netease/Music
    environment:
      - DISPLAY=unix$DISPLAY
      - PULSE_SERVER=unix:/run/user/1000/pulse/native
      - XDG_RUNTIME_DIR=/run/user/1000
      - QT_IM_MODULE=fcitx
      - XMODIFIERS=@im=fcitx
      - GTK_IM_MODULE=fcitx
    # command: --no-sandbox #如果不想设置privileged: true，可使用此参数以no-sandbox 方式运行
```
或

```bash
    docker run -d --name music --privileged=true\
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -v ${XDG_RUNTIME_DIR}/pulse/native:${XDG_RUNTIME_DIR}/pulse/native \
    -v $HOME/Music:/home/netease/Music \
    -e DISPLAY=unix$DISPLAY \
    -e PULSE_SERVER=unix:${XDG_RUNTIME_DIR}/pulse/native \
    -e XDG_RUNTIME_DIR=${XDG_RUNTIME_DIR} \
    -e XMODIFIERS=@im=fcitx \
    -e QT_IM_MODULE=fcitx \
    -e GTK_IM_MODULE=fcitx \
    bestwu/netease-cloud-music
```