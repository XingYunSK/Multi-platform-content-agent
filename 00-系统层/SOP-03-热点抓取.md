# 热点抓取

> 多源热点采集 → 去重脱水 → 交付日报的完整流程

---

## ⚠️ 去重机制（每次抓取前必做）

### 问题
同一热点被不同来源多次报道 → 日报重复 → 选题卡重复 → 内容同质化

### 解决方案：三重去重

**第一重：时间窗口去重**
- 7天内已报道的相同话题 → 标记为"跟进"，不作为新热点
- 同一事件的后续发展 → 作为"事件进展"追加到原素材

**第二重：标题指纹去重**
- 提取标题核心词（去掉品牌名/时间词/情绪词）
- 与素材库已有标题比对，相似度>70% → 合并或跳过

**第三重：内容摘要去重**
- 对每篇新文章提取100字摘要
- 与当日已抓取文章摘要比对，重复>50% → 跳过

### 去重执行方式

```
抓取前：
1. 读取 02-热点池/日报/ 目录，检查今日是否已有相同热点
2. 读取 02-热点池/热点素材库/ 最近7日的素材，检查话题重复

抓取中：
3. 每篇文章抓取后，立即做标题指纹比对
4. 重复 → 记录"来源B补充"到原热点文件夹，跳过
5. 不重复 → 正常入库

抓取后：
6. 最终输出前，再做一次摘要去重
```

---

## 数据源矩阵（已验证可用）

### ⭐ 聚合热榜（一站式多源覆盖）

| 数据源 | URL | 覆盖范围 | 说明 |
|--------|------|----------|------|
| **今日热榜 tophub.today** | `tophub.today` | 微博/知乎/抖音/B站/微信/36kr/虎嗅等 | ⭐首选，一个页面聚合全网热榜，可按频道抓取 |

使用方式：抓取 tophub.today 页面，按频道提取各来源热榜标题和链接，再逐篇抓取正文。

### 主数据源（垂直领域深度内容）

| 类型 | 数据源 | URL | 内容特点 | 更新频率 |
|------|--------|------|----------|----------|
| **营销案例** | **数英网** | `digitaling.com/rss` | 品牌营销/广告创意/优秀项目 | 每日多次 |
| **运营增长** | **运营派** | `yunyingpai.com/rss` | 小红书/抖音/视频号/增长方法论 | 每日多次 |
| **运营干货** | **人人都是产品经理** | `woshipm.com/operate` | 内容运营/增长策略/案例拆解 | 每日更新 |
| **营销策略** | **人人都是产品经理** | `woshipm.com/marketing` | 营销策略/品牌案例/趋势分析 | 每日更新 |
| **商业洞察** | **虎嗅网** | `huxiu.com` | 科技/互联网/商业模式深度分析 | 每日更新 |
| **AI前沿** | **机器之心** | `jiqizhixin.com` | AI技术/行业应用/深度报道 | 每日更新 |
| **AI产品** | **爱范儿** | `www.ifeng.com` (科技版块) | 科技产品/创业/趋势 | 每日更新 |
| **技术社区** | **掘金** | `juejin.cn` | AI工具/开发者/效率方法 | 每日更新 |
| **效率工具** | **少数派** | `sspai.com` | 效率工具/产品推荐/方法论 | 每日更新 |
| **行业榜单** | **新榜** | `newrank.cn` | 平台榜单/AIGC周榜/数据报告 | 每周/每日 |

### 备用数据源（主源不可用时启用）

| 数据源 | URL | 备用品类 |
|--------|------|----------|
| 知乎热榜 | `zhihu.com/search/?type=hot` | 社会热点/大众讨论 |
| 微博热搜 | `s.weibo.com/top/summary` | 热搜话题/娱乐热点 |
| 36氪 | `36kr.com` | 科技创业/融资动态 |
| IT之家 | `ithome.com` | 科技资讯/产品发布 |

---

## 数据源配置

在数据源配置文件中维护：

```yaml
# 主数据源 - 聚合热榜
sources_aggregator:
  - name: 今日热榜
    type: html
    url: https://tophub.today
    channels:
      - 微博热搜      # 搜索热度/大众情绪
      - 知乎热榜      # 深度讨论/观点碰撞
      - 抖音热榜      # 短视频趋势/种草热点
      - B站排行榜     # 年轻人关注/内容趋势
      - 微信热文      # 公众号爆款/深度阅读
      - 36氪      # 科技创业/商业洞察
      - 虎嗅      # 深度分析/科技趋势
      - 爱范儿     # 科技产品/创业动态
    min_words: 500

# 主数据源 - 垂直媒体
sources_vertical:
  - name: 数英网
    type: rss
    url: https://www.digitaling.com/rss
    category: 营销案例
    content_type: 项目/案例/广告
    min_words: 300

  - name: 运营派
    type: rss
    url: https://www.yunyingpai.com/rss
    category: 运营增长
    content_type: 运营干货/增长策略
    min_words: 500

  - name: 人人都是产品经理-运营
    type: html
    url: https://www.woshipm.com/operate
    category: 产品运营
    content_type: 运营方法论/案例分析
    min_words: 800

  - name: 人人都是产品经理-营销
    type: html
    url: https://www.woshipm.com/marketing
    category: 营销推广
    content_type: 营销策略/品牌案例
    min_words: 800

  - name: 虎嗅网
    type: html
    url: https://www.huxiu.com
    category: 商业洞察
    content_type: 深度分析/科技趋势
    min_words: 1000

  - name: 机器之心
    type: html
    url: https://www.jiqizhixin.com
    category: AI前沿
    content_type: AI技术/行业应用
    min_words: 800

  - name: 掘金
    type: html
    url: https://juejin.cn
    category: 技术社区
    content_type: AI工具/开发效率
    min_words: 500

  - name: 少数派
    type: html
    url: https://sspai.com
    category: 效率工具
    content_type: 效率方法/产品推荐
    min_words: 500

  - name: 新榜
    type: html
    url: https://www.newrank.cn
    category: 行业榜单
    content_type: 榜单/数据/周报
    min_words: 200

# 备用数据源
backup_sources:
  - name: 知乎热榜
    type: html
    url: https://www.zhihu.com/hot
    category: 社会热点
    trigger: 主数据源文章数 < 10

  - name: 微博热搜
    type: html
    url: https://s.weibo.com/top/summary
    category: 热搜话题
    trigger: 主数据源文章数 < 10
```

---

## 抓取执行流程

### Step 1：检查去重记录（每日首次抓取前）

读取以下文件，检查是否已有相同话题：

```
检查顺序：
1. 02-热点池/日报/今日热点简报-YYYY-MM-DD.md   ← 今日已入库热点
2. 02-热点池/日报/热点最终筛选-YYYY-MM-DD.md    ← 已入选题热点
3. 02-热点池/热点素材库/ 近7日素材文件夹        ← 7天内已报道话题
```

### Step 2：聚合热榜抓取（tophub.today）

一次性抓取全网热榜聚合页，提取各频道热榜：

```bash
# 抓取今日热榜首页
curl -s "https://tophub.today" > /tmp/tophub.html

# 按频道分类提取热榜标题和原始链接
# 频道：微博热搜/知乎热榜/抖音/B站/微信热文/36氪/虎嗅等
```

**提取逻辑：**
1. 解析 HTML 页面，按频道分区提取热榜列表
2. 每条提取：标题 / 原始来源URL / 热度数据（阅读/讨论量）
3. 按频道分类汇总

### Step 3：RSS 批量抓取（数英网 / 运营派）

一次性获取最新文章列表（通常20-50条）：

```bash
# 数英网 RSS
curl -s "https://www.digitaling.com/rss" > /tmp/digitaling_rss.xml

# 运营派 RSS
curl -s "https://www.yunyingpai.com/rss" > /tmp/yunyingpai_rss.xml
```

RSS 特点：一次性返回全部最新文章，包含标题、链接、摘要、发布时间，无需翻页。

### Step 4：HTML 页面抓取（人人都是产品经理 / 虎嗅 / 机器之心）

按频道抓取列表页：

```bash
# 人人都是产品经理-运营
curl -s "https://www.woshipm.com/operate" > /tmp/woshipm_operate.html

# 人人都是产品经理-营销
curl -s "https://www.woshipm.com/marketing" > /tmp/woshipm_marketing.html

# 虎嗅网
curl -s "https://www.huxiu.com" > /tmp/huxiu.html

# 机器之心
curl -s "https://www.jiqizhixin.com" > /tmp/jiqizhixin.html
```

### Step 5：逐篇正文抓取

对每个来源的文章列表，提取正文内容：

```bash
# 示例：抓取数英网文章正文
python3 fetch_article.py \
  --url "https://www.digitaling.com/articles/XXXXX.html" \
  --min_words 300 \
  --output "/path/to/02-热点池/原始素材/YYYY-MM-DD/[来源]-[文章ID].md"
```

**质量门槛：**
- 正文字数 < 300字 → 丢弃（非完整文章）
- 列表页/聚合页 → 丢弃
- 内容高度重复（与已有文章相似度>70%）→ 合并或跳过

### Step 6：去重合并

```python
# 去重逻辑伪代码
def dedupe(new_article):
    # 1. 检查标题指纹
    title_fingerprint = extract_fingerprint(new_article.title)
    if title_fingerprint in existing_fingerprints:
        append_to_existing(new_article, existing_id)
        return "MERGED"

    # 2. 检查摘要重复
    summary = extract_summary(new_article.content, 100)
    if is_duplicate(summary, today_summaries):
        return "SKIP"

    # 3. 检查7日话题重复
    if is_same_topic(new_article, last_7_days):
        return "FOLLOW_UP"

    return "NEW"
```

---

## 筛选标准

### 自动过滤

- 正文字数 < 300字 → 丢弃
- 列表页/首页/聚合页 → 丢弃
- 内容重复（摘要相似度>50%）→ 跳过
- 7天内相同话题 → 标记"跟进"，不重复入库

### AI评分（保留Top 15）

| 维度 | 权重 | 说明 |
|------|------|------|
| 时效性 | 30% | 24h内爆发的话题优先 |
| 客户关联 | 30% | 能服务≥1个客户的热点优先 |
| 多平台适配 | 25% | 能转化为≥2个平台的内容优先 |
| 素材丰富度 | 15% | 有≥3个高质量来源的优先 |

---

## 热点脱水分析

对Top 15做脱水分析，生成 `今日热点简报-YYYY-MM-DD.md`：

```markdown
# 今日热点简报 | YYYY-MM-DD

## 🔥 Top 1: [热点标题]
- **来源**：今日热榜/数英网/运营派/人人都是产品经理/虎嗅/机器之心
- **热度**：高/中/低（来自原始热度数据）
- **核心事件**：一句话概括
- **关联客户**：客户A（强）/ 客户B（中）
- **推荐指数**：⭐⭐⭐⭐⭐
- **去重状态**：✅ 新增 / 🔄 跟进 / ⏭️ 跳过（重复）

## 🔥 Top 2: [热点标题]
...
```

---

## 输出物清单

```
02-热点池/
├── 原始素材/
│   └── 原始素材-YYYY-MM-DD.md       ← 抓取回来的原始数据（含去重记录）
└── 日报/
    ├── 今日热点简报-YYYY-MM-DD.md    ← Top 15 热点（已去重）
    ├── 热点脱水分析-YYYY-MM-DD.md    ← Top 5 深度分析
    └── 热点最终筛选-YYYY-MM-DD.md    ← 入选热点（含素材引用说明）
```

---

## 异常处理

| 问题 | 处理方式 |
|------|----------|
| tophub.today 无法访问 | 切换到备用数据源（知乎热榜/微博热搜） |
| RSS 抓取失败 | 切换备用数据源 |
| HTML 页面加载超时 | 降低并发数 / 稍后重试 |
| 某数据源长时间不可用 | 从主数据源移至备用列表，通知更新 |
| 抓取数量过少（<5条） | 启动备用数据源 |
| 内容高度重复（>50%） | 合并到已有素材，标注"来源补充" |

---

## 数据源质量监控

每周检查一次数据源有效性：

```
检查清单：
□ tophub.today 是否正常加载（应>200KB）
□ 数英网 RSS 是否正常返回（应>300KB）
□ 运营派 RSS 是否正常返回（应>200KB）
□ 人人都是产品经理页面是否正常加载
□ 虎嗅网页面是否正常加载
□ 机器之心页面是否正常加载
□ 新榜榜单是否正常更新

异常处理：
- 某数据源连续3次失败 → 移入"备用数据源"列表
- 某数据源恢复可用 → 移回"主数据源"列表
```

---

*热点抓取完成 | 去重保证质量 | 多源保障广度*
