# 配图生成标准流程

> 配图未完成 = 草稿不完整，不得提交审核

---

## 一、平台配图规范一览


| 平台    | 封面图            | 内容配图                        | 数量要求          | 比例                              |
| ----- | -------------- | --------------------------- | ------------- | ------------------------------- |
| 微信公众号 | `00-cover.png` | `01-xxx.png` / `02-xxx.png` | 封面1张 + 内容2-4张 | 封面 2.35:1 / 内容 16:9 或 4:3 或 1:1 |
| 小红书   | `00-cover.png` | `01-xxx.png`                | 封面1张 + 内容0-1张 | 封面 3:4 / 内容 3:4 或 1:1           |
| 独立博客  | `00-cover.png` | `01-xxx.png` / `02-xxx.png` | 封面1张 + 内容1-4张 | 封面 16:9 / 内容 16:9               |


---

## 二、配图类型说明


| 类型  | 用途               | 命名                   | 适用场景      |
| --- | ---------------- | -------------------- | --------- |
| 封面图 | 文章开头，吸引力担当       | `00-cover.png`       | 每篇必选      |
| 框架图 | 可视化核心观点、结构       | `01-framework.png`   | 核心论点较多的文章 |
| 流程图 | 步骤、路径、闭环         | `01-flowchart.png`   | 方法论、操作类文章 |
| 对比图 | Before/After、优缺点 | `02-comparison.png`  | 评测、复盘类文章  |
| 时间线 | 历史、演进、发展         | `01-timeline.png`    | 热点事件梳理    |
| 信息图 | 数据、指标、榜单         | `01-infographic.png` | 数据驱动类文章   |
| 场景图 | 真实场景、人像代入        | `01-scene.png`       | 小红书、生活方式类 |


---

## 三、生图流程（baoyu-article-illustrator Skill）

使用 [baoyu-article-illustrator Skill](https://github.com/JimLiu/baoyu-skills#baoyu-article-illustrator) 生成配图提示词。**提示词文件是复现记录，必须在调用生图后端前生成。**

### Step 1：配置检查（EXTEND.md）

按优先级查找 EXTEND.md，找到则读取偏好设置，未找到则使用默认值：


| 优先级 | 路径                                                                                   |
| --- | ------------------------------------------------------------------------------------ |
| 1   | `.baoyu-skills/baoyu-article-illustrator/EXTEND.md`                                  |
| 2   | `${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-article-illustrator/EXTEND.md` |
| 3   | `$HOME/.baoyu-skills/baoyu-article-illustrator/EXTEND.md`                            |


常见偏好字段：`preferred_image_backend`、`preferred_style`、`preferred_palette`、`default_output_dir`、`language`。

### Step 2：分析文章内容

分析结果填入下表：


| 分析项  | 输出                                             |
| ---- | ---------------------------------------------- |
| 内容类型 | Technical / Tutorial / Methodology / Narrative |
| 目的   | Information / Visualization / Imagination      |
| 核心论点 | 2-5个主要观点                                       |
| 配图位置 | 需要视觉辅助的具体段落                                    |


**核心原则**：将隐喻可视化，而非字面图示。配图服务内容，不是内容的图解。

### Step 3：确认设置（AskUserQuestion）

⚠️ **必须确认后才能进入 Step 4。** 用户说"直接生成/跳过确认"除外。

一次最多4问，Q1、Q2 必问：


| 问题          | 选项                                                                                       |
| ----------- | ---------------------------------------------------------------------------------------- |
| **Q1：配图类型** | 推荐预设 / 或手动：infographic, scene, flowchart, comparison, framework, timeline, mixed         |
| **Q2：密度**   | minimal（封面+1-2张）、balanced（3-5张）、per-section、rich（6+张）                                    |
| **Q3：风格**   | 推荐风格 / minimal-flat / sci-fi / hand-drawn / editorial / scene / poster / Other — 预设已选则跳过 |
| **Q4：色板**   | 默认（风格色）/ macaron / warm / neon — 预设或 preferred_palette 已设置则跳过                            |
| Q5：语言       | 文章语言 ≠ EXTEND.md 设置时填写                                                                   |


### Step 4：生成大纲（outline.md）

在文章同级目录创建 `outline.md`：

```yaml
---
type: mixed
density: minimal
style: warm
palette: warm
image_count: 3
---

## Illustration 1
**Position**: [插入位置]
**Purpose**: [为什么需要这张图]
**Visual Content**: [视觉内容描述]
**Filename**: 01-timeline-journey.png
```

### Step 5：生成提示词文件（prompts/）

**⚠️ 硬性要求：调用任何生图后端前，必须先保存提示词文件。**

在 `{文章目录}/prompts/` 下为每张配图创建文件，命名格式：`NN-{type}-{slug}.md`。

**提示词文件结构（YAML frontmatter + 内容）：**

```yaml
---
illustration_id: 01
type: timeline
style: warm
palette: warm
references:                    # 仅当 references/ 目录下存在参考图时填写
  - ref_id: 01
    filename: 01-ref-diagram.png
    usage: direct              # direct | style | palette
---

[标题] - Chronological View

DIRECTION: [horizontal/vertical]

EVENTS:
- [时间节点1]：[具体事件，中文标签]
- [时间节点2]：[具体事件，中文标签]

MARKERS: [每个节点上方的图标，中文描述]
STYLE: [风格特征描述，参考 styles.md]
ASPECT: 16:9
```

**风格参考**（`references/styles.md`）：


| 风格                    | 描述                | 适合内容       |
| --------------------- | ----------------- | ---------- |
| `sketch-notes`        | 暖色奶油纸背景，手绘线条，粉彩色块 | 教育、知识、通用   |
| `vector-illustration` | 扁平矢量，清晰几何形状       | 知识、教程、科技   |
| `warm`                | 暖色系，友好亲切，人文感      | 个人成长、叙事、情感 |
| `elegant`             | 精致考究，商务感          | 商业、思想领导力   |
| `screen-print`        | 粗体剪影，丝网印刷质感       | 观点、社论、电影感  |
| `blueprint`           | 技术蓝图，网格布局         | AI、系统设计、工程 |


**色板参考**（`references/palettes/`）：


| 色板         | 描述              | 适合内容       |
| ---------- | --------------- | ---------- |
| `warm`     | 暖色调，无冷色（橙、陶土、金） | 品牌、个人成长、生活 |
| `macaron`  | 柔和粉彩（蓝绿薰衣草蜜桃）   | 教育、知识、教程   |
| `neon`     | 霓虹色（粉、青、黄）      | 游戏、复古、流行   |
| `mono-ink` | 黑白墨水 + 稀疏语义点缀   | 专业视觉笔记、宣言  |


**类型 × 风格推荐矩阵：**


|             | sketch-notes | vector | warm | elegant | screen-print |
| ----------- | ------------ | ------ | ---- | ------- | ------------ |
| infographic | ✓✓           | ✓✓     | ✓    | ✓✓      | ✓            |
| scene       | ✗            | ✓      | ✓✓   | ✓       | ✓✓           |
| flowchart   | ✓✓           | ✓✓     | ✓    | ✓       | ✗            |
| comparison  | ✓✓           | ✓✓     | ✓    | ✓✓      | ✓            |
| framework   | ✓✓           | ✓✓     | ✓    | ✓✓      | ✓            |
| timeline    | ✓            | ✓      | ✓    | ✓✓      | ✓            |


**提示词编写原则：**

1. **布局优先**：先描述构图、区域、流动方向
2. **具体数据**：使用文章中的实际数字、术语、引用
3. **视觉关系**：元素之间如何连接
4. **语义色**：按含义选色（红=警示，绿=高效）
5. **风格特征**：线条处理、纹理、情绪
6. **比例结尾**：以比例和复杂度结尾

**所有提示词必须包含：**

- 干净的构图，充足留白
- 主元素居中或按内容需求定位
- 中文标签，手写风格字体
- 简化人物剪影（非写实）
- 色值仅为渲染指导，不显示为可见文字

### Step 6：执行生图

根据 `preferred_image_backend` 或自动选择后端，执行生图：

```bash
~/.bun/bin/bun ~/.baoyu-skills/skills/baoyu-imagine/scripts/main.ts \
  --batchfile "04-草稿/YYYY-MM-DD/[平台]/[文章标题]/batch.json" \
  --jobs 4 \
  --provider openai --model gpt-image-2
```

**batch.json 格式（JSON 数组，内联 prompt）：**

```json
[
  {
    "prompt": "[从 prompts/NN-xxx.md 提取的完整提示词]",
    "image": "00-cover.png",
    "ar": "2.35:1",
    "model": "gpt-image-2"
  }
]
```

**生图后端选择顺序：**

1. 用户明确指定 → 使用指定后端
2. EXTEND.md 有 `preferred_image_backend` → 使用偏好
3. 自动选择：运行时原生工具 > 唯一已安装后端 > 多后端时询问用户

### Step 7：插入文章

在 Markdown 中使用标准语法插入配图：

```markdown
![封面](00-cover.png)

![时间线](01-timeline-journey.png)
```

路径相对于文章文件，按 `default_output_dir` 计算（`imgs-subdir` / `same-dir` / `illustrations-subdir` / `independent`）。

---

## 四、封面图风格规范


| 平台    | 比例        | 推荐风格           | 关键词                        |
| ----- | --------- | -------------- | -------------------------- |
| 微信公众号 | 2.35:1 横版 | warm/elegant   | 专业感、商务风、现代简约、暖色调、扁平插画，封面简洁 |
| 小红书   | 3:4 竖版    | warm           | 生活感、真实感、暖色调、人像代入、情绪共鸣，封面简洁 |
| 独立博客  | 16:9 横版   | elegant/vector | 专业信息图、清晰易读、数据图表            |


---

## 五、目录结构规范

每篇文章独立文件夹，配图与文章同级：

```
04-草稿/YYYY-MM-DD/[平台]/[文章标题]/
├── [文章标题].md
├── [文章标题]-v2.md        # 审核修改版本
├── outline.md              # 配图大纲（Skill 生成）
├── 00-cover.png           # 封面图
├── 01-xxx.png             # 内容配图
├── 02-xxx.png             # 内容配图
└── prompts/                # 提示词文件（Skill 生成）
    ├── 00-cover.md
    ├── 01-timeline.md
    └── 02-scene.md
```

---

## 六、常见错误


| 错误            | 说明                         |
| ------------- | -------------------------- |
| 多篇文章共用一个配图目录  | 禁止，必须每篇独立文件夹               |
| 配图放在文章外部的共享目录 | 禁止，配图必须在文章同级目录             |
| 封面图用了太多文字     | 封面简洁为主，文字越少越好              |
| 生成了配图但没有插入文章  | 配图必须用 `![]()` 语法插入正文       |
| 没有生成封面图就提交审核  | 禁止，封面图是必选项                 |
| 提示词文件命名不规范    | 应为 `prompts/00-cover.md` 等 |
| 未保存提示词文件就生图   | 禁止，提示词文件是复现记录，必须先保存        |


---

## 七、提交审核前确认清单

- 封面图 `00-cover.png` 已生成
- 封面图比例符合平台要求
- 封面图已用 `![]()` 插入文章
- 内容配图已生成（如需要）
- 内容配图已插入正文对应位置
- 配图风格符合平台规范
- 配图与文章在同一文件夹
- `prompts/` 目录下提示词文件已保存
- 无共用配图目录问题

---

## 八、Skill 参考资料


| 文件                                                          | 内容           |
| ----------------------------------------------------------- | ------------ |
| `~/.baoyu-skills/skills/baoyu-article-illustrator/SKILL.md` | 完整 Skill 文档  |
| `references/workflow.md`                                    | 详细操作流程       |
| `references/prompt-construction.md`                         | 提示词模板与规范     |
| `references/styles.md`                                      | 风格图鉴与类型×风格矩阵 |
| `references/palettes/warm.md`                               | warm 色板规范    |
| `references/palettes/macaron.md`                            | macaron 色板规范 |
| `references/config/first-time-setup.md`                     | 首次配置指南       |


---

*配图SOP | Holamuse AI运营内容引擎 | 基于 baoyu-article-illustrator v1.58*