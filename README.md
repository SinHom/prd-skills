<div align="center">

# PRD Skills

> *「写 PRD 的那双手，不去查 PRD。」*

[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-2%20skills-blueviolet)](skills/)
[![skills.sh](https://skills.sh/b/SinHom/prd-skills)](https://skills.sh/SinHom/prd-skills)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Zero API Key](https://img.shields.io/badge/API%20Key-不需要-brightgreen)](#安全边界)

**把碎片化的业务输入收敛成研发级 PRD，再用同一套六维质量门把 PRD 查一遍——生成和评审分成两双手。**

[看效果](#效果示例) · [安装](#快速开始) · [触发方式](#触发方式) · [两个技能的分工](#两个技能的分工) · [安全边界](#安全边界)

</div>

---

## 它解决什么问题

产品经理手里最不缺的就是"半份输入"——一段语音转文字、一份会议纪要、一个"我们要做个 XX 平台"的口头需求。缺的是把它变成研发能照着开工的文档。

大多数 AI 写 PRD 的做法是：你把需求给它，它一口气吐一份看着挺全的文档。问题在两头。**上游**，它不问，你不知道自己漏了什么——退款逆向、并发锁、数据留存周期、异常终态，这些不写进去，研发做到一半才发现。**下游**，它写完就完了，没有人以陌生研发的视角再读一遍，于是"验收标准：页面正常显示"这种话就一路滑到开发手里。

这两个技能是把这两头分别补上：

- `prd-design` 在生成前先做**行业经验智能补全**——识别产品类型（电商/政务/医疗/B端SaaS/AI对话…），把该行业的标准必备模块逐项对照，缺的显式列出来问你。生成后再跑一遍**六维质量门**自检，critical/major 当场修完才交付。
- `prd-review` 只评审不重写。它用同一套六维标准，但换一双手——**陌生研发的视角**，看这份 PRD 看不懂、没法测、没法做的分别是哪几处，输出带严重性分级的 findings 清单。

写的人和查的人不是同一个视角，这是这套技能唯一的核心主张。

---

## 效果示例

真实产出物都在仓库里，可以直接点开看：

| 产物 | 位置 |
|---|---|
| 完整 PRD 骨架样例（v1.3 实战版） | [`skills/prd-design/references/examples/prd-v1.3-full.md`](skills/prd-design/references/examples/prd-v1.3-full.md) |
| 审查报告样例（四级严重性 findings） | [`skills/prd-review/references/examples/review-report-sample.md`](skills/prd-review/references/examples/review-report-sample.md) |
| 正反例对照（什么叫可测的验收标准、什么叫越界到 HLD） | [`skills/prd-design/references/examples/positive-negative-patterns.md`](skills/prd-design/references/examples/positive-negative-patterns.md) |

`prd-review` 的产出长这样：

```markdown
## findings 清单

### 🔴 critical（必须修，否则无法开发）
| # | 维度 | 位置 | 问题 | 修复建议 |
|---|------|------|------|----------|
| 1 | 一致性与闭环 | 3.2 订单状态机 | 术语表定义 3 态，3.2 只枚举 2 态 | 以术语表为准补全"已取消"态及其流转 |
| 2 | 可测试性 | 4.1 数据留存 | "数据保留适当时间"，无法转测试用例 | 明确留存天数与归档/删除触发条件 |

### 总体结论
不建议交付。六维度通过 4/6，critical: 2 | major: 5 | minor: 3 | suggestion: 1
```

---

## 快速开始

```bash
npx skills add SinHom/prd-skills
```

装完对 Agent 说第一句话：

```text
我想做一个面向中小餐饮商家的订货小程序，帮我整理成 PRD。
```

它会先给你一份理解摘要和补全清单等你确认，再吐完整 PRD。

**Claude Code plugin 通道：**

```bash
/plugin marketplace add SinHom/prd-skills
/plugin install prd-skills
```

---

## 触发方式

**prd-design** —— 说这些话会触发：

- 「写 PRD」「做一份 PRD」「出一份需求文档」
- 「把这个需求整理成 PRD」「把这个想法落成 PRD」
- 「把这个业务场景做成产品」「帮我整理需求」
- 「写产品文档」「需求评审材料」
- 「补全订单模块的异常流」「这个功能加个退款逆向」（单点补全走快速通道）
- 「生成 PRD 大纲」「出个框架」（只出骨架不展开）

**prd-review** —— 说这些话会触发：

- 「审查这份 PRD」「PRD 评审」「检查这份 PRD」
- 「这份需求文档有什么问题」「PRD 体检」
- 「帮我看看这个 PRD 漏了什么」
- 「这个 PRD 能不能交付研发」
- 「帮我看看这个 PRD 里的状态机对不对」（只看指定维度）

**这两个技能都不用于**：做可交互 HTML Demo、代码 review、纯 API/技术文档撰写。审查完要改，`prd-review` 会把 findings 转交给 `prd-design`——它自己不动手改。

---

## 两个技能的分工

| 场景 | 用谁 |
|------|------|
| 从零 / 碎片输入生成 PRD | `prd-design`（生成后自带六维自检） |
| 审查别人写的、已有的 PRD | `prd-review`（只评审不重写） |
| 自己刚生成的 PRD 想再过一遍 | `prd-design` 阶段六自检 |
| 审查后要修复 | `prd-review` 出 findings → 转 `prd-design` 修 |

同一个六维框架：需求清晰度、可测试性、约束完整性、范围界限、一致性与闭环、开放问题与风险。差别只在**视角**——生成者回看自己的产出，评审者假装自己是第一次见这份文档的研发。

`prd-review` 另开了一个**中间地带**观察层，专门抓那些读起来流畅、但研发一动手就踩的问题：流程闭合、前置锁、数据生命周期唯一性、附录与正文一致性。实测这一层是高发区。

---

## 它和同类有什么不同

| 维度 | 常见做法 | 这套技能 |
|------|----------|----------|
| 输入处理 | 你给什么写什么 | 识别产品类型，对照行业补全库主动问缺的模块 |
| 生成之后 | 输出即终稿 | 六维质量门自检，critical/major 当场修 |
| 谁来评审 | 写的人自己看一遍 | 独立的评审技能，换陌生研发视角、换一双手 |
| 评审产出 | "建议优化一下验收标准" | 带四级严重性的 findings 清单，每条含位置+问题+修复方向 |
| 异常流 | 提一句"注意异常" | 每条异常流要求写到有终态，无终态判 critical |
| 依赖 | 多要一个 API Key | 纯 Markdown 工作流，零依赖零 Key |

差异化不在功能多，在**流程闭合**：生成 → 自检 → 外部评审 → 转交修复，环上每一段都有明确的交接物。

---

## 安全边界

这两个技能只做文档工作，不会：

- **不删你的东西**。不执行删除、不改写你的原始文档——评审只出 findings，修复要你明确授权才动。
- **不发外部请求**。核心流程不联网、不调用任何第三方 API、不需要 API Key。只有你主动要求做竞品分析时，才会用当前环境已有的搜索工具，并且结论必须带来源链接。
- **不动凭据**。不读写 `.env`、不碰密钥和 token。
- **不替你拍板**。行业补全项只标注建议，`[假设]` 处一律标出来等你回头修正——它不会把推断当事实写死。
- **不确定就问**。只在遇到致命业务矛盾（商业模式自相矛盾、主流程有死胡同）时停下问 1-3 个方向性问题，其余情况先给最合理方案再让你调整。

`prd-design` 在生成完整 PRD 前会**强制停一次**等你说"确认"——这是唯一的必经门，避免大返工。

---

## 文件结构

```
prd-skills/
├── skills/
│   ├── prd-design/                 # 生成技能
│   │   ├── SKILL.md                # 六阶段主流程
│   │   └── references/
│   │       ├── scenario-framework.md      # 阶段二：产品方向发散框架
│   │       ├── industry-completion.md     # 阶段三：分行业补全清单（电商/政务/医疗/B端SaaS/AI…）
│   │       ├── prd-template.md            # 阶段五：PRD 模板 + 正反例
│   │       ├── quality-gate.md            # 阶段六：六维质量门检查项
│   │       └── examples/                  # 真实产物样例
│   └── prd-review/                 # 评审技能
│       ├── SKILL.md                # 四步评审流程 + 严重性判定方法学
│       └── references/
│           ├── review-checklist.md        # 六维度完整检查项
│           └── examples/
└── .claude-plugin/marketplace.json # plugin 通道
```

参考文件平时不加载，只在对应阶段按需读取——装完之后不占你的上下文预算。

---

## 验证与测试

装完用这一句验收（不需要准备任何材料）：

```text
做一个给社区团购团长用的对账单功能，写成 PRD。
```

**合格表现**：

1. 它先给你一份**理解摘要**，把"材料明确给出的"和标了 `[假设]` 的推断分开列；
2. 它**识别出产品类型**并显式列出补全了哪些模块（对账单 → 至少该补：金额一致性校验、对账周期与截止规则、差异处理与申诉、历史对账查询、导出）；
3. 生成完整 PRD **之前停下等你确认**；
4. 输出的每个核心功能都带 Given/When/Then 形式的验收标准，没有"正常显示""体验流畅"这类词；
5. 结尾附一行六维自检结论。

再拿产出的 PRD 试一句：

```text
审查这份 PRD
```

**合格表现**：输出六维度通过数、按 critical/major/minor/suggestion 分级的 findings 清单（每条带位置和修复方向），以及一个可交付性结论。

---

## 致谢

六维质量门与分行业补全清单是在真实 PRD 工作里逐版打磨出来的（技能正文里标注的 v1.2 / v1.3 就是迭代痕迹）。**中间地带**观察层的检查项来自多次评审实测的反复踩坑。

配套技能：`prd-design` + `prd-review` 互为上下游。技能内部的负触发里提到的 `product-demo`、`skill-creator` 是可选的上游/下游工具，不装也不影响使用。

本仓库遵循 [Agent Skills](https://github.com/anthropics/skills) 规范，兼容 Claude Code / Codex / OpenCode / OpenClaw 等 Skill-compatible runtime。

## License

[MIT](LICENSE)

---

<div align="center">

*写的人和查的人，不该是同一个视角。*

</div>
