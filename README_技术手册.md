# trade-chat-coach 销冠教练 — 技术手册

> 从客户聊天截图/记录读出真实意图(真买/比价/探路/风险),给出推进到下单的下一步动作与可直接发送的话术。**国内销售优先,外贸同样适用。**
>
> **手册版本:v2.8(与技能同步,2026-10-09)**

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
| v2.6 | 对抗性审查收敛 + orchestrator 修复 + GitHub 开源发布 |
| v2.7 | 第 11 轮调研:EasySpider 内容级抓取(微信读书成功/知乎登录墙) + 知乎正文 18 条入库 |
| **v2.8** | **3 份原话术库(沟通/议价/下单) + B站 1380 条语料实证层 + 三视角对抗性审查修复 + 发布流水线 v2** |

## 〇、发布物

- **技能包**:`trade-chat-coach.skill`（**20 个 references**；171 KB / 21 条目；官方打包器 `ROOT_EXCLUDE_DIRS` 会排除 `evals/`，设计如此）
  - 发布门禁：**包内 references 数必须 == 源目录 references 数**（本轮修复前发布版只有 16 个，与源脱节）
- **整包 zip**:`trade-chat-coach-skill-v<版本>.zip`（30 条目 / ≈950 KB；**含 `evals/fixtures/` 8 张夹具图 + `evals.json`**，便于克隆者复现评测）
- **产物校验脚本**:`trade-chat-coach-workspace\verify_artifact.py`（校验顶层目录唯一、refs 数一致、无 evals、无反斜杠、CRC、frontmatter 仅 name/description）
- **源码仓库**:https://github.com/Asaceoo/trade-chat-coach-skills
- **安装方式**:解压 .skill 或克隆仓库,把 `trade-chat-coach/` 放入你的技能目录

## 〇之二、发布流水线与验证工具(本轮新增)

| 工具 | 路径 | 作用 |
|---|---|---|
| 发布流水线 | `trade-chat-coach-workspace\release_pipeline_v2.py` | 版本递增 → 两手册同步 → 构建 stage → **官方校验** → 打包 .skill/zip → CHANGELOG → 同步仓库 → git push → **建 Release + 上传资产** |
| 全量清单校验器 | `trade-chat-coach-workspace\validate_skill_full.py` | 36 项断言(结构/引用/Evals/编码/一致性/发布就绪),非零退出码即失败 |
| 官方校验器 | `skill-creator\scripts\quick_validate.py` | 上游 frontmatter 契约校验(白名单键 + 描述长度) |

用法:
```
python release_pipeline_v2.py --minor --dry-run      # 预演,不落盘
python release_pipeline_v2.py --minor --message "…"  # 正式发布
```

**必须 `PYTHONUTF8=1`**(流水线内部已强制):官方校验器用系统默认编码读 SKILL.md,中文 Windows 下是 GBK,不设会 `UnicodeDecodeError` 直接崩。

## 一、技能本体

**已安装位置(所有会话可见)**
- 实体:`C:\Users\iamly\.skills-manager\skills\trade-chat-coach\`
- 挂载:`C:\Users\iamly\.dsh\skills\trade-chat-coach\`(junction)
- 工作区源:`D:\deepseek\skills\trade-chat-coach\`(改这个,然后 `skills-manager-cli.exe skills update "trade-chat-coach"` 同步)

**20 个参考文件(按需读取,路由表见 SKILL.md)**
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
| **话术库** | **phrasebank-eq-cn.md** | **中文高情商情境话术 11 类(破冰/赞美/共情/拒绝/道歉/化解尴尬/催回复/坏消息/求人/边界) + 油腻话术黑名单 + 发出前五遍自检** |
| **话术库** | **phrasebank-price.md** | **嫌贵四岔口诊断 + 8 类价格异议应答 + 让步六铁律 + 守价五替身 + 三档报价法** |
| **话术库** | **phrasebank-closing.md** | **引导下单:成交信号 10 条 / 收口七术 / 催单节奏 / 破"再考虑"六种真身 / 临门一脚 / 防反悔 / 复购转介绍** |
| **实证** | **bili-corpus-2026-10.md** | **B站 1380 条评论实证层:陌拜案例逐句拆解 / 筹码四类型 / 定价方法 / 大客户三路径 / 获客渠道 / 灰色现实红线** |

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

## 七、对抗性审查记录（v2.8，三视角 + 工具闭环）

审查方式：**工具闭环拿真实报错 → 据此修正 → 复验**，而非人工目测。清单校验器 36 项断言作为兜底。

### 7.1 工具闭环实测出的真实缺陷（已修）

| # | 严重度 | 缺陷 | 证据 | 修复 |
|---|---|---|---|---|
| 1 | **P0** | `evals.json` 引用的 7 张评测截图**全部缺失**，11 个用例却全标 `passed`——评测结论不可复现 | 全目录搜 `*.png` = 0 张；仓库版 `trade-chat-coach/evals/` 也只有 `evals.json` | 从 workspace 补入 8 张 fixture 到 `evals/fixtures/` |
| 2 | **P1** | `evals.json` 中 `"fixture": "case2_genuine_buyer_p1/p2.png"` 是**畸形值**（两个文件名用 `/` 粘成一个），任何 runner 都无法解析 | `evals.json:6` | 改为 `"fixture": "case2_genuine_buyer_p1.png"` + 新增 `"fixtures": [p1, p2]` 数组 |
| 3 | **P1** | 官方校验器 `quick_validate.py:22` 用**系统默认编码**读 SKILL.md，中文 Windows(GBK) 下 `UnicodeDecodeError` 直接崩 | 实跑 traceback：`'gbk' codec can't decode byte 0xae` | 流水线强制 `PYTHONUTF8=1`；已在技术手册 〇之二 记录 |
| 4 | **P2** | `SKILL.md` 的 `description` 声明"内置 **15** 个话术与方法论库"，实际已是 **20** 个参考文件——描述与制品不符，会误导 AI 使用方 | `SKILL.md:5` | 改为 20，并补登 3 份原话术库文件；新增校验项 E7 防复发 |
| 5 | **P2** | 新增语料文件用了 📗 记号但**未在任何处定义**，AI 使用方无法解码 | `bili-corpus-2026-10.md` | 加"记号说明"表（📗/🏆/📘/👍N + 分级说明 + 红线）；新增校验项 D3 防复发 |
| 6 | **P2** | `description_en` 单行 1018 字符，超长行影响可读与 token 效率 | `SKILL.md:6` | 精简至 <1000 字符 |

### 7.2 官方工具链契约（实测确立）

- **原始技能目录必被官方校验拒**：`quick_validate.py` 的 `ALLOWED_PROPERTIES` 只允许 `name/description/license/allowed-tools/metadata/compatibility`，本技能的 `display_name/display_name_en/description_en/version/agent_created` 属"越权键"
- **因此打包前必须剥离这 5 个键**，剥离后官方校验返回 `Skill is valid!`（已实测）
- 打包器 `package_skill.py` 的 `ROOT_EXCLUDE_DIRS = {"evals"}`：**`.skill` 产物不含 evals**，这是设计如此；评测资产只随源码仓库与 zip 分发

### 7.3 清单兜底校验（36 项）

分 6 个维度：结构(6) / 引用完整性(6) / Evals 资产(5) / 编码格式(6) / 内容一致性(8) / 发布就绪(5)。
发布流水线第 0 步强制跑它，**非零退出即中止发布**，杜绝"坏包上线"。
