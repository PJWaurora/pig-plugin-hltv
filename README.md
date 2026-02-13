# pig-plugin-hltv

> HLTV CS2 赛事追踪插件 - [pig-sender](https://github.com/PJWaurora/pig-sender) QQ 机器人模块

## 功能

- **战队订阅** — 订阅关注的 CS2 战队，比赛开始/结束自动推送通知
- **今日赛程** — 查看当日所有 CS2 比赛日程安排
- **实时比分** — 查看正在进行的比赛实时比分（地图分数与回合分数）
- **战队排名** — 查看 HLTV 官方战队世界排名 Top 20
- **比赛结果** — 查看最近完成的比赛结果
- **每日推送** — 每日定时推送订阅战队的当日比赛安排
- **双数据源** — 支持 Bo3.gg API + HLTV 爬虫双数据源，自动降级回退
- **深色主题卡片** — 使用 PIL 渲染精美暗色主题赛事卡片

## 架构

```
hltv/
├── manifest.json         # 模块元信息与配置 schema
├── hltv_module.py        # 主模块（HLTVModule）— 命令路由与消息处理
├── models.py             # 数据模型 — MatchInfo, LiveMatch, MatchResult, TeamRanking
├── data_service.py       # 数据服务层 — 统一数据获取与缓存管理
├── live_providers.py     # 实时数据提供者 — Bo3.gg API 和 HLTV 爬虫实现
├── hltv_scraper.py       # HLTV 网页爬虫 — 使用 curl_cffi 绕过反爬
├── image_gen.py          # 图片生成器 — 赛事卡片渲染
└── subscription_db.py    # 订阅数据库 — SQLite 存储战队订阅关系
```

## 配置

| 配置组 | 参数 | 类型 | 默认值 | 说明 |
|--------|------|------|--------|------|
| **数据源** | `bo3gg_enabled` | boolean | `true` | 启用 Bo3.gg API |
| | `fallback_to_hltv` | boolean | `true` | API 失败回退到爬虫 |
| | `request_timeout` | integer | `30` | HTTP 请求超时（秒） |
| **轮询** | `match_poll_interval` | integer | `60` | 比赛状态轮询间隔（秒） |
| | `live_score_interval` | integer | `30` | 实时比分更新间隔（秒） |
| **推送** | `daily_notify_enabled` | boolean | `true` | 启用每日推送 |
| | `daily_notify_hour` | integer | `9` | 每日推送时间（小时） |
| | `match_start_notify` | boolean | `true` | 比赛开始通知 |
| | `match_end_notify` | boolean | `true` | 比赛结束通知 |
| **显示** | `timezone` | string | `Asia/Shanghai` | 显示时区 |
| | `render_style` | string | `modern_dark` | 渲染风格 |

## 使用方法

```
@机器人 订阅战队 NaVi       # 订阅 NaVi 战队
@机器人 取消订阅 NaVi       # 取消订阅
@机器人 我的订阅            # 查看当前订阅列表
@机器人 今日比赛            # 查看今日赛程
@机器人 实时比分            # 查看直播比赛比分
@机器人 战队NaVi           # 查看 NaVi 近期比赛
@机器人 战队排名            # 查看 HLTV 排名
@机器人 比赛结果            # 查看最近结果
```

## 依赖

- `aiohttp` — 异步 HTTP 请求
- `curl_cffi` — 绕过 HLTV 反爬检测
- `beautifulsoup4` + `lxml` — HTML 解析
- `Pillow` — 赛事卡片图片渲染
- `cs2api` — Bo3.gg CS2 赛事 API

## 许可

MIT
