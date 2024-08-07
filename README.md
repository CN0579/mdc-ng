# MDC-N(ext)G(eneration)
Yet another Movie Data Capture tool

[![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/mdc-ng/mdc-ng?label=mdc-ng)](https://github.com/mdc-ng/mdc-ng/releases)
[![Docker Pulls](https://img.shields.io/docker/pulls/mdcng/mdc?color=orange)](https://hub.docker.com/r/mdcng/mdc/tags)

Telegram发布频道：https://t.me/mdc_ng
> [!WARNING]
> **本项目处于开发阶段，保持高频率的功能更新，遇到使用问题请提交Issue或加入TG讨论组**

## Features
- [x] WebUI交互界面
- [x] 高性能、高质量刮削器，多刮削源同步抓取元数据，支持多种整理方式。目前已经支持近20个高质量刮削源，新源适配陆续添加中
- [x] 支持多目录监控，检测到新文件自动刮削入库
- [x] 手动存量整理
- [x] 联动Emby演员信息、图片刮削
- [x] 图片自动添加标签、多引擎文本翻译、内置演员数据库、sehuatang标题数据库等等

## 介绍
### 一、视频刮削整理
支持硬链接、软链接、复制、移动、原地整理等多种整理方式

1. 识别影片ID
2. 抓取元数据
3. 创建目标目录
4. 下载、处理图片
5. 整理视频以及自带的字幕文件到目标目录
6. 生成nfo文件

### 二、目录监控
> [!IMPORTANT]
> **注意：监控开启后不会刮削目录内的存量视频，只会对新文件的创建做出响应**

提供两种监控模式针对不同场景：
1. 性能模式：监听系统级文件变更事件，即时响应，性能最好。但不支持网盘、NAS等挂载到本地的目录
2. 兼容模式：定时检查文件更新，性能一般，响应有一定延迟。优点是不依赖系统事件，兼容性强
   
当你的媒体目录是本地盘时，优先使用性能模式来获得最佳体验。其他情况或遇到目录监控不工作，请改成兼容模式

### 三、演员刮削
1. 目前支持emby服务端刮削
2. 刮削信息源：维基百科
3. 刮削图片源：[graphis](graphis.ne.jp)，[gfriends](https://github.com/gfriends/gfriends)；其中graphis源的图片质量优秀并且有背景图，作为首选
4. graphis网站有演员早期到最近时期的照片，目前默认刮削早期照片（比较青春..)，后续版本更新会添加为可选项
5. gfriends是社区维护的演员头像数据库，质量参差不齐，但覆盖范围宽，能够作为graphis很好的补充。该数据在后端初始化时会进行缓存，重启服务会检查数据更新。后续会添加定时检查功能

## 安装
镜像版本可以去本项目[dockerhub页面](https://hub.docker.com/r/mdcng/mdc)查看

容器默认PUID, PGID为1000，刮削遇到`Permission denied`等权限问题按需调整

### docker-compose（推荐）
```yaml
version: "2.1"
services:
  mdc:
    image: mdcng/mdc:latest
    container_name: mdc
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Shanghai
    volumes:
      - /path/to/data:/config # 配置目录，必须
      - /path/to/media:/media # 媒体库，可映射多个
    ports:
      - 9208:9208
    restart: unless-stopped
```
### docker 命令行
```bash
docker run -d \
  --name=mdc \
  -p 9208:9208 \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Asia/Shanghai \
  -v /path/to/data:/config \
  -v /path/to/media:/media  \
  --restart unless-stopped \
  mdcng/mdc:latest
```

## 支持一下
你可以送我一杯咖啡，以表示对这个项目的支持😉

<img src="https://user-images.githubusercontent.com/124132602/222636597-f8d48940-a528-41e8-9362-8d15f7517bf6.png" width="300" />
