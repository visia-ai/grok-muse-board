# 决定：DeepSeek GitHub 工具按老6审核结论执行（2026-11-06）

- 日期：2026-11-06（北京时间）
- 决定人：子睿（Steven），在 Grok Bot 对话中亲口确认「老6审核完放行了」，并授权此后类似事项不再让他本人过目
- 依据：results/REVIEW-2026-11-05-DEEPSEEK-GITHUB-TOOLS.md（老6 / Muse 只读审核）；待审清单 results/deepseek-github-tools-2026-11-05.md
- 执行人：G哥

## 流程（站着有效）

以后工具 / 插件 / skill 类事项：老6（Muse）审核放行后，G哥直接写入仓库并执行试用，不再要子睿本人确认。仅在花钱、登客户交付链、或跨过老6标明的约束（许可证、凭据、主力账号）时再问子睿。

## 本批结论

- 通过 13：ScrapeGraphAI/Scrapegraph-ai、assafelovic/gpt-researcher、unclecode/crawl4ai、JimLiu/baoyu-design、OpenCoworkAI/open-codesign、OvidijusParsiunas/deep-chat、nexu-io/open-design、Zhiyuan-Fan/Awesome-DeepSeek-Harness-Plugins（仅索引，第三方插件仍须逐个审）、deepseek-ai/awesome-deepseek-agent（仅阅读选型）、deepseek-ai/awesome-deepseek-integration（仅阅读选型）、rockbenben/ChatGPT-Shortcut、BerriAI/litellm（仅限自托管自用，不启用 enterprise/ 目录，不用第三方托管实例）、promptfoo/promptfoo
- 待定 10：firecrawl（AGPL，本批不装，用 crawl4ai 替代）、dify（修改版 Apache：内部自用可试，不用于客户白标交付；本批先不装）、new-api（AGPL，本批不装）、cc-switch、CowAgent、anbeime/skill、humanize-text、free-llm-intel、claw-compactor、opencompass
- 不通过 1：CommonstackAI/UncommonRoute（不装）

## 执行顺序

1. rockbenben/ChatGPT-Shortcut：提取展示站文案 / 调研提示词
2. JimLiu/baoyu-design：先读 SKILL.md 再决定是否接入
3. unclecode/crawl4ai：自托管抓取标杆站，服务灵感库
4. BerriAI/litellm：需要统一网关时再自托管安装

其余通过项按需启用。deep-chat 给客户站集成时 API key 必须走后端代理。open-codesign 的 API key 只填本地版。
