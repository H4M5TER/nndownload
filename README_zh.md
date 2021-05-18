# nndownload - Niconico 下载器

![PyPI](https://img.shields.io/pypi/v/nndownload.svg)

nndownload 能帮助你下载 [Niconico](http://nicovideo.jp) 上的视频、图片、漫画，以及处理其他来自 Niconico 的链接。
Unlike other existing downloaders, this program communicates wth DMC (Dwango Media Cluster) servers to ensure access to high quality videos.

## 声明

- 在非登录状态下 (使用 -g 或 --no-login 选项)，一些视频可能无法下载，或只能下载较低画质。
- 如果你没有 Niconico 的付费会员 ([premium account](https://secure.nicovideo.jp/secure/premium_detail/))，在 Niconico 的经济模式(限流)时段 (没有另行通知的情况下，时段为日本标准时间中午 12 点到凌晨 3 点) 或其他流量高峰期，你可能会下载到较低画质的视频。
- Running multiple download sessions on the same connection may lead to temporary blocks or throttling.
- 暂时不支持的功能:
  - Downloading Niconama timeshifts
  - 下载 Seiga 缩略图和评论
  - 下载频道博客的评论

## 特性

- 下载视频，以及评论和封面等元数据。
- 下载 Seiga 插图和漫画，以及元数据。
- 下载频道的视频和博客，以及元数据。
- 下载整个 mylist
- 针对用户下载视频、mylist、插图和漫画
- 给 Nico 生放送生成直播流链接
- 使用多线程优化视频下载速度
- 从文件读取下载目标链接列表

## 安装

### 下载可执行文件

在 [Release 页](https://github.com/AlexAplin/nndownload/releases)可以下载Windows 系统的可执行文件

### 作为 Python 模块安装

```bash
pip install nndownload
```

## 使用

### 使用命令行

```
usage: nndownload.py [options] input

positional arguments:
  input                 链接或文件

可选参数:
  -h, --help            显示帮助信息并退出
  -u MAIL/TEL, --username MAIL/TEL
                        账户邮箱地址或手机号
  -p PASSWORD, --password PASSWORD
                        账户密码
  --session-cookie COOKIE
                        登录态 cookie
  -n, --netrc           使用 .netrc 鉴权
  -q, --quiet           静默模式 不在命令行显示日志
  -l, --log             输出日志到文件
  -v, --version         显示版本号并退出

下载选项:
  -y PROXY, --proxy PROXY
                        HTTP 或 SOCKS 协议的代理地址
  -o TEMPLATE, --output-path TEMPLATE
                        自定义输出路径 (见下)
  -r N, --threads N     使用指定的线程数下载
  -g, --no-login        使用非登录态下载
  -f, --force-high-quality
                        只在检测到最高画质可下载的情况下下载
  -a, --add-metadata    给视频文件添加元数据 (仅限 MP4)
  -m, --dump-metadata   导出元数据到文件
  -t, --download-thumbnail
                        下载视频封面
  -c, --download-comments
                        下载视频评论
  -e, --english         在英语站请求视频
  -aq AUDIO_QUALITY, --audio-quality AUDIO_QUALITY
                        指定音频质量
  -vq VIDEO_QUALITY, --video-quality VIDEO_QUALITY
                        指定视频质量
  -s, --skip-media      skip downloading media
  --playlist-start N    指定在 playlist 中开始下载的序号 (从 0 开始)
```

### 使用模块

```python
import nndownload

url = "https://www.nicovideo.jp/watch/sm35249846"
output_path = "/tmp/{id}.{ext}"
nndownload.execute("-g", "-o", output_path, url)
```

### 自定义输出路径

自定义输出路径的格式像标准 Python 模板字符串一样，例如 `{uploader - {title}.{ext}`。对于 Seiga 漫画，每个章节会被输出到对应的输出路径，例如 `{manga_id}\{id} - {title}`

花括号里可选的选项如下:

- comment_count (videos, images, manga, articles)
- description (videos, images, manga)
- document_url (videos, images, manga, articles)
- ext (videos, images, articles)
- id (videos, images, manga, articles)
- published (videos, images, manga, articles)
- tags (videos, images, manga, articles)
- title (videos, images, manga, articles)
- uploader (videos, images, manga, articles)
- uploader_id (videos, images, manga, articles)
- url (videos, images)
- view_count (videos, images, manga)
- audio_quality (videos)
- video_quality (videos)
- article (articles)
- blog_title (articles)
- clip_count (images)
- duration (videos)
- manga_id (manga)
- manga_title (manga)
- mylist_count (videos)
- page_count (manga)
- size_high (videos)
- size_low (videos)
- thread_id (videos)
- thumbnail_url (videos)

### 使用直播流链接

After generating a stream URL, the program must be kept running to keep the stream active. [mpv](https://github.com/mpv-player/mpv) and [streamlink](https://github.com/streamlink/streamlink) are the best options for playing generated stream URLs. Other programs that use aggressive HLS caching and threading may also work.

`mpv https://...`
`streamlink https://... best`

## 协议

这个项目使用 MIT 协议开源。
