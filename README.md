# trade-chat-coach 销冠教练技能

> 从客户聊天截图/记录读出真实意图（真买/比价/探路/风险），给出推进到下单的下一步动作与可直接发送的话术。
> **国内销售优先，外贸同样适用。** 身经百战的资深销冠教练：60+ 主题实证、中英俄+六语言、三种运行模式、红线过滤。

[![Version](https://img.shields.io/badge/version-2.6.0-blue)](CHANGELOG.md)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

## 这是什么

一个 AI 技能（SKILL.md + 16 个参考文件），让任何支持 Agent Skills 的 AI 助手变成资深销售教练：

- **判断**：客户聊天截图/记录 → 四必答题（目的/时间/付款/信任）→ 真买·比价·探路·风险 + 置信度
- **话术**：中/英/俄/西/阿/法/德/葡 草稿，可直接复制发送
- **模式**：实战（完整诊断卡）/ 批量（多客户优先级表）/ 实时（2-3 条即发回复）
- **实证**：每句话术带来源与 A/B/C 数据分级（书籍方法论 + B站字幕 + 小红书/知乎高赞 + 网页正文）

## 快速开始

1. 下载 [`trade-chat-coach.skill`](releases/) 或克隆本仓库
2. 把 `trade-chat-coach/` 目录放入你的 Agent 技能目录（如 Claude Code: `~/.claude/skills/`）
3. 对 AI 说：*"帮我看看这个客户是不是真要买"* + 丢一张聊天截图

详细用法见 [使用手册.md](使用手册.md)，技术架构见 [README_技术手册.md](README_技术手册.md)。

## 目录结构

```
trade-chat-coach/
├── SKILL.md              # 主文件：五步流程 + 三模式 + 参考路由表
├── references/           # 16 个参考文件（按需读取）
│   ├── signals.md        #   信号词典（真买/比价/探路/风险）
│   ├── framework.md      #   四必答题/承诺阶梯/信任度刻度
│   ├── zh-playbook.md    #   中文场景 S1-S16 + 内贸风险
│   ├── playbook.md       #   英文场景 12 剧本
│   ├── i18n-phrases.md   #   六语言外贸短句
│   ├── objections.md     #   异议处理 25+ 话术
│   ├── eq-communication.md # 高情商句式 30+
│   ├── templates.md      #   D0→D21 完整模板
│   ├── sales-methods.md  #   SPIN/挑战者/MEDDIC/铁军/LTC
│   ├── negotiation.md    #   让步阶梯/BATNA/锚定
│   ├── influence.md      #   影响力与行为科学 + 伦理边界
│   ├── video-corpus.md   #   B站字幕实证
│   ├── web-corpus.md     #   外贸网页实证
│   ├── domestic-corpus.md #  国内销售实证（销冠六聊/30句转译）
│   ├── courses.md        #   课程/书单/知乎高赞
│   └── sources.md        #   数据审计（A/B/C 分级/禁引用清单）
└── evals/evals.json      #  评测档案（10 用例）
```

## 评测

10 个评测用例（英文比价/真买/风险、中文比价/内贸风险、俄语、中英混合、批量/实时模式、回归），全部通过。
关键指标：**四必答题结构覆盖 with-skill 5/5 vs 干净基线 0/5**——技能的核心价值是结构强制与伦理红线，而非替代模型能力。

## 红线（内置）

技能显式拒绝：演戏逼单（假库存/假稀缺）、编造资质与背书、消费信贷式催收施压、利诱回扣。
判断永远给置信度与"错在哪"——**是概率，不是判决**。

## License

MIT
