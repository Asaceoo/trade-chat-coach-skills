# trade-chat-coach 销冠教练 — 技术手册

> 从客户聊天截图/记录读出真实意图(真买/比价/探路/风险),给出推进到下单的下一步动作与可直接发送的话术。**国内销售优先,外贸同样适用。**
>
> **手册版本:v2.6(与技能同步,2026-10-07)**

## 版本历史

| 版本 | 内容 |
|---|---|
| v1.0 | 基础判断(五步流程/输出模板) |
| v2.0 | 9 个话术库 + A/B/C 实证分级 + 路由表 |
| v2.1 | 六语言外贸短句(i18n) |
| v2.2 | 完整跟进模板库(D0→D21 整件) |
| v2.3 | 批量台账/实时对话模式 + 示范案例 + 一致性修复 |
| v2.4 | 中文客户支持 + 国内销售重心 + 读图兜底 + 鲁棒性验证 |
| v2.5 | 知乎/小红书/B站字幕实证层 + 销冠六聊/30句转译 + MediaCrawler 数据回流 |
| **v2.6** | **对抗性审查收敛 + orchestrator 修复 + GitHub 开源发布** |

## 〇、发布物

- **技能包**:`trade-chat-coach.skill`(104KB,SKILL.md + 16 references)
- **源码仓库**:https://github.com/Asaceoo/trade-chat-coach-skills
- **安装方式**:解压 .skill 或克隆仓库,把 `trade-chat-coach/` 放入你的技能目录

## 一、技能本体

**已安装位置(所有会话可见)**
- 实体:`C:\Users\iamly\.skills-manager\skills\trade-chat-coach\`
- 挂载:`C:\Users\iamly\.dsh\skills\trade-chat-coach\`(junction)
- 工作区源:`D:\deepseek\skills\trade-chat-coach\`(改这个,然后 `skills-manager-cli.exe skills update "trade-chat-coach"` 同步)

**16 个参考文件(按需读取,路由表见 SKILL.md)**
| 层 | 文件 | 内容 |
|---|---|---|
| 判断 | signals.md | 真买/比价/探路/风险信号词典、置信度标定、跨地区差异 |
| 判断 | framework.md | 四必答题、需求翻译四层、承诺阶梯、信任度 0-5 |
| 判断 | zh-playbook.md | 中文信号 + S1-S16 中文话术 + 内贸风险与合规 |
| 话术 | playbook.md | 12 个英文场景剧本 |
| 话术 | i18n-phrases.md | 俄/西/阿/法/德/葡 六语言跟进短句 + 文化雷区 |
| 话术 | objections.md | 异议分类 + 模型(LAER/3F/太极/ARC) + 25+ 条中英话术 |
| 话术 | eq-communication.md | 高情商句式 30+ 条 + 实证发现 |
| 话术 | templates.md | D0→D21 整封模板(报价/PI/催单/延误/返单/电话/会议) |
| 方法论 | sales-methods.md | SPIN/挑战者/顾问式/Miller Heiman/MEDDIC/铁军/LTC |
| 方法论 | negotiation.md | 让步阶梯/BATNA/锚定/Malhotra 清单 |
| 方法论 | influence.md | Cialdini 七原则 + 行为科学(带原始实验出处) + 伦理边界 |
| 实证 | video-corpus.md | B站 12 视频字幕提炼(天地人框架/购买冲动/六聊) |
| 实证 | web-corpus.md | 外贸 22 主题网页正文(含催款层/93%砍价数据) |
| 实证 | domestic-corpus.md | 国内 20 主题(FFAB拜访/应标/催款三级/国企采购/销冠六聊/30句转译) |
| 实证 | courses.md | 培训课程地图 + 知乎高赞(虚拟决定者/转介绍) + 知乎语料 |
| 审计 | sources.md | 双平台评分 + A/B/C 分级 + 禁引用清单 + 平台攻防结果 |

## 二、抓取工具箱(本机)

- 脚本:`D:\deepseek\research_toolbox\scrape.py`(命令:weread / douban / dedao / bili / cffi / pw / **search** / selftest)
- **免费搜索 MCP(免 API key)**:`mcporter call search.web_search query="..." num=5` —— Bing 主通道(带质量自检)+ 百度自动回退;server 源码 `research_toolbox\search_mcp.py`,配置在 `D:\deepseek\config\mcporter.json`
- 环境:`C:\Users\iamly\.agent-reach-venv\`(requests/httpx/bs4/lxml/curl_cffi/playwright/DrissionPage/yt-dlp/feedparser/mcp)
- MCP:`mcporter`(全局 npm)+ `D:\deepseek\config\mcporter.json`(playwright / search 可用;firecrawl 待 key)
- Playwright 用系统 Chrome(`--browser chrome --headless`),免下载浏览器

## 三、调研数据(研究足迹)

`D:\deepseek\trade-chat-coach-workspace\research\`:
- `_weread_raw.csv` / `booklist_mined.md` — 微信读书 464 条书目
- `douban_books.md` — 16 本核心书豆瓣评分+要点
- `bilibili_corpus.md` — B站 51 条视频元数据(yt-dlp 时代)
- `bilibili_corpus_pw.md` — B站语料扩充(Playwright 通道, 2026-10-07)
- `netease_books.md` — 网易云阅读排行榜/出版图书
- `bookan_netease.md` — 博看(负面结论:期刊库无销售书)
- `platform_probe.md` — 平台探测记录
- `r1-sales-methods.md` / `r5-eq-communication.md` — 两路深度调研(完整落盘)

评测:`D:\deepseek\trade-chat-coach-workspace\iteration-{1,2,3,4}\`(中英俄三语 7+ 用例,with-skill vs 干净基线对比;四必答题结构 5/5 vs 0/5)

## 四、当前卡点(需要用户输入才能解锁)

| 卡点 | 解锁方式 |
|---|---|
| 知乎/小红书正文 | Cookie-Editor 插件导出登录态 → 发给我 |
| B站字幕 | 关闭 Chrome 后重试 `--cookies-from-browser chrome`;或导出 cookie 文件 |
| firecrawl 全文抓取 | firecrawl.dev 注册免费 key |
| 搜索插件 | 修好 DeepSeek 搜索 API key(当前 401) |
| 得到/京东读书 SPA | 需登录会话 + 页面适配器 |

## 五、故障排查（对抗性审查实测产出，2026-10-07）

| 症状 | 原因 | 解法 |
|---|---|---|
| `mcporter list` 报 "Previous daemon exited unexpectedly / unverified retirement" | daemon 状态文件记录了已死进程的 PID/管道 | 确认该 PID 已死 → 删除 `C:\Users\iamly\.mcporter\daemon\{user.json,user.key,user.log}` → `mcporter daemon start` |
| `scrape.py bili` 报 "HTTP Error 412" | yt-dlp 搜索被 B站风控（服务端） | 已绕过：`bili` 命令默认走 **Playwright 渲染后端**（2026-10-07 验证有效），yt-dlp 仅作回退 |
| 中文输出乱码 | GBK 控制台 | v2 起脚本内部已 `sys.stdout.reconfigure(utf-8)`，无需手动设置 |
| 当前会话模型读不了图 | 模型不支持图像输入 | 把对话文字贴给技能即可（技能有读图失败兜底） |

## 六、验证过的结论(防翻车清单,详见 sources.md)

- 禁引用:"5 次拒绝 8 次跟进"、"64% 销售不主动要成交"、"35,000 次 SPIN 访谈" 均为查无出处的民俗数字
- Challenger 销售原始数据未公开("not obtainable for reference")
- 情绪标注只在客户母语中有效(2026 PNAS)
- 本技能红线:不编造资质/假水单/假稀缺/虚假背书;回扣=双方都要担责,微信留痕拒绝是自保
