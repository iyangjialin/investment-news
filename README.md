# Investment News · 投资资讯

**中文** · [English](README.en.md)

<p align="center">
  <b>你投的每个赛道，AI 每天替你嚼成几条要点 —— 100% 本地运行，用你自己的 AI，零 API key。</b><br>
  Your investment sectors, distilled into a daily brief by your own AI — 100% local, zero API key.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/version-1.0.0-blue.svg" alt="version 1.0.0">
  <img src="https://img.shields.io/badge/python-3.7+-blue.svg" alt="Python 3.7+">
  <img src="https://img.shields.io/badge/license-MIT-green.svg" alt="MIT">
  <img src="https://img.shields.io/badge/赛道-12_大方向-orange.svg" alt="12 sectors">
  <img src="https://img.shields.io/badge/信息源-100+_权威媒体-red.svg" alt="100+ sources">
  <img src="https://img.shields.io/badge/依赖-纯标准库-lightgrey.svg" alt="stdlib">
  <img src="https://img.shields.io/badge/大模型-订阅_or_API-purple.svg" alt="LLM">
</p>

---

## 这是什么

**Investment News 是一个本地运行的「投资赛道资讯看板」——它用你自己的大模型，把全球 100+ 权威信息源、12 大投资赛道的最新动向，每天提炼成中文「今日要点」并翻译，呈现在一个浏览器看板里。**

不是又一个让你刷不完的新闻聚合器——它的核心是 **AI 替你读完、提炼**：每个赛道顶部 3–5 条「今日要点」，跨多源聚合去重，一眼掌握全局，感兴趣再点 ↗ 看原文。抓取在**你本机**跑（纯 Python 标准库），AI 用**你自己**的 Claude 订阅（$0）或任意 API key，**数据不出本机、零账号、零托管**。

为什么需要它：

- 想盯住一堆投资赛道，但**新闻太多、人读不过来，而且大半是英文**。
- 门户/聚合器给你的是一堆碎片；你要的是「这个赛道今天到底发生了什么」——这正是 AI 要点层在做的事。

> 📊 **它是资讯/趋势工具（领先信号），不是行情，更不是投资建议。** Industry trends, **not financial advice**.

## ✨ 能力 Features

| 能力 | 说明 |
|---|---|
| **AI 今日要点** | 每个赛道顶部 3–5 条中文要点，跨多源聚合去重、突出公司/数字/趋势，由**你自己的大模型**生成 |
| **全中文双语** | 英文标题自动翻译；卡片＝中文大字 + 原文小字，英语不好也能刷 |
| **12 大投资赛道** | AI/大模型 · 半导体/芯片 · 机器人/自动化 · 汽车/新能源车 · 能源/新能源 · 生物医药/健康 · 航天/太空 · 网络安全 · 科技/互联网 · 消费电子/数码 · 财经/宏观 · 科学/前沿 |
| **要点可溯源** | 每条要点带 ↗ 直达它主要来源的原文 |
| **一键刷新** | 看板左上角 ⟳：后端跑抓取 + AI，转圈跑完自动更新，无需碰命令行 |
| **订阅 / API 双选** | `claude-cli`（本机 Claude 订阅，$0）或任意 OpenAI 兼容 API（DeepSeek/OpenAI/硅基/OpenRouter…），改一个配置切换 |
| **本地 + 零 key** | 抓取纯 Python 标准库；数据不出本机、无数据库、无托管服务、无 RSSHub |
| **红线过滤** | 自动滤掉赌博 / 预测市场 / 加密 / 色情；时政、财经照收 |

## 📸 截图 Screenshot

![dashboard](docs/screenshot.png)

> **本工具的本体就是这个浏览器看板。** 跑起来后，一切操作的终点都是打开 `http://localhost:8793` 看它 —— AI 今日要点、中文翻译、分赛道、跳转原文，全在这个网页里（终端给不了）。

## 🚀 快速开始

**依赖**：Python 3.7+（标准库即可，**无需 pip 装任何东西**）+ 一个大模型（下方二选一）。

```bash
git clone https://github.com/simonlin1212/investment-news.git
cd investment-news

# 1) 配置大模型(见下「配置」)——默认用本机 Claude 订阅，零成本
# 2) 启动看板
python3 server.py            # 默认端口 8793，保持运行
# 3) 浏览器打开看板
open http://localhost:8793   # Windows 用 start、Linux 用 xdg-open
# 4) 点左上角 ⟳ 刷新 —— AI 抓取 + 出要点 + 翻译，转圈跑完自动更新
```

## ⚙️ 工作原理

```
sources.json  (108 个源 / 12 赛道)
       │
       ▼  scripts/fetch.py    抓取 + 红线过滤 + 最近N天 + 北京时间
  data.js  (原始条目)
       │
       ▼  scripts/digest.py   你的大模型 → 每赛道「今日要点」+ 中文翻译 + 溯源链接
  data.js  (含 AI 要点)
       │
       ▼  index.html          浏览器看板(单文件、零构建、零依赖)
  server.py：本地服务 + 刷新按钮的 /api/refresh 接口
```

全程**纯 Python 标准库 + 一个大模型**，无数据库、无 RSSHub、无托管服务。`digest` 在 `claude-cli` 模式下 spawn 本机 `claude -p`（订阅鉴权、禁用全部工具、只处理文本），**仅本机可用、$0**。

## 🤖 配置大模型（订阅 / API 二选一）

编辑 `llm.config.json`：

| provider | 说明 | 成本 |
|---|---|---|
| **`claude-cli`（默认）** | 用本机已登录的 **Claude Code 订阅**（`claude login` 过一次即可），仅本机可用 | **$0** |
| **`api`** | 任意 **OpenAI 兼容 API**（DeepSeek / OpenAI / 硅基流动 / OpenRouter…），任意机器可用 | 按量 |

```jsonc
{ "provider": "claude-cli" }                       // 用订阅，啥都不用填
// 或
{ "provider": "api",
  "api": { "base_url": "https://api.deepseek.com", "api_key": "sk-...", "model": "deepseek-chat" } }
```

## 🌐 覆盖赛道与信息源

12 大赛道、108 个精选源，**英文权威媒体 + 中文垂直媒体**并重，例如：

- **AI/大模型**：OpenAI · Google Research · Hugging Face · 量子位 · 机器之心 · 智东西 · MIT Tech Review
- **半导体/芯片**：DIGITIMES · SemiAnalysis · IEEE Spectrum · EE Times · Semiconductor Engineering
- **机器人 / 汽车 / 能源**：The Robot Report · IEEE Spectrum · Electrek · InsideEVs · CleanTechnica · 国际能源网
- **生物医药 / 航天 / 安全**：STAT · Endpoints · SpaceNews · NASA · Krebs on Security · BleepingComputer
- **科技 / 财经**：TechCrunch · The Verge · Ars Technica · 虎嗅 · 36氪 · 钛媒体 · FT · CNBC · 华尔街见闻 · 东方财富

> 完整清单见 `sources.json`。源若失效/想增删，直接改这一个文件。

## ➕ 加源 = 加一行

`sources.json` 的 `sources` 数组里加一行即可，无需改代码：

```jsonc
{ "name": "某媒体", "hint": "ai", "type": "rss", "url": "https://example.com/feed" }
```

`hint` = 赛道 key（ai / semi / robot / auto / energy / bio / space / security / tech / consumer / macro / science）。
`fetch.recent_days` 控制只看最近几天（默认 7）；`redline_keywords` 是红线过滤词。

## 🗂️ 项目结构

```
investment-news/
├── index.html          浏览器看板(单文件:侧栏12赛道 + AI要点 + 双语列表 + 刷新按钮)
├── server.py           本地服务 + /api/refresh
├── sources.json        108 源 / 12 赛道 / 红线词(改这个文件即调整源)
├── llm.config.json     大模型 provider(订阅 / API)
├── data.js             生成的数据(fetch + digest 产出)
├── scripts/
│   ├── fetch.py        抓取 + 红线过滤 + 最近N天(纯标准库)
│   ├── digest.py       调你的大模型出「今日要点」+ 翻译
│   ├── llm.py          统一大模型入口(claude-cli / api 双 provider)
│   └── build_sources.py 重建/校验 sources.json(liveness 实测)
└── docs/screenshot.png
```

## 🧰 技术栈与依赖

- **Python 3.7+**，**纯标准库**（urllib / json / xml.etree / http.server / subprocess）——抓取与看板零第三方依赖。
- **一个大模型**：本机 Claude Code 订阅（`claude-cli`，$0）或任意 OpenAI 兼容 API key。
- 联网即可（部分国际源可能需代理）。

## ⚖️ 使用边界 / 免责声明

- **仅本地运行**：不含任何托管 / 上传 / 服务端，数据只落在你本机。
- **只读公开 RSS / 接口**，低频、当个有礼貌的访客；遵守各信息源的 ToS。
- **结论仅供参考**：本工具是**资讯聚合**，呈现的是行业动向/领先信号，**不构成任何投资建议**，据此操作自负其责。

本软件以 [MIT 许可](LICENSE)「按现状」提供，不作任何担保。

## 🙋 作者

**Simon 林** · 抖音「Simon林」· 公众号「硅基世纪」

一个把全球行业资讯嚼成中文要点的本地看板。欢迎 PR 补充更多赛道 / 信息源。

## 📄 License

[MIT](LICENSE)
