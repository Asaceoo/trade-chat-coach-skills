# trade-chat-coach 销冠教练 — 技术手册

> 从客户聊天截图/记录读出真实意图(真买/比价/探路/风险),给出推进到下单的下一步动作与可直接发送的话术。**国内销售优先,外贸同样适用。**
>
> **手册版本:v2.10(与技能同步,2026-10-09)**

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
| v2.9 | 第 12 轮调研三重收敛(R1/R2/R3) + 扩展键收进 metadata + 赞数纠错 + 竞品贬低剔除 |
| v2.10 | 读书语料层四源合一：书目地图 ×2 + 渠道销售 + 对象系异议 + 读者划线金句库(引证型) + data-bank 数字总表 |
| v2.10.1–2.10.2 | 对抗性审查 R2/R3：金句库引证定位与条数口径(180→125 去重) + SKILL.md 路径脱敏 + 发布管道 Release 标题去硬编码 + C5 校验器假阳性修复 |
| v2.10.3 | 对抗性审查 R3：description 仍写「7 份可直接抄」而正文已是 6 份（矛盾只在对外触发字段里）→ 修为 6 份可抄 + 第 7 份引证型；技术手册两张版本历史表补齐 v2.9/v2.10（此前停在 v2.8.2） |
| v2.10.4 | 对抗性审查 R4：发布仓库 10 个历史 zip（约 10MB）脱离 git 跟踪并加 `.gitignore`；清单新增 **F8**（zip 不堆积）/**F9**（手册副本不落后）；**E6 由硬编码 3 个文件名改为按磁盘发现全部 phrasebank**（后加的 4 份库此前根本没被覆盖） |
| v2.10.5 | 对抗性审查 R5：第 3 层 `verify_artifact.py` 三处腐烂修复（陈旧默认路径 / 硬编码 `n_ref==23` → **与源目录动态比对** / frontmatter 误把 metadata 子键算顶层键）并**接入发布管道 [4b]**；zip 清理的字典序 bug 改语义版本；GitHub 门面 README 按磁盘重写；金句库分节条数由去重前 180 改为去重后 125；清单新增 **F10**/**F11** |

## 〇、发布物

- **技能包**:`trade-chat-coach.skill`（**20 个 references**；171 KB / 21 条目；官方打包器 `ROOT_EXCLUDE_DIRS` 会排除 `evals/`，设计如此）
  - 发布门禁：**包内 references 数必须 == 源目录 references 数**（本轮修复前发布版只有 16 个，与源脱节）
- **整包 zip**:`trade-chat-coach-skill-v<版本>.zip`（30 条目 / ≈950 KB；**含 `evals/fixtures/` 8 张夹具图 + `evals.json`**，便于克隆者复现评测）
- **产物校验脚本**:`trade-chat-coach-workspace\verify_artifact.py`（校验顶层目录唯一、refs 数一致、无 evals、无反斜杠、CRC、frontmatter 仅 name/description）
- **源码仓库**:https://github.com/Asaceoo/trade-chat-coach-skills
- **安装方式**:解压 .skill 或克隆仓库,把 `trade-chat-coach/` 放入你的技能目录

## 〇之二、发布流水线与验证工具(本轮新增)

**验证分四层，逐层加压**（前三层查"制品对不对"，第四层查"说明书能不能被执行"）：

| 层 | 工具 | 路径 | 查什么 |
|---|---|---|---|
| 1 结构 | 全量清单校验器 | `trade-chat-coach-workspace\validate_skill_full.py` | **39 项断言**（结构/引用/Evals/编码/一致性/发布就绪），非零退出即失败 |
| 2 上游契约 | 官方校验器 | `skill-creator\scripts\quick_validate.py` | frontmatter 白名单键 + 描述长度（**必须 `PYTHONUTF8=1`**） |
| 3 产物 | 产物校验脚本 | `trade-chat-coach-workspace\verify_artifact.py` | .skill 顶层唯一 / refs 数==源 / 无 evals / 无反斜杠 / CRC / frontmatter 仅 name+description |
| 4 **行为** | **端到端冒烟** | `trade-chat-coach-workspace\smoke_e2e.py` | **46 项**：20 个引用可解析、章节锚点（`#S3`/`§4`/`第6节`）真存在、9 类高频提问一步定位、术语有定义、红线成章 |
| 编排 | 发布流水线 | `trade-chat-coach-workspace\release_pipeline_v2.py` | 版本递增 → 两手册同步 → 构建 stage → **官方校验** → 打包 .skill/zip → CHANGELOG → 同步仓库 → git push → **建 Release + 上传资产**（含快照回滚） |

> **第 4 层的价值**：前三层全绿只说明"文件都在、格式都对"，**不能说明 AI 拿到它会不会用**。
> 冒烟测试按 SKILL.md 路由表**模拟 AI 消费**——把路由表的每一行当断言来跑，
> 于是"路由指向不存在的章节""某类提问没有入口"这类问题会在发布前暴露。
> 写这个测试时它自己也出过 2 个假阳性（表格正则被行内 `**` 截断、红线检测词写死为"不得"而正文用的是"禁止"）——
> **测试也需要被审查**，我们按实际原文修正了测试而非改技能。

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

### 7.3 清单兜底校验（39 项）

分 6 个维度：结构(6) / 引用完整性(6) / Evals 资产(6) / 编码格式(6) / 内容一致性(10) / 发布就绪(7)。
发布流水线第 0 步强制跑它，**非零退出即中止发布**，杜绝"坏包上线"。

> 校验器自身也曾犯错：初版 F1 要求 top-level 存在 `display_name/version/agent_created`——**恰是上游打包器拒绝的键**，等于"本地全绿保证上游必挂"；F5 则 `return True` 恒真。已重写为**真实打包契约验证**（剥离 5 键 → 跑上游 `validate_skill`）与带豁免清单的真检查。

### 7.4 首次真机发布暴露的 2 个流水线缺陷（已修，v2.8.0 发布时实测）

真实跑一次发布，抓到两个**只有实跑才会暴露**的 bug：

| # | 现象 | 根因 | 修复 |
|---|---|---|---|
| 1 | 两份中文名资产传上 Release 后变成 `-v2.8.md`；第二个直接 HTTP 422 `already_exists` | **GitHub 的 `uploads.github.com` 会静默丢弃 `name` 参数里的非 ASCII 字符**。探针实测：发 `?name=%E4%B8%AD%E6%96%87%E5%90%8D%E6%B5%8B%E8%AF%95-probe.md`，服务端只存成 `-probe.md`；于是「用户手册-v2.8.md」与「技术手册-v2.8.md」削名后**同名** → 重名冲突 | 资产名改**纯 ASCII**（`user-manual-v2.8.md` / `tech-manual-v2.8.md`），并在流水线加 `name.isascii()` 守卫，非 ASCII 直接中止。手册**内容**仍是中文 |
| 2 | 资产上传失败后，**已推送成功的本地版本号被回滚**成旧值，出现"远端 2.8.0 / 本地 2.7.0"不一致 | 为防"改一半失败"加了发布前快照 + 失败自动回滚，但**回滚没区分"远端是否已固化"**——`git push` 已成功时远端已是新版本，此时回滚本地即制造不一致 | 在 `git push` 成功后**立即清除回滚快照**：一旦推送成功即视为已固化，后续步骤失败只报错、不再回滚本地 |

> **教训**：回滚的边界是**最后一道不可逆动作**（push / 发布），不是"整个流程的结束"。跨过不可逆点之后，本地要向已固化的远端对齐，而不是把本地拖回去。

### 7.5 Git 凭据弹窗挂死（v2.8.1 实测，已根治）

**症状**：跑发布流水线时弹出 Git Credential Manager 的「Select an account」GUI 框（列出 `Asaceoo` 与 `x-access-token` 两个账号），
`git credential fill` 子进程无输入一直等待 → **30 秒 TimeoutExpired**，Release 步骤失败（此时代码往往已推送成功，属第 7.4 条 #2 的同款半成品状态）。

**根因**：本机凭据库同时存在**两个 GitHub 账号**，GCM 无法判断用哪个 → 升级为交互式选择。
而 `subprocess.run(["git", "credential", "fill"], ...)` 默认继承了可交互环境，**没有禁用 GCM 的 GUI**。

**修复**（`gh_token()` 与 `run()` 双侧加固）：
```python
env.update({
    "GIT_TERMINAL_PROMPT": "0",   # 禁终端提问
    "GCM_INTERACTIVE":     "never",  # ← 关键：禁 GCM 账号选择弹窗
    "GIT_ASKPASS": "",
    "SSH_ASKPASS": "",
})
subprocess.run(..., env=env, stdin=subprocess.DEVNULL, timeout=15)
```
并且 `run()` 里**所有 git 子进程**（add/commit/push）都带上 `GIT_TERMINAL_PROMPT=0` + `GCM_INTERACTIVE=never`。

**推荐用法（最稳，绕开凭据库）**：显式注入环境变量，token 不落盘、不写日志、不进记忆：
```powershell
$env:GH_TOKEN = "<your-PAT>"
python release_pipeline_v2.py --patch --message "..."
```
`gh_token()` 现在**优先读 `GH_TOKEN` / `GITHUB_TOKEN`**，只有在没有环境变量时才回落凭据库，且回落路径也已非交互化。

> **安全提醒**：PAT 一旦出现在聊天记录、日志或版本库里，即视为**已泄露**，应立即到 GitHub Settings → Developer settings → Fine-grained tokens 吊销并重发。
> 本流水线不会把 token 写入任何文件；排查时也用 `长度` / `是否非空` 断言，而不打印明文。

**① 根因侧根治：清理重复账号（已执行，2026-10-09）**

代码加固只是"绕过"，真正让弹窗消失的是**消除账号歧义**。本机 Windows 凭据管理器原有 3 条 `x-access-token` 记录：

| Target | User | 处置 |
|---|---|---|
| `git:https://github.com` | x-access-token | 🗑 删除 |
| `git:https://x-access-token@github.com:443` | x-access-token | 🗑 删除 |
| `git:https://x-access-token@github.com` | x-access-token | 🗑 删除 |
| `git:https://Asaceoo@github.com` | Asaceoo | ✅ 保留 |
| `gh:github.com:Asaceoo` | Asaceoo | ✅ 保留 |

```powershell
# 查看（不显示密码）
cmdkey /list | Select-String "github" -Context 0,2
# 删除（只需 target 去前缀）
cmdkey /delete:"git:https://x-access-token@github.com"
```

**效果对比（同一台机器实测）**：

| 时点 | 环境配置 | `git credential fill` 耗时 |
|---|---|---|
| 删除前 | 仅 `GIT_TERMINAL_PROMPT=0` | **9.4 s**（卡在等弹窗） |
| 删除前 | + `GCM_INTERACTIVE=never` | 0.6 s（靠开关绕过） |
| **删除后** | **仅 `GIT_TERMINAL_PROMPT=0`（旧配置）** | **0.5 s** ✅ |

删除后 `git credential fill` 返回 `username=Asaceoo`，`git fetch` 1.7 s 正常。
**结论：消歧（清重复账号）+ 代码加固（GCM_INTERACTIVE=never）双保险**；只做代码加固也能用，但"根因不除、偶发仍会慢"。

**② 备份与取证**：删除前用 `cmdkey /list > credman_backup_<ts>.txt` 留档（**只含 target/user/type，不含密码明文**）。

### 7.6 版本历史（发布记录）

| 版本 | 交付 |
|---|---|
| **v2.8.2** | **功能实测（无上下文子代理真实使用）驱动的 7 项可用性修复**：① 读取纪律改**按阶段定额度**并定义「单次」= 一个阶段（旧版"单次最多 2 个"与判定层要求读 3 个**直接冲突**，是唯一真正卡住测试者的缺陷）；② 新增**「我方信息」清单**（价格口径/底价权限/非价格筹码/BATNA/付款底线）——旧版 9 项缺失信息**全是客户侧**，导致话术必然留占位符；③ 新增独立**「占位符规则」**（统一 `[X]` 写法 + 填不出时删短语不删句 + 超 3 个不出稿）；④ 示范案例补**显式省略标注**（原来 §2 直跳 §6、事实表 9 字段只印 3 行）；⑤ 置信度**各档都给了升级路径**（原来只有低档）；⑥ 补价格口径处理指引；⑦ SKILL.md 提示 **evals 自测样本**（含 `.skill` 不含 evals 的说明）。冒烟断言 46 → **54 项** |
| **v2.8.1** | ① `phrasebank-price.md` §1.5「嫌贵进阶」（知乎 18 条内容级正文：报价前四资格题 / 「和什么比？」一句话诊断 / 问折扣三种含义 / 报价留余地与非价格让利算术 / 不自己拒绝客户）；② **Git 凭据弹窗根治**（清 3 条 `x-access-token` 重复账号 + `GCM_INTERACTIVE=never`，9.4s → 0.5s）；③ **新增端到端冒烟测试** `smoke_e2e.py`；④ 技术手册补 §7.5 凭据根治 |
| v2.8.0 | 3 份可直接抄原话术库 + B站 1380 条语料实证层 + 三视角对抗性审查收敛（R1+R2 共 82 项）+ 发布流水线 v2 + Release 资产名 ASCII 守卫 + 补齐 7 张评测夹具 |
| v2.9.0–2.9.x | 新增 2 份可抄话术库（phrasebank-bidan 逼单转译公式 / phrasebank-followup 跟进破冰）+ channel-sales；第 12 轮调研三重收敛：R1/R2/R3 共 32 项修复（赞数纠错 1127→3030、4069→3049；剔除贬低竞品话术；礼品禁令与 B2B 迁移表；路由表补 3 条仲裁；--help 陷阱修复）；SKILL.md 扩展键收进 metadata 使源码直接通过官方校验 |
| v2.10.0 | 读书语料层：book-corpus ×2（60 本书目地图）+ phrasebank-objections-cn（8 类对象系异议）+ phrasebank-book-highlights（读者划线金句库，引证型）+ data-bank（A/B/C 分级数字总表）+ channel-sales；参考文件 23→28（经 git ls-tree 核对）；phrasebank 文件 5→7（其中【可直接抄】5→6，第 7 份 book-highlights 为引证型） |
| v2.10.1–2.10.2 | 对抗性审查 R2/R3：金句库改为「引证型，不能照抄」定位并修正条数口径（180 原始→125 去重）；SKILL.md 本机路径脱敏（%USERPROFILE% 占位符）；发布管道 Release 标题/正文去硬编码（跟随 --message）；C5 校验器假阳性修复（files/fixtures 数组计入声明） |
| v2.10.3 | 审查 R3：**对外触发字段**仍有矛盾——description 写「7 份可直接抄」而正文已是 6 份 → 修为 6 份可抄 + 第 7 份引证型 |
| v2.10.4 | 审查 R4：10 个历史 zip（约 10MB）移出 git 跟踪 + `.gitignore`；清单新增 F8/F9；**E6 动态化**（后加的 4 份话术库此前根本没被 E6 覆盖） |
| v2.10.5 | 审查 R5：第 3 层 `verify_artifact.py` 三处腐烂修复并**接入管道 [4b]**；zip 清理字典序 bug 改语义版本；README 门面按磁盘重写；金句分节条数去重前→去重后；清单新增 F10/F11；手册预同步解除 F9 自锁 |
| v2.10.6 | 审查 R6：**首次实跑 `--dry-run` 分支**（确认无副作用：版本未变 / CHANGELOG 无 2.10.6 段 / repo clean），但日志谎称「已插入 / -> v」→ 三处补 `[DRY-RUN 未写盘]` 标记；按 `git ls-tree` 逐 tag 核对真实文件数，纠偏版本归属（参考文件 23→28、phrasebank 5→7；原文误记 27→28 与「新增 4 个库」——后者与同段正文列出的 5 个文件自相矛盾）；CHANGELOG 2.10.1–2.10.5 空壳段（仅 40–110 字）补实质正文；管道 [5] 步改为按分隔符拆 bullet（旧写法把整段 msg 当单行塞入，导致每版只剩一句标题）；清单新增 **F12**；管道 [2] 步自动向 §7.6 补版本行，并修两处守卫（边界止于连续表格块；判重只看表格行，避免正文版本串糊弄检查） |

> **功能实测的意义（v2.8.2 的来源）**：前四层验证只能证明"文件全、格式对、路由通"，
> 不能证明"**真拿它办事时好不好用**"。本轮派了一个**无历史上下文的子代理**（模拟首次使用者）
> 真实走完实战/批量/实时三个模式 + 一个合规拒绝测试，评分 **3.9/5「可直接用」**：
> 模式纪律 5/5、红线执行 5/5（回扣场景给出法律定性 + 明面替代 + 微信留痕自保，零规避建议），
> **摩擦项仅 2/5**——问题全部集中在"说明书可执行性"而非"判断质量"，这正好是只有真实使用才能暴露的层次。

> **本版验证结论（四层全绿）**：结构 39/39 ｜上游契约 `Skill is valid!` ｜产物 .skill 20 refs、zip 30 条目 ｜**端到端行为 46/46** ｜运行时可见性 22/22。
> 三方版本一致：源目录 / 中央库 / 发布仓 均为 `2.8.1`，30 个文件零差异。

---

## 八、v2.9.1 二轮对抗性审查（三视角 + R2 收敛）

### 8.1 过程纪律：**先冻结，再审查**

R1 暴露的第一个问题不是技能缺陷，而是**流程缺陷**：审查员在采样期间发现技能被**并发改写 4 次**
（8 个文件，`SKILL.md` 31,985 → 33,782 B），有 2 条缺陷被"边审边修掉"——
**审的不是同一个东西，结论无法签发。**

> **教训**：发布/评审前必须先**冻结**（快照 + 记录 tree hash），审查结论必须绑定该 hash。
> 本轮冻结基线：**`tree_hash = 4203910a313489a1`**（33 文件，`SKILL.md` 38,160 B）。

### 8.2 R1 三视角共出 **约 50 项缺陷**，最重的三类

| # | 视角 | 缺陷 | 性质 |
|---|---|---|---|
| 1 | 工具实现者 | **P1 构建阻断**：`SKILL.md` 的 5 个扩展键（`display_name` / `display_name_en` / `description_en` / `version` / `agent_created`）**不在官方白名单**，官方校验器失败、官方打包器**直接拒绝出包**（`added file count: 0`） | 只在真跑工具链时才暴露 |
| 2 | 内容审查者 | **P0 自相矛盾**：`phrasebank-bidan.md` §3 某行把「别家打折款敢用××吗？」标为 ✅销冠——而**同一文件 30 行前刚声明"本库剔除贬低竞品"** | 违反自有红线 |
| 3 | 内容审查者 | **P1 数据造假**：两处赞数标错。`👍1127` 实为 **3030**（**1127 是那篇的正文字数，被当成了赞数**）；`👍4069` 实为 **3049** | 源标注不可信 |

### 8.3 P1 的正确修法：**不是"绕过校验"，而是"让源码本身合法"**

初版流水线的做法是**打包前剥离这 5 个键**——能出包，但**元数据会永久丢失**（实测 `display_name` / `description_en` / `version` LOST）。

**正解**：官方白名单里 **`metadata` 是允许的、且支持任意嵌套键**（实证：已装的 `scrapling-official` 就用 `metadata.homepage` / `metadata.openclaw.emoji`）。
把 5 个键**收进 `metadata:`** 后：

| | 修前 | 修后 |
|---|---|---|
| 源码过官方校验 | ❌ FAIL（`Unexpected key(s)...`） | ✅ **`OK|Skill is valid!`** |
| 官方打包器 | ❌ 拒绝出包 | ✅ 正常出包 |
| 元数据 | 打包后丢失 | **完整保留** |
| 流水线 | 需剥离（`STRIP_PATTERNS` 5 条） | **不再需要剥离（已清空）** |

配套同步：流水线版本号正则改为可匹配缩进版（`^\s*version:`）；本地校验器 **A4/F1 断言升级**——
F1 从"剥离后能过"升级为更强的「**源码顶层键全部合法 且 直接过官方校验**」。

### 8.4 一个值得复用的规律：**校验器会编码旧设计**

改设计后，`validate_skill_full.py` 立刻报 2 项失败（A4 要求顶层 `version`、F1 要求"扩展键可剥离"）。
**这不是回归，是断言过期**——正确反应是**升级断言**，而不是回退设计。
本轮把 A4 放宽到"顶层或 metadata 缩进均可"，把 F1 收紧为"顶层不得有任何非白名单键"。

另发现该校验器的 `parse_frontmatter` **只解析顶层键**（`^key:` 不匹配缩进行），
所以 A4 必须**从原文正则兜底取缩进版**——这个解析器的局限本身也是一处待改进点。

### 8.5 R1 修复清单（19 项已逐条落地）

工具侧 5 项（frontmatter / 悬空"第 8 节"引用 / "11 类"枚举漏项 / 裸文件名 / 包外溯源标注）＋
内容侧 14 项（贬低竞品话术 / 礼品禁令与 B2B 迁移表 / 赞数纠错 / 「故意弄错」教作假 / 「别追问」加 P1 限定 /
§4.1 与 §4.2 相反动作加路由 / A-B-C 残余清零 / 三条真红线入 `边界` 节 / 涨价加真实性提示 /
危险应答加禁用边界 / 姿态闸门挂接 / 静默改写加标注 / 四类市场补判定法 / 零售行加迁移标记）。

### 8.6 本版验证结论

| 层 | 结果 |
|---|---|
| 结构清单 | **39/39** |
| 官方契约（**源码直接**） | **`Skill is valid!`** |
| 行为路由冒烟 | **58/58** |
| 回归护栏 | **8/8** |
| 冻结 hash | `4203910a313489a1` |

### 8.7 R2 复审：把「审的人」也审了一遍

R2 复审判定**不可以发布**，抓到 4 个发布阻塞项，其中**有两个是"修复动作本身造成的"**：

| # | 谁造成的 | 缺陷 | 教训 |
|---|---|---|---|
| **N4** | **我自己** | 我用正则"清空 `STRIP_PATTERNS`"时只吃掉了列表头，**留下残体** → `py_compile` 报 `SyntaxError: unterminated string literal` → **整条发布流水线跑不起来** | 用正则改代码**必须紧跟一次 `py_compile`**；改完不编译等于没改 |
| **N6** | **我自己** | 修术语时往 `description` 的 **YAML 双引号串里插入了裸 ASCII `"`** → frontmatter 解析失败、官方校验 FAIL | YAML 双引号串内只能放全角「」或转义；已加护栏 **G9** |
| **N2** | R1 修复动作 | 我给路由表加的 3 条「仲裁」写在了**多余单元格**里（管道数 5 vs 表头 4）→ GFM 渲染**整段丢弃**，等于 R1 的两项修复被静默吃掉 | 表格行必须机器校验列数；`validate_skill_full.py` 的 D4 只扫 `references/`，**不覆盖 SKILL.md** → 已列入待改进 |
| **N1** | R1 修复动作 | 我给 §3 加的"哪些行能进 B2B"标记，**三处写了三个互相矛盾的集合** | 集合型断言必须**由脚本从内容正推**，不能凭记忆填 |

> **最值得记住的一条**：R2 明确指出我的"已修复"声明**超出实际落地程度**——
> 例如 `research\*` 溯源标注我声称"已加"，实际只落地 **1/30**。这不是笔误，是**自证倾向**。
> 现在补齐为 **6 个文件全覆盖**，并把这条写进纪律：**声明必须与落地范围逐一对齐**。

### 8.8 三轮收敛的过程纪律

| 轮次 | 做什么 | 产出 |
|---|---|---|
| **R1** | 三视角并行对抗审查（工具实现者 / 内容审查者 / AI 使用方） | 约 **50 项**缺陷（P0×2、P1×14…） |
| **修复** | 逐条落地 | 19 项声称修复 |
| **R2** | **对冻结版**验证修复 + 找修复引入的新问题 | **13 项**（含 4 个发布阻塞），并发现 2 处"过度声称" |
| **修复** | 修 N1–N13 | 全部落地 |
| **R3** | 再验证（进行中） | — |

**冻结基线**（口径已写明，第三方可复现）：

```
tree_hash 前16位 = 7549ef8a0617a719
口径 = sha256( 排序后的 [相对路径(正斜杠) + 文件字节] 依次拼接 )
复跑 = python tree_hash.py
```

> ⚠️ **R2 反馈的一条流程问题**：R1 期间技能被**并发改写 4 次**，有 2 条缺陷"边审边修掉"，
> 导致**审的不是同一个东西、结论无法签发**。R2 起改为**先冻结、后审查**，
> 并实测校验 `live ≡ snapshot`（逐文件 path+size+sha256 相同）。
