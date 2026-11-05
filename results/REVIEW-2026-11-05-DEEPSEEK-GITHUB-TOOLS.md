# 审核结论：DeepSeek 相关 GitHub 工具清单（2026-11-05）

- 待审文件：`results/deepseek-github-tools-2026-11-05.md`（Grok 提交，24 个项目）
- 审核人：老6（Muse）
- 审核方法：只读审核。逐个调用 GitHub 公开 API（`/repos/<owner>/<repo>` 与 `/license` 端点，含许可证原文开头）核对存在性、stars、最近 push、许可证、archived 状态。未 clone、未安装、未运行任何项目；安全判断仅依据 API 元数据、许可证原文与仓库公开描述。
- 审核时间：2026-11-06 00:50（CST）前后，API 实测值。

## 总览

| 结论 | 数量 | 项目 |
|---|---|---|
| 通过 | 13 | Scrapegraph-ai、gpt-researcher、crawl4ai、baoyu-design、open-codesign、deep-chat、open-design（nexu-io）、Awesome-DeepSeek-Harness-Plugins、awesome-deepseek-agent、awesome-deepseek-integration、ChatGPT-Shortcut、litellm、promptfoo |
| 待定 | 10 | firecrawl、dify、new-api、cc-switch、CowAgent、anbeime/skill、humanize-text、free-llm-intel、claw-compactor、opencompass |
| 不通过 | 1 | UncommonRoute |

**数据诚信总体印象：Grok 的数字没有造假，也没有夸大。** 24 个项目的 stars 与 API 实测差值全部在 0–9 之间，且 Grok 所报数字全部 ≤ 实测值（差异方向一致为 star 随时间自然增长），最近 push 日期 24/24 完全对得上，无一仓库不存在或被 archived。点名复核的四个「看起来虚高」的数字全部属实：

| 项目 | Grok 所报 | API 实测 | 差值 |
|---|---:|---:|---:|
| firecrawl/firecrawl | 198874 | 198883 | +9 |
| langgenius/dify | 160467 | 160469 | +2 |
| farion1231/cc-switch | 152088 | 152089 | +1 |
| nexu-io/open-design | 106081 | 106084 | +3 |

**清单真正的盲点不在数字，在许可证与用途**：Grok 未标注 dify 的修改版许可证附加条件（多租户需商用授权、不得移除 LOGO）、new-api 的 AGPL-3.0、CowAgent 需接入微信等账号登录态这三处，而 Top 5 第 1 名恰是 dify。firecrawl 的 AGPL Grok 在 Top 5 注释里提到了，这一点它做对了。

## 逐项结论

### 通过（13）

| 项目 | 核实后 stars / 许可证 | 理由 |
|---|---|---|
| ScrapeGraphAI/Scrapegraph-ai | 32532 / MIT | 数字属实；自托管 Python 爬取库，用自己的 LLM key，灵感采集场景对口。 |
| assafelovic/gpt-researcher | 30503 / Apache-2.0 | 数字属实；主流调研 agent，自托管、key 自持，调研简报场景对口。 |
| unclecode/crawl4ai | 86950 / Apache-2.0 | 数字属实；自托管免费、宽松许可证，同场景优于 AGPL 的 firecrawl，Top 5 入选合理。 |
| JimLiu/baoyu-design | 6127 / MIT（许可证原文已核：原作者作品部分为 MIT，第三方材料另有 NOTICE 条款） | 设计类 Skill，场景对口；Skill 形态，装前速览一遍 SKILL.md 即可。 |
| OpenCoworkAI/open-codesign | 8164 / MIT | 本地设计工具；注意 API key 只填本地版，不要用任何云端同步功能。 |
| OvidijusParsiunas/deep-chat | 3743 / MIT | 前端聊天组件，本身不经手凭据；给客户站点集成时 API key 必须走后端代理，禁止前端直连暴露。 |
| nexu-io/open-design | 106084 / Apache-2.0 | 数字属实（一度被怀疑虚高，实测更高）；本地优先设计工作区，场景对口。 |
| Zhiyuan-Fan/Awesome-DeepSeek-Harness-Plugins | 563 / MIT | 本身只是索引列表，无代码风险；注意：列表指向的每一个第三方插件仍须逐个审过才能装。 |
| deepseek-ai/awesome-deepseek-agent | 6395 / 无许可证文件 | DeepSeek 官方资源合集，内容为链接汇编，仅作选型阅读，不涉及代码复用，无许可证不构成问题。 |
| deepseek-ai/awesome-deepseek-integration | 39729 / CC0-1.0 | 官方集成列表，数字属实；最近 push 为 2026-02-23，时效一般，引用其中具体工具时另行核实其现状。 |
| rockbenben/ChatGPT-Shortcut | 8913 / MIT | 纯提示词库，零代码风险，中文友好，场景对口。 |
| BerriAI/litellm | 62774 / MIT（许可证原文已核：enterprise/ 目录另有企业许可，核心为 MIT） | 网关会经手全部 API key：**仅限自托管自用**，不要使用任何第三方托管的 litellm 实例；不要启用 enterprise/ 目录部分。 |
| promptfoo/promptfoo | 26875 / MIT | 本地评测/红队 CLI，数字属实（Grok 报 26872），可做交付前提示词回归测试。 |

### 待定（10）

| 项目 | 核实后 stars / 许可证 | 理由 |
|---|---|---|
| firecrawl/firecrawl | 198883 / **AGPL-3.0** | 数字属实、无造假；但 AGPL 用于接单交付或对外提供抓取服务会触发开源传染义务，云版则抓取目标与数据要过其服务器。同场景已有 Apache-2.0 的 crawl4ai 可替代。仅内部自用可考虑，不进客户交付链。 |
| langgenius/dify | 160469 / **修改版 Apache-2.0**（许可证原文已核） | 附加条件：① 用其源码运营多租户环境需另行取得商用许可；② 不得移除或修改控制台/应用中的 LOGO 与版权信息。**拿它给客户做白标交付会直接撞条款**；仅内部自用工作流不受影响。定为待定：内部试用可，客户交付前先解决授权与 LOGO 条款。Grok 清单对此只字未提，是最大盲点。 |
| QuantumNous/new-api | 51433 / **AGPL-3.0** | 自托管网关，经手全部上游 API key，自托管自用尚可控；但 AGPL 限制对外提供服务，且该项目业内常见用法是对外分销 API，会撞上游服务商 ToS。仅限自托管自用，禁止拿去给客户分销或交付。 |
| farion1231/cc-switch | 152089 / MIT | 数字属实；但它是桌面二进制分发，会集中读写 Claude Code / Codex / OpenCode 等多个 CLI 配置文件中的 API key，且带一键导入第三方 Skills 功能——凭据集中 + 技能供应链两个风险面。只装 GitHub 官方 Release，导入的每个 Skill 仍须逐个审。 |
| zhayujie/CowAgent | 47805 / MIT | 个人 agent 框架，需接入微信等即时通讯账号的登录态，并可在本机执行工具与技能：主力微信号有封禁风险，登录态托管本身也是风险。不要接主力账号；技能来源逐个审后再用。 |
| anbeime/skill | 9203 / **无许可证文件**（API 确认 Not Found） | 第三方技能包合集（技能商店）：每个技能包都是可执行指令 + 脚本，属供应链风险，且无许可证、复用权利不清。装任何一个之前必须逐个审内容，禁止批量安装。 |
| lynote-ai/humanize-text | 3482 / MIT | 代码风险低；但产品定位是「去 AI 味」规避 AI 检测，用在客户交付文案上属诚信灰区。非安全问题，是交付诚信问题：至多作润色参考，不作「过检」工具。 |
| rockbenben/free-llm-intel | 54 / MIT | 安全无明显问题，但仓库体量极小（54 stars）、内容只是 AI 厂商动态 RSS，与接单主线相关性低，非必需品。 |
| open-compress/claw-compactor | 2003 / MIT | 最近 push 为 2026-04-01，已 7 个月未更新；token 压缩收益无第三方验证，上手成本高、与主线关系弱。 |
| open-compass/opencompass | 7563 / Apache-2.0 | 正规学术评测套件，安全无问题；但对单人接单主线过重，相关性低。需要认真评测模型时再启用不迟。 |

### 不通过（1）

| 项目 | 核实后 stars / 许可证 | 理由 |
|---|---|---|
| CommonstackAI/UncommonRoute | 703 / MIT | 仓库存在、数字属实，**不是造假问题**。不通过原因：① 其宣称的「省 82% 成本 / 79.4% 准确率 / 93.4% 通过率」全部是厂商自报，无任何第三方验证，Grok 却据此给了「高」推荐度；② 路由层会经手全部 prompt 与 API key，仓库小众且自 2026-06-26 起 4 个多月未更新；③ 功能与已通过的 litellm 完全重叠且 litellm 更可控。风险收益不划算。 |

## 给子睿的三个重点提醒

1. **许可证坑比安全坑更实际**：dify（修改版 Apache：多租户要授权、不能去 LOGO）、firecrawl / new-api（AGPL）都直接决定能不能进客户交付链。内部自用都没问题，往客户那边交付前必须先过这一关。
2. **凭据与登录态只过自己的手**：litellm / new-api 这类网关经手全部 API key，只自托管；cc-switch 集中管理多个 CLI 的 key 配置，只装官方 Release；CowAgent 不要接主力微信号。
3. **技能包是供应链，不是应用商店**：anbeime/skill 这类合集在逐个审完内容之前不能装；单个知名作者的技能（如 baoyu-design）风险相对可控，但也先速览再装。

另：Grok 的 Top 5（dify、crawl4ai、litellm、baoyu-design、ChatGPT-Shortcut）里 4 个在通过名单内，dify 改为「内部试用通过、客户交付待定」，其余可按原计划推进，等子睿确认后再由 G哥 执行试用。
