# 实验室公众号新闻 → 博客同步模板

> 将此文件复制给维护公众号的同学。发一篇公众号推文后，填下面一行，发给 PI 即可完成同步。

---

## 操作步骤（同学侧）

1. 在微信公众平台发布文章后，点文章右上角「…」→「复制链接」，得到类似 `https://mp.weixin.qq.com/s/...` 的链接。
2. 填写下方模板（一行一条），发给 PI。

## 填写模板

按以下格式，每篇新闻填 **一行**：

```yaml
- date: "YYYY-MM"
  text: "新闻标题（中文）"
  url: "从微信复制的链接"
  new: true
```

> `new: true` 表示标记为 NEW，只有最新一条填 `true`，其余填 `false`。
> 示例：

```yaml
- date: "2026-10"
  text: "仿生变体滑翔机完成首次室内试飞"
  url: "https://mp.weixin.qq.com/s/xxxxxxxxxxxx"
  new: true
```

## PI 侧操作（收到同学发来的 YAML 行后）

在 `_data/news.yml` 文件**最上方**（`- date:` 前）插入新的一行。

例如，原文件开头是：

```yaml
- date: "2026-09"
  text: "自制地面接收天线成功解调卫星数据包"
  url: ""
  new: true
```

插入后变为：

```yaml
- date: "2026-10"
  text: "仿生变体滑翔机完成首次室内试飞"
  url: "https://mp.weixin.qq.com/s/xxxxxxxxxxxx"
  new: true
- date: "2026-09"
  text: "自制地面接收天线成功解调卫星数据包"
  url: ""
  new: false
```

注意两条变更：
- 新条目的 `new: true`。
- **旧条目的 `new: true` 改为 `false`**（NEW 标签只给最新一条）。

保存后在 VS Code 中 Commit + Push 即可，博客「近期新闻」自动更新。