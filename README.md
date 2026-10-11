# code-motion-video

用 AI 写**确定性代码**做动效视频：HTML / Canvas 按第几秒画出画面，再一帧一帧截图，用 FFmpeg 拼成 MP4。

适合：产品功能演示、动态排版 showreel、横屏 / 竖屏 / 多语言版本。

不适合：概念隐喻 B-roll（那种请用 [space-video-broll](https://github.com/SpaceZephyr/creator-buddy) 一类流程）。

方法参考开源 Skill [xilo-opus-video](https://github.com/Kianzzz/xilo-opus-video)（MIT）。

---

## 它解决什么问题

产品要出一条动效演示片时，常见两条路：

1. 用视频生成模型硬生成——画面不好控，改一处往往整条重来。
2. 找外包做——慢，版本多（竖屏、英文）成本高。

第三条路：让模型写代码。`window.render(t)` 只由时间 `t` 决定画面，同一秒永远是同一帧。改文案、改配色、出竖屏、出英文，改完重新渲即可。

---

## 快速开始

1. 把本仓库放到你的 Agent / Cursor skill 目录（例如 `workflows/code-motion-video/`）。
2. 确保目录里有 `SKILL.md`（本仓库根目录即是）。
3. 对 Agent 说：「按 code-motion-video 做一条产品动效片」，并提供：
   - 产品主张 / 功能清单
   - Logo 原文件
   - 品牌色（或让它从 Logo 取色）
   - 设备图、界面截图（如有）
   - 时长、画幅、语言

Agent 会按 skill 里的步骤跑：确认事实 → 脚本分镜 → 关键帧 → 整片 → 音效踩点 → 自评再改。

---

## 核心约定

| 约定 | 说明 |
|------|------|
| `window.render(t)` | 第 t 秒画面只由 t 决定，不用 CSS 动画、定时器、真随机 |
| 先关键帧再整片 | 不要一上来渲 50 秒 |
| Logo 用原文件 | 默认不重画、不改色、不裁切、不拆零件；只允许整图缩放 / 平移 / 淡入淡出 |
| 不夸大 | 未确认的卖点不写；数据条只统计画面里发生的事 |
| 竖屏重排 | 禁止从横屏直接裁；注意平台安全区 |
| 交片前自检 | ffprobe、黑帧、乱码、Logo 对比图、响度约 -14 LUFS |

完整步骤、画面手法、质检清单见 [`SKILL.md`](./SKILL.md)。

---

## 推荐目录结构

```text
vN/
  plan.md 或 script.md
  src/
    index.html      # window.render(t)
    timeline.js     # 拍点 cue（画面和音效共用）
    music.py        # 配乐 + 音效（可用代码合成）
    render.py       # Playwright 截帧 + FFmpeg
    assets/         # logo、设备图（按字节拷贝原文件）
  frames/  hd/  qa/
  *.mp4
  contact_sheet_2fps.png
```

---

## 和 space-video-broll 的分工

| 需求 | 用 |
|------|----|
| 产品 UI + 文字动效 + 设备图 + 多语言 | **本 skill** |
| 概念隐喻、暗色 SaaS 魔术感 B-roll | space-video-broll |
| 口播稿、字幕、配音 | space-video-script / subtitle / audio |

---

## 许可

本仓库内容按 MIT 许可发布，见 [LICENSE](./LICENSE)。

参考实现 [xilo-opus-video](https://github.com/Kianzzz/xilo-opus-video) 亦为 MIT；使用时请保留其版权声明。

---

## 作者说明

流程在真实产品动效片制作中跑通后整理成 skill，便于以后复用和分享。欢迎 Issue / PR。
