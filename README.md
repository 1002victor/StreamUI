<div align="center">
  <img src="./frontend/assets/logo.svg" width="64px"/>
  <h1>StreamUI</h1>
  <a>
    <img src="https://travis-ci.org/xia-chu/ZLMediaKit.svg?branch=master">
    <img src="https://img.shields.io/badge/language-python-EE4C2C.svg">
    <img src="https://img.shields.io/badge/license-MIT-green.svg">
    <img src="https://img.shields.io/badge/platform-linux%20|%20macos%20|%20windows-blue.svg">
    <img src="https://img.shields.io/badge/PRs-welcome-yellow.svg">
  </a>

</div>

## Overview

🚀 一个极简、轻便的视频流媒体管理平台 | A minimal and lightweight video streaming management platform

> StreamUI 中 Stream 取自 [ZLMediaKit](https://github.com/ZLMediaKit/ZLMediaKit) 流概念，UI 取自 [Layui](https://github.com/layui/layui)，主题色以蓝绿色（#16baaa）为主，以简单、易用、可扩展为设计理念，在代码复杂度和功能实现度之间反复不断取舍 | The name StreamUI combines "Stream" — inspired by the streaming concept from ZLMediaKit — and "UI" — drawn from Layui. Designed around simplicity, ease of use, and extensibility, StreamUI features a teal color scheme (#16baaa) and carefully balances code complexity against functional completeness.

> StremUI 力求做到简单易用，同时具备良好的扩展性，方便二次开发 | StreamUI strives to be both intuitive for end users and highly extensible for developers, making it easy to customize and integrate into your own projects.

## Supported Features

- 支持 RTSP/RTMP/HLS/WebRTC/RTP/GB28181 等主流协议的拉流推流接入 | Supports ingest and egress via mainstream streaming protocols, including RTSP, RTMP, HLS, WebRTC, RTP, and GB28181

- 支持 ONVIF 设备识别，云台控制 | Supports ONVIF device discovery and PTZ (pan-tilt-zoom) control。

- 支持分发 RTSP/WebRTC/RTMP/FLV/HLS/HLS-fMP4/HTTP-TS/HTTP-fMP4 等协议 | Supports stream distribution over multiple protocols: RTSP, WebRTC, RTMP, FLV, HLS, HLS-fMP4, HTTP-TS, and HTTP-fMP4

- 支持多屏播放 | Enables multi-screen playback for simultaneous stream viewing

- 支持流本地录制、回放、下载、自动清理，支持事件录制（事件发生前 n 秒+事件发生后 n 秒） | Provides local stream recording, playback, download, and automatic cleanup; supports event-triggered recording (capturing n seconds before and after an event)

- 支持 GB28181 接入/级联（coming soon ...）| GB28181 ingest and cascading support (coming soon...)

## Quick Start

本项目推荐 `docker compose` 部署 | This project is best deployed using Docker Compose

```bash
cd ./docker
docker compose up -d   # Use `docker-compose up -d` if you're on an older Docker version
```

开启后，访问 `http://{服务器地址}:10800` 即可登录，默认密码为 `streamui`（可在 `./frontend/login.html` 修改密码） | Once started, visit http://{your-server-ip}:10800 in your browser to log in.The default password is streamui (you can change it directly in ./frontend/login.html).

如果修改配置后需要重启，请运行 | If you modify the configuration and need to apply changes, restart the services with

```bash
docker compose restart
```

🤗 推荐启动后，先根据业务需要修改配置再重启使用（重启后需重新拉流）|  Recommendation: After initial startup, adjust the settings according to your specific use case before restarting the service (note that streams will need to be re-ingested after a restart).

- 考虑开启按需转发，优点是节省带宽，缺点是第一个观众观看时，需要等待转发流启动 | Consider enabling on-demand forwarding to save bandwidth. The downside is that the first viewer will have to wait for the forwarding stream to start.

- 考虑关掉不需要转发的协议，比如不需要分发 RTMP 协议，就关掉 RTMP 转发 | Consider disabling unnecessary protocols, such as RTMP forwarding if it's not required for distribution.

- 考虑开启 faststart，优点是播放时可以快速 seek，缺点是录制时需要多占用一些存储空间 | Consider enabling faststart to enable fast seeking during playback. The downside is that more storage space will be required for recording.

- 考虑增大 GOP 缓存，优点是播放平滑，录制事件视频回溯时间变长，缺点是增大内存占用 | Consider increasing the GOP cache size to achieve smoother playback and longer event video playback. The downside is increased memory usage.

- 更多选项深入研究请参考 ZLMediaKit 的 [配置说明](https://github.com/ZLMediaKit/ZLMediaKit/tree/master/conf) | For more options, refer to the [configuration documentation](https://github.com/ZLMediaKit/ZLMediaKit/tree/master/conf) in ZLMediaKit.

## Snapshots

<img src="./snapshots/login.png" alt="wall" style="zoom:33%;" />

<img src="./snapshots/home.png" alt="home" style="zoom: 33%;" />

## Development

StreamUI 是一个前后端分离的项目，前端使用 HTML + Layui，后端使用 Python + FastAPI | StreamUI is a front-end and back-end separation project, with the front-end using HTML + Layui and the back-end using Python + FastAPI.

> StreamUI 追求极简实现，所以没有用到 Vue、React 等前端框架，及大而全的 Java Spring 后端框架，而是选择了轻量级的 Layui 和 FastAPI，方便二次开发 | StreamUI aims for minimal implementation, so it does not use front-end frameworks like Vue or React, nor does it employ comprehensive back-end frameworks like Java Spring. Instead, it uses lightweight tools like Layui and FastAPI, making it easy to customize and extend.

```bash
├── backend  # 后端服务 | Backend services
│   ├── main.py  # 主程序入口 | Main program entry
│   ├── onvif  # ONVIF 设备识别 | ONVIF device discovery
│   │   ├── api.py
│   │   ├── client.py
│   │   └── wsdl
│   ├── scheduler.py  # 定时任务 | Scheduled tasks
│   └── utils.py  # 工具函数 |    
│
├── frontend
│   ├── assets  # 静态资源 | Static resources
│   ├── index.html  # 主页面 | Main page
│   ├── login.html  # 登录页面 | Login page
│   ├── mime.types  # mime 类型 | MIME types
│   ├── nginx.conf  # nginx 配置 | Nginx configuration
│   └── pages  # 子页面 | Subpages
│       ├── api-docs.html  # 接口文档 | API documentation
│       ├── home.html  # 首页概览 | Home overview
│       ├── playback.html  # 录像回放 | Video playback
│       ├── pull-stream.html  # 主动拉流 | Active ingest
│       ├── settings.html  # 基础配置 | Basic configuration
│       ├── stream-push.html  # 被动推流 | Passive egress
│       └── wall.html  # 分屏展示 | Multi-screen display
```

## Thanks

- [ZLMediaKit](https://github.com/ZLMediaKit/ZLMediaKit)
- [Layui](https://github.com/layui/layui)
- [FastAPI](https://fastapi.tiangolo.com/)

🤗 ZLMediaKit https://github.com/ZLMediaKit/ZLMediaKit has included this project

## License

StreamUI is licensed under the [MIT License](LICENSE).

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=lmk123568/StreamUI&type=date&legend=top-left)](https://www.star-history.com/#lmk123568/StreamUI&type=date&legend=top-left)