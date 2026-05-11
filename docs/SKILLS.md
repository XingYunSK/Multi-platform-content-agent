# 所需 Cursor Agent Skills

> 本系统依赖 Cursor Agent 的 Skills 扩展能力。以下列出所有已集成的 Skills 及安装方式。
>
> **已集成的 Skills 由系统预装，无需手动安装。** 以下仅作说明，如需添加新 Skill 请参考本指南。

---

## 已集成 Skills

### 1. aihot — AI 热点资讯查询

| 属性 | 说明 |
|------|------|
| **触发词** | "今天 AI 圈有什么"、"AI 日报"、"AI HOT"、"AI 资讯"、"AI 热点" |
| **功能** | 抓取 aihot.virxact.com 公开 API，整理为中文简报 |
| **本系统用途** | 热点抓取（SOP-03）的核心数据源之一 |
| **是否预装** | ✅ 是（已安装于 `~/.claude/skills/aihot/`） |

### 2. xiaohongshu — 小红书内容工具

| 属性 | 说明 |
|------|------|
| **触发词** | "小红书上有哪些关于 XX 的讨论"、"分析小红书舆情"、"小红书 XX 话题报告" |
| **功能** | 搜索内容、获取帖子详情、评论分析、用户主页 |
| **本系统用途** | 热点抓取（SOP-03）+ 选题库舆情分析（SOP-05） |
| **是否预装** | ✅ 是（已安装于 `~/.claude/skills/xiaohongshu/`） |

---

## 选装 Skills（按需安装）

以下 Skills **不在系统预装范围内**，如需特定功能可手动安装。

### 3. x-mastery-mentor — X/Twitter 运营导师

| 属性 | 说明 |
|------|------|
| **触发词** | "怎么写推文"、"怎么涨粉"、"X 策略"、"推特选题" |
| **功能** | 选题-写作-增长操作手册，AI/科技赛道专精 |
| **本系统用途** | 如需扩展至 X/Twitter 平台内容分发 |
| **安装方式** | 见下方「如何安装新 Skill」 |

### 4. gpt-image2-ppt-skills — PPT 幻灯片生成

| 属性 | 说明 |
|------|------|
| **触发词** | "做一份 PPT"、"生成幻灯片"、"用 gpt-image 生成 PPT" |
| **功能** | 10 种视觉风格 + 模板克隆模式，输出 HTML 预览和 PPTX |
| **本系统用途** | 如需为内容营销生成演示文稿 |
| **安装方式** | 见下方「如何安装新 Skill」 |

### 5. find-skills — 技能发现工具

| 属性 | 说明 |
|------|------|
| **触发词** | "有这个 Skill 吗"、"帮我找一个 Skill"、"how do I do X" |
| **功能** | 搜索并安装 Agent Skills |
| **用途** | 发现和安装更多能力扩展 |

---

## 如何安装新 Skill

### 方式一：通过 find-skills 自动安装

```
有这个 Skill 可以帮我写 X/Twitter 推文吗？
```

→ AI 调用 `find-skills` Skill，搜索并引导安装

### 方式二：手动安装

1. 找到目标 Skill 的 `SKILL.md` 文件路径
2. 在 Cursor 中执行：

```
安装 Skill：[SKILL.md 的完整路径]
```

3. Skill 写入 `~/.claude/skills/[skill-name]/SKILL.md` 后自动生效

---

## Skill 目录结构

每个 Skill 包含：

```
~/.claude/skills/[skill-name]/
└── SKILL.md          # Skill 主文件（含触发词 + 执行逻辑）
```

### 查看已安装 Skills

```
~/.claude/skills/
├── aihot/
│   └── SKILL.md
├── xiaohongshu/
│   └── SKILL.md
├── x-mastery-mentor/
│   └── SKILL.md
├── gpt-image2-ppt-skills/
│   └── SKILL.md
└── find-skills/
    └── SKILL.md
```

---

## 系统必需 Skills 确认

| Skill | 必需 | 用途 | 状态 |
|-------|------|------|------|
| aihot | ✅ 必需 | AI 行业热点来源 | ✅ 已安装 |
| xiaohongshu | ✅ 必需 | 小红书舆情分析 | ✅ 已安装 |
| find-skills | ⬜ 推荐 | 发现新 Skill | ✅ 已安装 |
| x-mastery-mentor | ⬜ 可选 | X 平台扩展 | ⬜ 未安装 |
| gpt-image2-ppt-skills | ⬜ 可选 | PPT 生成 | ⬜ 未安装 |

---

*最后更新：2026-05-12*
