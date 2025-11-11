<div align="center">
  <img src="./frontend/assets/logo.svg" width="64px"/>
  <h1>StreamUI</h1>
</div>

### 概述

一个极简、轻便的 NVR

- 支持 RTSP/RTMP/HLS/SRT/WebRTC/RTP/GB28181 等协议接入

- 支持 Onvif 协议接入, 云台控制

- 支持分发 RTSP/WebRTC/RTMP/FLV/HLS/HLS-fMP4/HTTP-TS/HTTP-fMP4 等协议

- 支持多屏播放

- 支持视频本地录制、回放、下载、自动清理

- 支持 GB28181 级联（待开发...）

### 快速启动

本项目推荐 docker compose 部署，密码默认为 litenvr

```bash
cd ./docker
docker compose up -d
```

如果修改参数后需要重启

```bash
docker compose restart
```

### 效果图

<img src="./snapshots/login.png" alt="wall" style="zoom:33%;" />

<img src="./snapshots/home.png" alt="home" style="zoom: 33%;" />
