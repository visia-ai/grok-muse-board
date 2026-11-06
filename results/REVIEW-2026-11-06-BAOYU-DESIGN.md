# baoyu-design 安全与可用性审阅

- 仓库：https://github.com/JimLiu/baoyu-design
- 审阅日期：2026-11-06（America/Los_Angeles）
- 方式：只读拉取公开文件（SKILL / NOTICE / README / `skills/claude-design/scripts/*.mjs` 头部与静态扫描），**未安装** npm 包、未跑编辑器、未调 DeepSeek API

## 结论（建议接入：是，但轻量、只读接入）

**建议接入：是。**  
理由：入口 `SKILL.md` / `NOTICE.md` 未见「外传凭据、越权执行不明脚本、隐蔽 webhook」类指令；本地工具以 Node 脚本在本机起编辑器为主，对「有格调展示站」的落地页/画板/品牌视觉初稿有直接价值。但许可是**混合许可**（维护者工具 MIT，大量 Claude Design / Anthropic 材料**不适用 MIT**），且完整编辑器体量大——本轮只建议「按需 Read skill + 产出 HTML」，不要全局 `npx skills add` / 不要装全套 DeepSeek Harness 插件。

## 仓库现状（重要）

原路径 `skills/baoyu-design/SKILL.md` **已不存在**（404）。当前结构：

| 路径 | 角色 |
|------|------|
| `archive/baoyu-design/SKILL.md` | 旧版「单一 baoyu-design」编排入口（仍可读，适合理解流程） |
| `skills/claude-design/SKILL.md` | **现行**设计画布 / 落地页相关主 skill |
| `skills/claude-slides/` 等 | 幻灯片、设计系统、仪表盘、动效等拆分 skill |
| `NOTICE.md` | 许可与来源声明（必读） |
| `dsh-plugins/` / npm `dsh-claude-*` | DeepSeek Harness 插件形态（本轮不装） |

## SKILL.md 审阅要点（archive + claude-design）

审阅对象：

1. `archive/baoyu-design/SKILL.md`（约 11KB）
2. `skills/claude-design/SKILL.md`（约 31KB）
3. 根目录 `NOTICE.md`
4. `skills/claude-design/scripts/artifact.mjs`、`server.mjs`（静态扫描，未执行）

### 未发现的风险类型

- 无「把 API key / 密码 / token 发给外部」的指令
- 无要求忽略安全策略、外传对话内容的隐蔽 jailbreak
- 无要求执行不明远程 shell one-liner / 下载不明二进制后执行
- `package.json` 几乎为空壳（依赖不在根安装）；编辑器脚本自述 prebuilt、本地运行

### 已知/可接受风险（需知情）

1. **混合许可**：`NOTICE.md` 明确 Anthropic 来源的系统提示词、内置技能、编辑器还原件等**不是 MIT**；商用展示站交付时，对外分发/再发布 Anthropic 原文材料要谨慎，优先把**我们自己生成的 HTML 交付物**当产物，而不是整仓再分发。
2. **本地 HTTP 服务 + 可选局域网分享**：`server.mjs` / `artifact.mjs share` 会在本机（及可选 LAN）起服务，URL 含角色 token。风险是**内网误分享设计稿**，不是外传云端密钥。默认应只绑 `127.0.0.1`，不要对公网暴露。
3. **会拉公开 CDN 资源**：文档/产物侧常见 `unpkg.com`（React/Babel，带 SRI）与 `fonts.googleapis.com`。离线或国内网络需改成本地资源。
4. **`child_process.spawn`**：用于启动本地编辑器/相关子进程，符合「本机工具」预期；本轮未实际执行，后续若运行应在隔离目录。
5. **体量大**：全量 skill + editor 不适合在积分紧张时整仓装进 Agent；应「任务触发时 Read 对应 SKILL.md」而不是常驻全量。

### archive `SKILL.md` 在干什么

纯 Markdown 编排：读 `system-prompt.md` → 识别 Cursor/Claude/Codex harness → 按需加载 `built-in-skills/*` → 询问澄清问题 → 在 `designs/<project>/` 写自包含 HTML → 本地 HTTP 预览。无外传凭据逻辑。

## 能不能接入 / 干什么 / 怎么用 / 风险

| 项 | 内容 |
|----|------|
| **能不能接入** | 能（轻量）。SKILL 文本干净可用；**本轮不执行安装器**。 |
| **干什么** | 为展示站出高保真 HTML 画板/落地页初稿、品牌视觉方向、简单交互原型；也可配合 slides skill 做提案 Deck。 |
| **怎么用（轻量）** | 需要视觉稿时，让 Agent：`Read https://raw.githubusercontent.com/JimLiu/baoyu-design/main/skills/claude-design/SKILL.md`（或 archive 入口），再按其中流程在沙箱 `designs/` 写 HTML。展示站默认路由：landing / hi-fi UI。 |
| **不要怎么用** | 不要本轮 `npx skills add JimLiu/baoyu-design`；不要装 `dsh-claude-*`；不要 `artifact.mjs share` 到公网；不要把 Anthropic 原文 skill 整包再发布到客户仓库当「自有代码」。 |
| **风险摘要** | 许可混合 > 局域网误分享 > CDN 依赖；未见凭据外传。 |

## 使用要点摘录（干净可用）

1. **触发时机**：真正要「做/改视觉交付物」时再加载；纯讨论架构不必提前加载。
2. **产物位置**：默认 `designs/<项目名>/`，自包含 HTML + 资源，避免散落到仓库根。
3. **先问清**：品牌参考、保真度、要几版方案；没有设计上下文时主动要参考（竞品站、色板、摄影风格）。
4. **预览**：用本地 HTTP 打开，不要依赖 `file://`（多文件原型会挂）。
5. **与我们业务的契合点**：`website / landing page`、品牌 tokens、相册式画板——正对民宿/咖啡馆/花艺/摄影/婚礼场地展示站。

## 本轮明确未做

- 未 `npm install` / 未 `npx skills add`
- 未启动 `artifact.mjs` / `server.mjs`
- 未调用任何模型 API
- 未写入 GitHub 仓库

## 建议的下一步（给父代理，非本子代理执行）

1. 把本审阅与 ChatGPT-Shortcut 精选一并记入 muse-board（由父代理提交）。
2. 下次真实客户视觉任务时，再 Recursively Read `skills/claude-design/SKILL.md` + 所需 `built-in-skills`，在沙箱出 1 页落地页 HTML 做验收。
3. 若必须装 DeepSeek Harness 插件，另开安全与积分评估，不在本轮。
