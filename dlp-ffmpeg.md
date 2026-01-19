# YouTube 音频下载与分割完整流程

> 目的：
> 从 YouTube 下载播客/课程音频，并分割成小于 70MB 的 MP3 文件，方便上传到「每日英语听力」。

---

## 一、适用场景

* YouTube 上的英语播客、访谈、课程
* 音频文件过大（>70MB）
* 需要分段学习、反复听

---

## 二、环境说明

* 系统：macOS
* 终端：zsh（macOS 默认）
* 工具：

  * `yt-dlp`（下载 YouTube 音频）
  * `ffmpeg`（无损切割音频）

---

## 三、安装工具（只需一次）

### 1. 使用 Homebrew 安装

```bash
brew install yt-dlp ffmpeg
```

> 如果没有 Homebrew，需要先安装 Homebrew。

---

## 四、从 YouTube 下载音频（MP3）

### 1. 基本命令（⚠️ URL 必须加引号）

```bash
yt-dlp -x --audio-format mp3 "https://www.youtube.com/watch?v=VIDEO_ID"
```

### 2. 为什么必须加引号？

* zsh 会把 `?` 当作通配符
* 不加引号会出现：

```text
zsh: no matches found
```

---

### 3. 成功标志

终端出现类似内容：

```text
[download] 100% of   64.04MiB
[ExtractAudio] ... .mp3
```

说明：

* 音频已成功下载
* 已转换为 MP3

---

## 五、查看下载后的音频信息

```bash
ls
```

示例文件名（通常很长，包含空格和符号）：

```text
How Trump’s Greenland Tariffs Could Reshape U.S.–Europe Relations in 2026 ｜ English Podcast [aOxuje1l7zA].mp3
```

> ⚠️ 后续所有操作都必须给文件名加引号

---

## 六、使用 ffmpeg 分割音频（无损）

### 1. 音频基本信息示例

```text
Duration: 01:18:04.73
bitrate: 79 kb/s
```

说明：

* 总时长：78 分钟
* 码率较低（播客音质），非常适合听力学习

---

### 2. 分割为两段（推荐做法）

#### 第一段：0–30 分钟

```bash
ffmpeg -i "How Trump’s Greenland Tariffs Could Reshape U.S.–Europe Relations in 2026 ｜ English Podcast [aOxuje1l7zA].mp3" \
-ss 00:00:00 -to 00:30:00 -c copy part1.mp3
```

#### 第二段：30 分钟到结尾（无重叠）

```bash
ffmpeg -i "How Trump’s Greenland Tariffs Could Reshape U.S.–Europe Relations in 2026 ｜ English Podcast [aOxuje1l7zA].mp3" \
-ss 00:30:00 -c copy part2.mp3
```

---

### 3. 参数说明

* `-ss`：开始时间
* `-to`：结束时间
* `-c copy`：不重新编码（速度快、音质不变）

---

## 七、检查分割结果

```bash
ls -lh part*.mp3
```

示例结果：

* `part1.mp3`：30 分钟 / ~17MB
* `part2.mp3`：49 分钟 / ~28MB

✅ 都小于 70MB，可直接上传

---

## 八、推荐的文件命名方式（强烈建议）

```text
Trump_Greenland_Podcast_Part1_0-30min.mp3
Trump_Greenland_Podcast_Part2_30-78min.mp3
```

好处：

* 一眼知道内容
* 方便长期积累
* 不容易混乱

---

## 九、常见问题与坑

### 1. 文件名有空格怎么办？

* 必须使用英文双引号：

```bash
"filename with space.mp3"
```

---

### 2. 出现 WARNING 要不要管？

```text
WARNING: YouTube is forcing SABR streaming
```

* ❌ 不需要处理
* ✅ 不影响下载和音质

---

### 3. 是否需要重新压缩？

* 听力学习：**不需要**
* 当前 79kbps 已非常合适

---

## 十、完整流程总结（可快速回顾）

```text
1. yt-dlp 下载 YouTube 音频（MP3）
2. 确认文件大小和时长
3. ffmpeg 无损切割音频
4. 重命名文件
5. 上传到每日英语听力
```

---

## 十一、适用人群备注

* 英语初学 / 中级听力训练
* 希望建立个人技巧库
* 希望“学一次，用很多次”

---

> 本文档适合长期保存于 GitHub Pages / Markdown 笔记中，作为可复用学习流程。

---

# 十二、个人技巧网站整体结构设计（推荐）

## 目录结构示例（GitHub Pages / Markdown）

```text
my-skills/
├── index.md                 # 首页
├── README.md
├── english/
│   ├── listening.md         # 听力方法
│   └── resources.md         # 学习资源
├── terminal/
│   ├── yt-dlp.md            # 下载音频
│   └── ffmpeg.md            # 音频处理
├── mac/
│   └── tips.md              # macOS 使用技巧
└── templates/
    └── skill-template.md    # 技巧模板
```

---

# 十三、首页 index.md 示例（可直接用）

```md
# 我的个人技巧库

这是一个用于**长期复用**的个人学习技巧网站。

## 📚 分类导航
- [英语学习](english/listening.md)
- [终端技巧](terminal/yt-dlp.md)
- [Mac 使用](mac/tips.md)

## ⭐ 最近更新
- yt-dlp 下载 YouTube 音频
- ffmpeg 无损切割 MP3
```

---

# 十四、技巧 Markdown 通用模板（强烈推荐）

> 新学到任何技巧，复制本模板即可

````md
# 技巧标题

## 使用场景

简要说明：什么时候用？解决什么问题？

## 操作步骤

### 第 1 步

```bash
命令或操作
````

### 第 2 步

```bash
命令或操作
```

## 注意事项 / 常见坑

* 坑 1
* 坑 2

## 结果说明

执行成功后会发生什么。

## 一句话总结

下次我只要记住这一句话即可。

````

---

# 十五、导航与链接的基本原则（新手必看）

- 所有链接使用**相对路径**
- 文件名只用：英文 + 数字 + `-`
- 一个技巧 = 一个 `.md` 文件

示例：

```md
[ffmpeg 切音频](terminal/ffmpeg.md)
````

---

# 十六、日常使用流程（真正“能坚持”的关键）

```text
1. 今天学到一个新技巧
2. 打开 GitHub → 新建 .md 文件
3. 套用【技巧模板】填写
4. Commit 保存
5. 网站自动更新
```

---

# 十七、给未来自己的建议（非常重要）

* ❌ 不追求好看
* ❌ 不折腾技术
* ✅ 只记录「下次能直接用的东西」

> 这个网站不是给别人看的，是给未来的自己用的。

---

# 十八、下一阶段可选升级（等你内容多了再做）

* 站内搜索（MkDocs / VitePress）
* 标签系统（#英语 #终端）
* 自动目录（Table of Contents）

现在**完全可以不用管**。

---

📌 本文档 + 目录结构 + 模板 = 一个完整可长期使用的个人技巧网站方案。
