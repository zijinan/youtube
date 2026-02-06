# YouTube 去广告 + 字幕兼容 (Quantumult X)

## 问题背景

墨鱼(ddgksf2013)的 YouTube 去广告配置使用 Maasea 脚本对 `/player` 接口进行 protobuf 二进制修改，在去广告的同时会启用字幕自动翻译功能。该功能在处理 protobuf 数据时会损坏字幕元数据（`playerCaptionsTracklistRenderer`），导致：

- 字幕加载报错
- 字幕选项消失
- 与双语字幕插件（DualSub/DualSubs）冲突

## 解决方案

本配置基于 [Ender-Wang](https://github.com/Ender-Wang/YouTubeAds-PiP-BackgroundPlay) 的修改方案，关闭了 Maasea 脚本中的字幕自动翻译功能（`captionLang` 和 `lyricLang`），同时保留所有其他功能。

### 保留功能
- ✅ 视频广告移除
- ✅ 画中画 (PIP)
- ✅ 后台播放
- ✅ 瀑布流/搜索页/播放页广告移除
- ✅ 短视频/贴片广告移除
- ✅ **字幕正常工作**

### 移除功能
- ❌ 字幕自动翻译为中文（此功能导致字幕损坏）

## 安装方法

### 仅去广告（字幕正常）

在 Quantumult X 的 `[rewrite_remote]` 中添加：

```
https://raw.githubusercontent.com/zijinan/youtube/main/YoutubeAds.conf, tag=YouTube去广告(字幕兼容), update-interval=172800, opt-parser=false, enabled=true
```

### 去广告 + 双语字幕（推荐）

在 `[rewrite_remote]` 中添加以下两行，**注意顺序（去广告在上，DualSubs 在下）**：

```
https://raw.githubusercontent.com/zijinan/youtube/main/YoutubeAds.conf, tag=YouTube去广告(字幕兼容), update-interval=172800, opt-parser=false, enabled=true
https://github.com/DualSubs/YouTube/releases/latest/download/DualSubs.YouTube.qxrewrite, tag=YouTube双语字幕, update-interval=172800, opt-parser=true, enabled=true
```

> ⚠️ **重要**：去广告配置必须在 DualSubs 上方，QX 中位置靠上的重写优先级更高。

### 额外建议

如果视频广告仍然存在，在 QX 配置文件的 `[general]` 下添加：

```
udp_drop_list=443
```

## 致谢

- [ddgksf2013](https://github.com/ddgksf2013) - 原始去广告配置
- [Maasea](https://github.com/Maasea) - YouTube protobuf 处理脚本
- [Ender-Wang](https://github.com/Ender-Wang/YouTubeAds-PiP-BackgroundPlay) - 字幕修复方案
- [DualSubs](https://github.com/DualSubs/YouTube) - 双语字幕解决方案
- [VirgilClyne](https://github.com/VirgilClyne) - DualSubs 作者
