# trade-chat-coach 销冠教练技能

> 从客户聊天截图/记录读出真实意图（真买/比价/探路/风险），给出推进到下单的下一步动作与可直接发送的话术。
> **国内销售优先，外贸同样适用。** 资深销冠教练：60+ 主题实证、中英俄+六语言、三种运行模式、红线过滤。

[![Version](https://img.shields.io/badge/version-2.8.0-blue)](CHANGELOG.md)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

## 这是什么

一个 AI 技能（SKILL.md + **20 个参考文件**），让任何支持 Agent Skills 的 AI 助手变成销售教练：

- **判断**：客户聊天截图/记录 → 四必答题（目的/时间/付款/信任）→ 真买·比价·探路·风险 + 置信度
- **话术**：中/英/俄/西/阿/法/德/葡 草稿，可直接复制发送
- **模式**：实战（完整诊断卡）/ 批量（优先级总表 P1/P2/P3）/ 实时（2–3 条即发回复）
- **实证**：话术带来源与可靠性分级（实证级 A/B/C）；B站 1380 条评论语料实证层

## 快速开始

1. 从 [Releases](https://github.com/Asaceoo/trade-chat-coach-skills/releases/latest) 下载最新 `trade-chat-coach.skill`（或整包 zip），或克隆本仓库
2. 把 `trade-chat-coach/` 目录放入你的 Agent 技能目录（如 Claude Code: `~/.claude/skills/`）
3. 对 AI 说：*"帮我看看这个客户是不是真要买"* + 丢一张聊天截图

详细用法见 [使用手册.md](使用手册.md)，技术架构见 [README_技术手册.md](README_技术手册.md)。

## 目录结构

```
trade-chat-coach/
├── SKILL.md                    # 主文件：五步流程 + 三模式 + 参考路由表（带仲裁规则）
├── references/                 # 20 个参考文件（按需读取）
│   ├── signals.md              #   信号词典（真买/比价/探路/风险）
│   ├── framework.md            #   四必答题/承诺阶梯/信任度/P1-P2-P3 分档
│   ├── zh-playbook.md          #   中文场景 S1-S16 + 内贸风险与合规
│   ├── playbook.md             #   英文场景 12 剧本
│   ├── i18n-phrases.md         #   六语言外贸短句
│   ├── objections.md           #   异议处理 25+ 话术 + 成交信号/推进
│   ├── eq-communication.md     #   高情商句式 20 条 + 实证依据
│   ├── templates.md            #   D0→D21 完整模板
│   ├── sales-methods.md        #   SPIN/挑战者/MEDDIC/铁军/LTC
│   ├── negotiation.md          #   让步阶梯/BATNA/锚定
│   ├── influence.md            #   影响力与行为科学 + 伦理边界
│   ├── video-corpus.md         #   B站语料实证
│   ├── web-corpus.md           #   外贸网页实证（22 主题）
│   ├── domestic-corpus.md      #   国内销售实证 + 催款函三级递进
│   ├── courses.md              #   课程/书单/知乎高赞
│   ├── sources.md              #   数据审计（实证级 A/B/C / 禁引用清单）
│   ├── phrasebank-eq-cn.md     #   ★ 高情商沟通话术库（11 类情境，可直接抄）
│   ├── phrasebank-price.md     #   ★ 价格异议与议价话术库（嫌贵四岔口/让步六铁律/守价）
│   ├── phrasebank-closing.md   #   ★ 引导下单话术库（成交信号/收口七术/催单/防反悔）
│   └── bili-corpus-2026-10.md  #   ★ B站 1380 条语料实证层（含陌拜案例逐句拆解）
└── evals/
    ├── evals.json              #   11 个评测用例（含 expectations 断言）
    └── fixtures/               #   8 张评测夹具截图（中英俄三语）
```

> `.skill` 包按官方打包器约定**排除 `evals/`**；评测资产随源码仓库与整包 zip 分发。

## 评测

**11 个评测用例**（英文比价/真买/风险、中文比价/内贸风险、俄语、中英混合、批量/实时模式、回归、盲测）+ **8 张夹具截图**，全部可复现。

关键指标：**四必答题结构覆盖 with-skill 5/5 vs 干净基线 0/5**——技能的核心价值是结构强制与伦理红线，而非替代模型能力。

> 注：`evals.json` 的 `status` 为自述结论（`verification: self-reported`）；`expectations` 已补齐，runner 机检链路待接入。

## 红线（内置）

技能显式拒绝：演戏逼单（假库存/假稀缺）、编造资质与客户案例、消费信贷式催收施压、以礼品/宴请或回扣换取采购决策。
判断永远给置信度与"错在哪"——**是概率，不是判决**。

## 版本

| 版本 | 要点 |
|---|---|
| **v2.8.0** | 3 份原话术库 + B站 1380 条语料实证层 + 三视角对抗性审查收敛（R1+R2 共 82 项）+ 发布流水线 v2 |
| v2.7.x | 第 11 轮调研：EasySpider 内容级抓取 + 知乎正文入库 |
| v2.6.x | 对抗性审查收敛 + GitHub 开源发布 |

完整变更见 [CHANGELOG.md](CHANGELOG.md)。

## License

MIT
