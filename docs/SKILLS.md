# 所需 Cursor Agent Skills

> 本系统依赖 Cursor Agent 的 Skills 扩展能力。以下列出所有已集成的 Skills 及安装方式。

---

## 系统必需 Skills

### 1. baoyu-article-illustrator — 文章配图生成


| 属性        | 说明                                                            |
| --------- | ------------------------------------------------------------- |
| **触发词**   | "帮我生成配图"、"为文章配图"、"生成插图"                                       |
| **功能**    | 分析文章结构 → 生成配图提示词 → 调用 gpt-image-2 出图                          |
| **本系统用途** | SOP-06B 配图生成的核心工具                                             |
| **是否预装**  | ✅ 是（已安装于 `~/.baoyu-skills/skills/baoyu-article-illustrator/`） |


**核心功能：**

- 智能分析文章内容，识别需要视觉辅助的位置
- Type × Style × Palette 三维度生成配图
- 支持 infographic / scene / flowchart / comparison / framework / timeline 等类型
- 生成可复现的提示词文件，便于迭代调整

### 2. baoyu-url-to-markdown — 网页内容抓取


| 属性        | 说明                                                        |
| --------- | --------------------------------------------------------- |
| **触发词**   | "帮我抓取这个网页"、"把网页转成 markdown"                               |
| **功能**    | 将任意网页转换为结构化 Markdown，保留原文格式                               |
| **本系统用途** | SOP-03 热点抓取 的素材采集环节                                       |
| **是否预装**  | ✅ 是（已安装于 `~/.baoyu-skills/skills/baoyu-url-to-markdown/`） |


**核心功能：**

- 支持主流媒体网站（数英网、虎嗅、36氪等）
- 保留原文段落结构和关键数据
- 自动提取标题、来源、发布时间

---

## 选装 Skills（按需安装）

以下 Skills **不在系统预装范围内**，如需特定功能可手动安装。

### 3. baoyu-post-to-wechat — 微信公众号发布


| 属性        | 说明                       |
| --------- | ------------------------ |
| **功能**    | 将 Markdown 文章转换为公众号格式并发布 |
| **本系统用途** | 如需扩展至自动化发布环节             |
| **安装方式**  | 见下方「如何安装新 Skill」         |


### 4. baoyu-youtube-transcript — YouTube 字幕抓取


| 属性        | 说明                    |
| --------- | --------------------- |
| **功能**    | 抓取 YouTube 视频字幕用于素材采集 |
| **本系统用途** | 热点素材的多媒体来源            |
| **安装方式**  | 见下方「如何安装新 Skill」      |


### 5. baoyu-slide-deck — PPT 幻灯片生成


| 属性        | 说明                |
| --------- | ----------------- |
| **触发词**   | "做一份 PPT"、"生成幻灯片" |
| **功能**    | 生成演示文稿，支持多种视觉风格   |
| **本系统用途** | 如需为内容营销生成演示文稿     |
| **安装方式**  | 见下方「如何安装新 Skill」  |


### 6. find-skills — 技能发现工具


| 属性      | 说明                                          |
| ------- | ------------------------------------------- |
| **触发词** | "有这个 Skill 吗"、"帮我找一个 Skill"、"how do I do X" |
| **功能**  | 搜索并安装 Agent Skills                          |
| **用途**  | 发现和安装更多能力扩展                                 |


---

## 如何安装新 Skill

### 方式一：通过 find-skills 自动安装

```
有这个 Skill 可以帮我做 PPT 吗？
```

→ AI 调用 `find-skills` Skill，搜索并引导安装

### 方式二：手动安装

1. 找到目标 Skill 的 GitHub 仓库（如 `https://github.com/JimLiu/baoyu-skills`）
2. 克隆到本地：

```bash
git clone https://github.com/JimLiu/baoyu-skills.git ~/.baoyu-skills
```

1. 或在 Cursor 中执行：

```
安装 Skill：[SKILL.md 的完整路径]
```

---

## Skill 目录结构

每个 baoyu Skill 包含：

```
~/.baoyu-skills/skills/[skill-name]/
└── SKILL.md          # Skill 主文件（含触发词 + 执行逻辑）
```

### 查看已安装 Skills

```
~/.baoyu-skills/skills/
├── baoyu-article-illustrator/   # 文章配图生成 ⭐必需
├── baoyu-url-to-markdown/      # 网页内容抓取 ⭐必需
├── baoyu-post-to-wechat/       # 微信公众号发布
├── baoyu-youtube-transcript/   # YouTube 字幕抓取
├── baoyu-slide-deck/           # PPT 幻灯片生成
├── baoyu-markdown-to-html/     # Markdown 转 HTML
├── baoyu-imagine/             # 生图后端
└── ...（更多 baoyu skills）
```

---

## 系统必需 Skills 确认


| Skill                     | 必需   | 用途        | 状态    |
| ------------------------- | ---- | --------- | ----- |
| baoyu-article-illustrator | ✅ 必需 | 文章配图生成    | ✅ 已安装 |
| baoyu-url-to-markdown     | ✅ 必需 | 网页内容抓取    | ✅ 已安装 |
| find-skills               | ⬜ 推荐 | 发现新 Skill | ✅ 已安装 |
| baoyu-post-to-wechat      | ⬜ 可选 | 公众号发布     | ⬜ 未安装 |
| baoyu-youtube-transcript  | ⬜ 可选 | 视频素材抓取    | ⬜ 未安装 |
| baoyu-slide-deck          | ⬜ 可选 | PPT 生成    | ⬜ 未安装 |


---

## baoyu Skills 完整列表


| Skill                     | 用途                |
| ------------------------- | ----------------- |
| baoyu-article-illustrator | 文章配图生成            |
| baoyu-url-to-markdown     | 网页转 Markdown      |
| baoyu-markdown-to-html    | Markdown 转 HTML   |
| baoyu-post-to-wechat      | 微信公众号发布           |
| baoyu-post-to-weibo       | 微博发布              |
| baoyu-post-to-x           | X/Twitter 发布      |
| baoyu-youtube-transcript  | YouTube 字幕抓取      |
| baoyu-slide-deck          | PPT 幻灯片生成         |
| baoyu-imagine             | 生图后端（gpt-image-2） |
| baoyu-infographic         | 信息图生成             |
| baoyu-cover-image         | 封面图生成             |
| baoyu-diagram             | 流程图/架构图生成         |
| baoyu-image-cards         | 图片卡片生成            |
| baoyu-comic               | 漫画生成              |
| baoyu-translate           | 翻译                |
| baoyu-format-markdown     | Markdown 格式化      |
| baoyu-compress-image      | 图片压缩              |


完整列表及文档：[https://github.com/JimLiu/baoyu-skills](https://github.com/JimLiu/baoyu-skills)

---

*最后更新：2026-05-12*