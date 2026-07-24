# 搜索渠道与记录格式

> 本文件为 effective-industry-research skill 的搜索渠道详细参考。调研时按维度查阅对应章节。
>
> **🚀 搜索策略：agent-reach 优先，传统搜索引擎补充**
> 1. 每个维度先用 agent-reach（Exa/GitHub/Jina Reader）搜索
> 2. 再用传统搜索引擎（Google/Bing/百度）补充覆盖
> 3. 社交媒体和视频平台用 agent-reach 对应渠道验证
> 4. agent-reach 命令详见 → [agent_reach_guide.md](agent_reach_guide.md)
>
> **使用前先体检**：`agent-reach doctor --json` 确认各平台可用后端

---

## 维度①：学术界文献调研

### 🚀 agent-reach 渠道（优先使用）

| 渠道 | 命令 | 说明 |
|------|------|------|
| Exa AI 搜索 | `mcporter call 'exa.web_search_exa(query: "英文关键词 survey OR review", numResults: 10)'` | 英文论文首选，质量高 |
| GitHub 代码 | `gh search repos "英文关键词" --sort stars --limit 10` | 论文配套代码、数据集 |
| Jina Reader | `curl -s "https://r.jina.ai/PAPER_URL"` | 论文页面深度阅读 |
| B站论文解读 | `bili search "中文关键词 论文解读" --type video -n 5` | 中文论文讲解视频 |

### 传统搜索渠道（补充覆盖）

| 渠道 | URL模板 | 执行次数 | 说明 |
|------|---------|---------|------|
| Semantic Scholar API | `https://api.semanticscholar.org/graph/v1/paper/search?query=【英文关键词URL编码】&limit=10&fields=title,authors,year,externalIds,abstract,citationCount,venue` | 3-4次 | 首选，2亿+论文 |
| arXiv | `https://arxiv.org/search/?query=【英文关键词URL编码】&searchtype=all&start=0` | 1-2次 | 最新预印本 |
| Google Scholar | `https://scholar.google.com/scholar?q=【中英文关键词URL编码】&hl=zh-CN` | 1-2次 | 中英文通用 |
| 知网CNKI | `https://kns.cnki.net/kns8s/AdvSearch?keyword=【中文关键词URL编码】` | 1次 | 中文论文补充 |

### 论文分层策略

- 🥇 **经典论文**（引用>500）→ 必须精读
- 🥈 **近3年高引**（引用>50）→ 选读
- 🥉 **综述论文**（标题含 survey/review）→ 快速建立全景图

### 记录格式

```
标题: [论文标题]
作者: [作者列表]
年份: [年份]
出处: [期刊/会议名称]
链接: [URL]
引用数: [citationCount]
核心发现: [2-3句摘要]
论文层级: [经典/近3年高引/综述]
```

---

## 维度②：互联网大厂实践调研

### 🚀 agent-reach 渠道（优先使用）

| 渠道 | 命令 | 说明 |
|------|------|------|
| Exa AI 搜索 | `mcporter call 'exa.web_search_exa(query: "公司名 英文关键词 engineering blog", numResults: 10)'` | 英文技术博客首选 |
| Jina Reader | `curl -s "https://r.jina.ai/BLOG_URL"` | 大厂博客深度阅读 |
| 小红书 | `opencli xiaohongshu search "中文关键词 大厂" -f yaml` | 员工真实分享 |
| V2EX | `curl -s "https://www.v2ex.com/api/topics/show.json?node_name=程序员&page=1" -H "User-Agent: agent-reach/1.0"` | 技术社区讨论 |
| Twitter | `twitter search "公司名 keywords" -n 10` | 大厂官方动态 |

### 传统搜索渠道（补充覆盖）

#### 国内大厂

| 渠道 | URL模板 | 说明 |
|------|---------|------|
| 美团技术博客 | `site:tech.meituan.com 【中文关键词】` | 质量、测试、研发效能 |
| 阿里技术 | `site:developer.aliyun.com 【中文关键词】` | 研发效能、DevOps、质量 |
| 阿里云 | `site:developer.aliyun.com 阿里云 【中文关键词】` | 云原生、中间件、DevOps |
| 蚂蚁集团 | `蚂蚁集团 【中英文关键词】` | 金融科技、质量保障、测试 |
| 华为 | `华为 【中英文关键词】` 或 `site:huawei.com 【英文关键词】` | 云服务、研发效能、质量 |
| 百度 | `百度 【中英文关键词】` 或 `site:developer.baidu.com 【中文关键词】` | AI工程、质量保障 |
| 腾讯云+社区 | `site:cloud.tencent.com/developer 【中文关键词】` | 缺陷管理、质量运营 |
| 字节跳动 | `字节跳动 【中英文关键词】` | 质量保障、效能 |
| InfoQ中文 | `site:infoq.cn 【中英文关键词】` | 大会演讲稿、专题 |
| 掘金 | `site:juejin.cn 【中英文关键词】` | 开发者实践分享 |

> **搜索技巧**：中英文关键词同时搜索，如 `蚂蚁集团 缺陷分类 defect classification quality`

### 国外大厂搜索渠道

| 渠道 | URL模板 | 说明 |
|------|---------|------|
| Netflix TechBlog | `site:netflixtechblog.com 【英文关键词】` | 事件管理、RCA、SRE |
| Meta Engineering | `site:engineering.fb.com 【英文关键词】` | 缺陷分类、事后复盘 |
| Google SRE | `Google 【英文关键词】 SRE engineering` | SRE实践、postmortem |
| Microsoft DevBlogs | `site:devblogs.microsoft.com 【英文关键词】` | 工程实践、DevOps |
| Uber Engineering | `Uber engineering 【英文关键词】` | 事件管理 |
| Spotify Engineering | `site:engineering.atspotify.com 【英文关键词】` | 敏捷、质量 |
| GitLab Blog | `site:about.gitlab.com/blog 【英文关键词】` | DevOps、缺陷管理 |

### 记录格式

```
公司: [公司名]
标题: [文章标题]
链接: [URL]
发表时间: [日期]
核心实践: [3-5句描述具体做法]
可借鉴点: [哪些做法可直接借鉴]
局限性: [有前提条件的做法]
成熟度: [🟢规模化/🟡试点中/🔴探索中]
```

---

## 维度③：国外企业/行业方案调研

### 🚀 agent-reach 渠道（优先使用）

| 渠道 | 命令 | 说明 |
|------|------|------|
| Exa AI 搜索 | `mcporter call 'exa.web_search_exa(query: "产品名 keywords workflow best practices", numResults: 10)'` | 英文产品文档首选 |
| GitHub | `gh search repos "关键词" --sort stars --limit 10` | 开源替代方案 |
| Jina Reader | `curl -s "https://r.jina.ai/PRODUCT_DOC_URL"` | 产品文档深度阅读 |
| Reddit | `opencli reddit search "产品名 review" -f yaml` | 用户真实评价 |
| YouTube | `yt-dlp --dump-json "ytsearch5:产品名 demo"` | 产品演示视频 |

### 传统搜索渠道（补充覆盖）

#### 商业产品

| 产品 | 领域 | 搜索关键词 |
|------|------|-----------|
| Atlassian Jira | 缺陷追踪/项目管理 | `Jira 【英文关键词】 workflow best practices` |
| ServiceNow | IT服务管理/问题管理 | `ServiceNow 【英文关键词】 ITIL` |
| PagerDuty | 事件管理/告警 | `PagerDuty 【英文关键词】 process` |
| Datadog | 可观测性/APM | `Datadog 【英文关键词】 root cause` |
| Splunk | 日志分析 | `Splunk 【英文关键词】` |
| Sentry | 错误追踪 | `Sentry 【英文关键词】` |
| Bugsnag | 错误监控 | `Bugsnag 【英文关键词】` |
| New Relic | APM/可观测性 | `New Relic 【英文关键词】` |
| Linear | 项目管理 | `Linear 【英文关键词】 workflow` |

### 产品文档直搜（备选）

```
web_fetch("https://docs.PRODUCT.com/search?q=【英文关键词】")
```

### 开源项目搜索

```
web_fetch("https://github.com/search?q=【中英文关键词】&type=repositories&s=stars&o=desc")
```

### 行业报告搜索

| 报告来源 | URL | 说明 |
|---------|-----|------|
| DORA State of DevOps | `DORA state of DevOps report 【年份】` | 研发效能领域必查 |
| Gartner | `https://www.gartner.com/en/search?q=【英文关键词】` | 技术成熟度曲线、魔力象限 |
| Forrester | `https://www.forrester.com/search?q=【英文关键词】` | Wave报告、技术评估 |
| IDC | `https://www.idc.com/search?q=【英文关键词】` | 市场份额、行业预测 |
| McKinsey Digital | `https://www.mckinsey.com/search?q=【英文关键词】` | 技术战略、组织变革 |

### 记录格式

```
产品: [名称]
公司: [公司名]
类型: [商业产品/开源方案/行业报告]
链接: [URL]
核心能力: [主要功能，逐条列出]
适用场景: [适合什么规模的团队/什么场景]
成熟度: [🟢🟡🔴]
```

---

## 维度④：银行同业案例调研

### 🚀 agent-reach 渠道（优先使用）

| 渠道 | 命令 | 说明 |
|------|------|------|
| Exa AI 搜索 | `mcporter call 'exa.web_search_exa(query: "banking IT risk management keywords", numResults: 10)'` | 英文银行案例 |
| Jina Reader | `curl -s "https://r.jina.ai/REGULATION_URL"` | 监管文件深度阅读 |
| 小红书 | `opencli xiaohongshu search "银行 技术关键词" -f yaml` | 金融从业者分享 |
| V2EX | `curl -s "https://www.v2ex.com/api/topics/show.json?node_name=程序员&page=1" -H "User-Agent: agent-reach/1.0"` | 技术社区讨论 |

### 传统搜索渠道（补充覆盖）

#### 监管文件

| 机构 | 搜索关键词 | 说明 |
|------|-----------|------|
| 银保监会 | `银保监会 信息科技 风险管理 指引 【中文关键词】` | 信息科技风险管理指引 |
| 中国人民银行 | `中国人民银行 金融科技 发展规划 【中文关键词】` | 金融科技发展规划 |
| 金融稳定局 | `系统重要性银行 监管 【中文关键词】` | 大行特有监管要求 |

### 银行实践搜索渠道

| 银行 | 搜索关键词 |
|------|-----------|
| 工商银行 | `工商银行 【中文关键词】 IT架构 质量保障` |
| 建设银行 | `建设银行 【中文关键词】 金融科技 测试` |
| 招商银行 | `招商银行 【中文关键词】 技术 质量保障` |
| 平安银行 | `平安银行 【中文关键词】 金融科技 质量` |
| 中国银行 | `中国银行 【中文关键词】 DevOps 研发` |

### 金融科技平台搜索

```
蚂蚁集团 【中文关键词】 质量保障 软件测试
京东科技 【中文关键词】 质量体系 DevOps
```

### 记录格式

```
机构: [银行/机构名称]
来源类型: [年报/技术大会/监管文件/白皮书/公众号]
标题: [文档标题]
链接: [URL]
核心做法: [具体实践描述]
特殊考量: [合规/安全/审计等银行业特殊要求]
成熟度: [🟢🟡🔴]
```

---

## 维度⑤：业界一流大会调研

### 🚀 agent-reach 渠道（优先使用）

| 渠道 | 命令 | 说明 |
|------|------|------|
| Exa AI 搜索 | `mcporter call 'exa.web_search_exa(query: "大会名 keywords talk presentation", numResults: 10)'` | 英文大会演讲搜索 |
| B站 | `bili search "大会名 中文关键词" --type video -n 5` | 中文大会录制视频 |
| YouTube | `yt-dlp --dump-json "ytsearch5:大会名 keywords talk"` | 英文大会演讲 |
| 字幕提取（B站） | `opencli bilibili subtitle BVxxx` | 视频字幕提取 |
| 字幕提取（YouTube） | `yt-dlp --write-sub --write-auto-sub --sub-lang "en" --skip-download -o "/tmp/%(id)s" "URL"` | 英文字幕 |
| Twitter | `twitter search "#大会名 keywords" -n 10` | 大会实时讨论 |
| 小红书 | `opencli xiaohongshu search "大会名 笔记" -f yaml` | 参会笔记 |
| 小宇宙播客 | `~/.agent-reach/tools/xiaoyuzhou/transcribe.sh --polish "EPISODE_URL"` | 播客转录 |

### 传统搜索渠道（补充覆盖）

#### 搜索渠道

| 大会 | 领域 | 搜索方式 | 说明 |
|------|------|---------|------|
| QECon | 质量效能 | `QECon 【中英文关键词】` 或 `site:qecon.cn 【关键词】` | 国内测试效能领域标杆大会 |
| QCon | 综合技术 | `site:qconbeijing.com 【关键词】` 或 `QCon 北京 【中英文关键词】` | 全球技术大会中国站 |
| ArchSummit | 架构 | `site:archsummit.com 【中英文关键词】` | 架构/工程实践大会 |
| KDD | 数据挖掘 | `KDD conference 【英文关键词】` | 数据挖掘顶会 |
| ICSE | 软件工程 | `ICSE conference 【英文关键词】` | 软件工程顶会 |
| FSE/ESEC | 软件工程 | `ESEC FSE 【英文关键词】` | 软件工程顶会 |
| Google I/O | 综合 | `Google IO 【英文关键词】` | Google开发者大会 |
| AWS re:Invent | 云计算 | `AWS reinvent 【英文关键词】` | AWS技术大会 |
| SREcon | SRE | `SREcon 【英文关键词】` | SRE领域专业大会 |
| DevOps Days | DevOps | `site:devopsdays.org 【中英文关键词】` | DevOps社区大会 |
| TiD | 质量效能 | `TiD 大会 【中文关键词】` | 中国质量效能大会 |
| MTSC | 测试 | `MTSC 测试 大会 【中英文关键词】` | 中国移动互联网测试大会 |

### 大会资料补充搜索策略

1. 大会官网搜索演讲议题列表
2. Google搜索大会名+中英文关键词，找PPT/视频/文章
3. 搜索演讲者的GitHub/博客获取补充材料
4. InfoQ等媒体通常有大会专题报道：`site:infoq.cn 【大会名】 【中英文关键词】`

### 记录格式

```
大会: [大会名称+届次]
演讲者: [演讲者姓名+公司]
议题: [演讲标题]
链接: [PPT/视频/文章URL]
核心观点: [3-5句话总结]
趋势判断: [这个议题代表了什么趋势？]
成熟度: [🟢已规模化/🟡试点中/🔴探索中]
```