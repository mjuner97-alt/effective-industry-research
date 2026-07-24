# agent-reach 集成指南

> 本文件为 effective-industry-research skill 的 agent-reach 集成参考。
> agent-reach 是一个 15 平台多后端互联网能力路由器，覆盖搜索/社交/视频/开发/网页/播客等渠道。

## 前置检查

```bash
# 检查可用 channel 与每个平台当前激活的后端
agent-reach doctor --json
```

按输出的 `active_backend` 字段选择对应命令组。多后端平台（小红书/Reddit/Twitter）必须先体检再选命令。

---

## 一、搜索（Exa AI）

Exa AI 是高质量 AI 搜索引擎，**英文内容和技术搜索的首选**。

```bash
# 通用网页搜索（调研主要搜索方式）
mcporter call 'exa.web_search_exa(query: "查询关键词", numResults: 10)'

# 代码/技术上下文搜索
mcporter call 'exa.get_code_context_exa(query: "code question", tokensNum: 3000)'
```

### 调研场景映射

| 调研维度 | Exa 搜索示例 |
|---------|-------------|
| 学术界 | `exa.web_search_exa(query: "defect classification software survey", numResults: 10)` |
| 大厂实践 | `exa.web_search_exa(query: "Google SRE postmortem incident management", numResults: 10)` |
| 国外企业 | `exa.web_search_exa(query: "Jira defect workflow best practices", numResults: 10)` |
| 银行同业 | `exa.web_search_exa(query: "banking IT risk management defect tracking", numResults: 10)` |
| 业界大会 | `exa.web_search_exa(query: "QECon SREcon defect management talk", numResults: 10)` |

---

## 二、社交媒体（小红书/Twitter/B站/V2EX/Reddit）

### 小红书（中文用户真实反馈）

```bash
# 先体检确认后端
agent-reach doctor --json

# 搜索笔记（OpenCLI 后端，桌面首选）
opencli xiaohongshu search "关键词" -f yaml

# MCP 后端（服务器场景）
mcporter call 'xiaohongshu.search_feeds(keyword: "关键词")' --timeout 120000
```

### Twitter/X（英文社区讨论）

```bash
# 搜索推文
twitter search "关键词" -n 10

# 读取特定推文/长文
twitter tweet URL_OR_ID
twitter article URL_OR_ID

# 失败重试链：重试 → pipx upgrade → opencli twitter search
```

### Reddit（深度技术讨论）

```bash
# OpenCLI 后端（桌面首选）
opencli reddit search "关键词" -f yaml

# 读帖子全文+评论
opencli reddit read POST_ID -f yaml
```

### V2EX（中文技术社区）

```bash
# 热门主题
curl -s "https://www.v2ex.com/api/topics/hot.json" -H "User-Agent: agent-reach/1.0"

# 节点主题搜索
curl -s "https://www.v2ex.com/api/topics/show.json?node_name=python&page=1" -H "User-Agent: agent-reach/1.0"
```

### 调研场景映射

| 调研维度 | 社交媒体价值 | 推荐平台 |
|---------|-------------|---------|
| 大厂实践 | 员工真实分享、面试题、内部文化 | 小红书、V2EX |
| 国外企业 | 用户真实评价、替代方案讨论 | Reddit、Twitter |
| 银行同业 | 金融从业者经验分享 | 小红书、V2EX |
| 业界大会 | 参会者笔记、PPT 分享 | Twitter、小红书 |

---

## 三、视频/播客（YouTube/B站/小宇宙）

### B站（中文技术视频）

```bash
# 搜索视频
bili search "关键词" --type video -n 5

# 视频详情
bili video BVxxx

# 字幕提取（OpenCLI）
opencli bilibili subtitle BVxxx
```

### YouTube（英文技术演讲）

```bash
# 搜索视频
yt-dlp --dump-json "ytsearch5:关键词"

# 字幕下载
yt-dlp --write-sub --write-auto-sub --sub-lang "zh-Hans,zh,en" --skip-download -o "/tmp/%(id)s" "URL"
```

### 小宇宙播客（中文技术播客）

```bash
# 转录播客（可选 --polish 增强标点）
~/.agent-reach/tools/xiaoyuzhou/transcribe.sh --polish "https://www.xiaoyuzhoufm.com/episode/EPISODE_ID"
```

### 调研场景映射

| 调研维度 | 视频/播客价值 | 推荐平台 |
|---------|-------------|---------|
| 学术界 | 论文讲解、综述视频 | YouTube |
| 大厂实践 | 技术分享视频、大会录制 | B站、YouTube |
| 业界大会 | 大会演讲、圆桌讨论 | B站、YouTube、小宇宙 |

---

## 四、网页深度阅读（Jina Reader / Web Reader）

```bash
# Jina Reader（首选，速度快）
curl -s "https://r.jina.ai/URL"

# Web Reader MCP（需要格式控制时）
mcporter call 'web-reader.webReader(url: "URL")'
```

### 调研场景映射

- Exa 搜索得到结果列表后，对关键文章用 Jina Reader 深度阅读
- 大厂技术博客、行业报告、产品文档等均可直接用 Jina Reader 抓取正文

---

## 五、开发工具（GitHub）

```bash
# 搜索相关仓库
gh search repos "关键词" --sort stars --limit 10

# 搜索代码
gh search code "关键词" --language python

# 仓库详情
gh repo view owner/repo
```

### 调研场景映射

| 调研维度 | GitHub 价值 |
|---------|------------|
| 国外企业 | 开源项目、工具链 |
| 学术界 | 论文配套代码、数据集 |
| 大厂实践 | 开源框架、内部工具 |

---

## 调研流程中的 agent-reach 使用策略

### Step B 五维调研中的推荐搜索顺序

```
1. Exa AI 搜索（主力，英文/技术内容质量最高）
   ↓ 补充/验证
2. GitHub 搜索（开源项目、代码参考）
   ↓ 深度阅读
3. Jina Reader（关键文章深度阅读）
   ↓ 社交验证
4. 小红书/Twitter/Reddit/V2EX（真实用户反馈和讨论）
   ↓ 视频补充
5. B站/YouTube（大会演讲、技术分享字幕提取）
   ↓ 兜底
6. 传统搜索引擎（site: 定向搜索、中文学术等 Exa 覆盖不足的领域）
```

### 各维度 agent-reach 使用侧重

| 维度 | 主力渠道 | 辅助渠道 |
|------|---------|---------|
| ①学术界 | Exa + Jina Reader（论文搜索+深度阅读） | GitHub（代码）、B站（论文解读视频） |
| ②大厂 | Exa + Jina Reader（技术博客） | 小红书/V2EX（员工分享）、Twitter（官方动态） |
| ③国外企业 | Exa + GitHub（产品+开源） | Reddit（用户评价）、YouTube（产品演示） |
| ④银行同业 | Exa + Jina Reader（监管文件、年报） | 小红书/V2EX（从业者分享） |
| ⑤业界大会 | Exa + B站/YouTube（演讲视频+字幕） | Twitter（大会讨论）、小红书（参会笔记） |

---

## 工作区规则

**不要在 agent workspace 创建文件。** 使用 `/tmp/` 存放临时输出，`~/.agent-reach/` 存放持久数据。

## 版本检查

完成一次较大调研后，顺手运行：
```bash
agent-reach check-update
```
有新版就在收尾汇报里提醒用户更新。