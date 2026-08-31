# 标题指纹索引

## 说明

每行一条 JSON，记录已报道热点的标题指纹，用于快速去重。

## 格式

```json
{"fingerprint": "md5hex", "topic_id": "uuid", "date": "YYYY-MM-DD", "status": "new|follow|skip", "title": "原始标题（保留，供人工核查）"}
```

## 字段说明


| 字段            | 说明                                        |
| ------------- | ----------------------------------------- |
| `fingerprint` | 标题核心词 MD5 哈希（去掉品牌名/时间词/情绪词后取 MD5）         |
| `topic_id`    | 热点唯一 ID                                   |
| `date`        | 首次入库日期                                    |
| `status`      | `new`=新热点 / `follow`=跟进热点 / `skip`=跳过（重复） |
| `title`       | 原始标题，保留供人工核查，不参与比对                        |


## 使用方式

1. 抓取前：读取最近 7 天行（判断话题是否已报道）
2. 抓取中：每篇新文章提取指纹 → 比对 → 命中则合并/跳过
3. 抓取后：追加今日新指纹到文件末尾
4. 定期清理：`status=skip` 的记录保留 30 天后删除；`status=new/follow` 永久保留

## 示例行

```json
{"fingerprint": "a3f5c8e12d...", "topic_id": "2026-05-14-001", "date": "2026-05-14", "status": "new", "title": "腾讯370亿创纪录"}
{"fingerprint": "b7e2f1a09c...", "topic_id": "2026-05-14-002", "date": "2026-05-14", "status": "follow", "title": "阿里AI收入358亿"}
```

