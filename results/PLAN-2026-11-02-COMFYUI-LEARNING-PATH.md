# PLAN-2026-11-02：ComfyUI 学习路径（面向「AI IP 账号 · 批量生成带货视频」）

- 对象：Steven（子睿）
- 内容日期：2026-11-02（调研于 2026-11-01 PT 完成）
- 主线：用 ComfyUI 工作流批量生成垂直品类（先做宠物用品）带货短视频，发抖音 / TikTok 赚分佣，不碰库存和发货
- 标记说明：**【推断】** = 根据资料做的判断或建议，不是原文结论；**【未核实】** = 没能直接验证的信息
- 所有链接于 2026-11-01 用 curl/WebFetch 检查过。B 站视频页对自动请求返回 412（反爬），能打开但无法自动读取 UP 主信息，表格里已单独标出

---

## 0. 一句话结论

**别从头看完一套 40 集课程。** 先装好环境（秋叶整合包或官方 Comfy Desktop 二选一），然后在每个阶段打开**官方模板 / 现成工作流**，跑通一次，再改出自己的版本。秋叶的价值主要在**国内网络下好装的整合包和启动器**，教程只挑「产品精修 / 产品替换 / 打光」这几集看。约 **25 个学习日（每天 3–4 小时，大约 4 周）** 可以做出「CSV 商品清单 → 批量产品场景图 → 批量图生视频 → 拼接字幕配音 → 导出成片」的流水线。

---

## 1. 学习目标（验收标准）

学完后能独立做到：

1. 在本地或云端稳定运行 ComfyUI，会装节点和模型、更新版本、排查缺节点或爆显存的问题。
2. 把一张**商家提供的真实商品图**处理成多张场景图：抠图、换背景、重新打光、局部重绘，并且**商品外观、颜色、尺寸、文字都不变**。
3. 用图生视频把场景图做成 5–10 秒竖屏镜头，商品不变形、不乱动。
4. 用 Python 脚本或 comfy-cli 读一份商品清单（CSV），一次排队生成 20 条以上视频素材，再用 ffmpeg 拼接、加字幕和配音。
5. 每条成片都能通过「平台合规检查清单」（第 5 节）。

---

## 2. 核心资源（已验证）

### 2.1 秋叶（秋葉aaaki）：用来装环境，不必全看

| 资源 | 链接 | 说明 |
|---|---|---|
| 秋葉aaaki B 站主页 | https://space.bilibili.com/12566101 | 已用 B 站公开接口确认 mid=12566101，昵称「秋葉aaaki」，约 180 万粉 |
| 整合包更新公告（2026.8，v3.2） | https://www.bilibili.com/opus/1233418589167419397 | 公告写明：ComfyUI v0.30.2 / Python 3.13 / torch 2.13+cu130；夸克网盘 `pan.quark.cn/s/64b808baa960`，解压密码 `bilibili-秋葉aaaki` |
| 整合包更新公告（2026.1） | https://www.bilibili.com/opus/1159516886456598528 | 作者说明整合包刻意只预装少量节点，「越干净越好」 |
| 启动器使用说明（ComfyUI Wiki） | https://comfyui-wiki.com/en/install/install-comfyui/aaaki-comfyui-launcher-user-guide | 启动器有国内镜像，能装插件；Wiki 同时推荐官方 Comfy Desktop |
| 「秋叶ComfyUI V10 中文整合包」合集（含第 12 节产品精修、14 节产品&模特打光、15 节透明背景、16 节产品替换） | https://www.bilibili.com/video/BV1YQj66mEJh/ | **【未核实】UP 主是否为秋葉aaaki 本人**。B 站上大量「秋叶 xx 教程」是第三方借名的引流号。这一集合的产品相关几集可以挑着看 |

**定位【推断】**：秋叶 = 国内 Windows + N 卡用户的「一键环境」。他本人的公开内容以整合包发布和单点新模型介绍为主，不是成体系的电商课。所以用他的整合包，教程只挑和产品有关的几集。另外，B 站搜「秋叶 ComfyUI」会出现大量标题党或擦边引流视频，直接跳过。

### 2.2 其他 B 站 / 社区资源

| UP 主 / 来源 | 资源 | 链接 | 用途 |
|---|---|---|---|
| -Zho-（ZHO） | B 站主页（接口确认 mid=484366804） | https://space.bilibili.com/484366804 | 原理、生态梳理和新模型第一时间上手，适合进阶时补原理 |
| ZHO | 工作流合集 GitHub | https://github.com/ZHO-ZHO-ZHO/ComfyUI-Workflows-ZHO | 现成工作流，可以照着拆 |
| 第三方合集（UP 主未核实） | 「ComfyUI 工作流从零基础到精通（2026）」：Z-Image-Turbo 出图、Qwen-Image-Edit-2511、Wan2.2 i2v、首尾帧、BiRefNet、Segment Anything | https://www.bilibili.com/video/BV1yc5R6wEb2/ | **选集最贴近当前模型**，按需看第 2/3/6/8/10/11 集和「实用节点 9–11」 |
| 第三方合集（UP 主未核实） | 「2026 最全面 ComfyUI 教程」：含 Flux 电商产品场景、IC-Light 光效、万物迁移（Redux）、电商精修 | https://www.bilibili.com/video/BV14KreBrEYY/ | 只看「实战」部分的电商相关几集 |

> 说明：两个第三方合集的「播放量 / 口碑」没有可靠数据，不作排名。它们之所以列进来，是因为选集内容和本项目直接相关。

### 2.3 官方文档与模板（主线资源）

| 资源 | 链接 |
|---|---|
| 官方文档首页 / 中文版 | https://docs.comfy.org/ ・ https://docs.comfy.org/zh-CN/ |
| 系统要求 | https://docs.comfy.org/installation/system_requirements |
| Comfy Desktop（Windows / macOS） | https://docs.comfy.org/installation/desktop/windows ・ https://docs.comfy.org/installation/desktop/macos ・ 下载 https://www.comfy.org/download |
| Windows 便携版 | https://docs.comfy.org/installation/comfyui_portable_windows |
| 更新 ComfyUI | https://docs.comfy.org/installation/update_comfyui |
| 第一次出图 | https://docs.comfy.org/get_started/first_generation |
| 工作流模板（Templates 面板） | https://docs.comfy.org/interface/features/template |
| ComfyUI-Manager | https://docs.comfy.org/manager/install ・ https://github.com/Comfy-Org/ComfyUI-Manager |
| 官方示例（老但经典，理解节点原理） | https://comfyanonymous.github.io/ComfyUI_examples/ |
| 官方工作流 Hub（含「产品 mockup」「换材质」等） | https://comfy.org/hub/workflows/ |
| 基础：文生图 / 局部重绘 / 放大 | https://docs.comfy.org/tutorials/basic/text-to-image ・ https://docs.comfy.org/tutorials/basic/inpaint ・ https://docs.comfy.org/tutorials/basic/upscale |

### 2.4 当前推荐模型（截至 2026-11，按用途分）

| 用途 | 推荐 | 官方教程 | 备注 |
|---|---|---|---|
| 文生图（快、中英文字准） | Z-Image-Turbo（6B，8 步） | https://docs.comfy.org/tutorials/image/z-image/z-image-turbo | 文档称可在 16GB 显存运行 |
| 文生图 + 编辑（最快） | FLUX.2 Klein 4B | https://docs.comfy.org/tutorials/flux/flux-2-klein | 文档：蒸馏版约 8.4GB 显存，Base 版约 9.2GB |
| 文生图（高质量） | FLUX.2 Dev | https://docs.comfy.org/tutorials/flux/flux-2-dev | 显存要求更高；**商用许可需自查** |
| **商品图编辑（保商品原样）** | Qwen-Image-Edit-2511 | https://docs.comfy.org/tutorials/image/qwen/qwen-image-edit-2511 | 文档强调一致性和「产品设计 / 材质替换」。一位开发者在 16GB 卡上实测认为它在**保留商品图案和文字**上最可靠：https://github.com/mln330/local-image-model-lab |
| 换背景 / 指令编辑 | FLUX.1 Kontext（Dev 本地；Pro/Max 走 API 节点） | https://docs.comfy.org/tutorials/flux/flux-1-kontext-dev ・ https://docs.comfy.org/tutorials/partner-nodes/black-forest-labs/flux-1-kontext | 文档明确支持换背景 |
| 抠图 | RMBG-2.0 / BiRefNet 节点 | https://github.com/1038lab/ComfyUI-RMBG | |
| 重新打光 | IC-Light（SD1.5 体系） | https://github.com/kijai/ComfyUI-IC-Light | 搭配 KJNodes：https://github.com/kijai/ComfyUI-KJNodes |
| 参考图风格 / 一致性 | IPAdapter Plus | https://github.com/cubiq/ComfyUI_IPAdapter_plus | 主要用在 SDXL 体系，新模型多半直接用编辑模型 |
| 现成「商品换背景」工作流 | FLUX.2 Klein + RMBG-2.0，分 16GB / 32GB 两版 | https://github.com/MattAIstudio/-2026WK2701-ComfyUI-Workflow---Product-Background-Swap | 个人仓库，0 star，**仅作拆解参考** |
| **图生视频（本地开源，入门）** | Wan2.2 TI2V 5B | https://docs.comfy.org/tutorials/video/wan/wan2_2 | 文档称 5B 工作流配合原生 offload 可在 **8GB 显存**运行；Apache-2.0 |
| 图生视频（本地，高质量） | Wan2.2 A14B（I2V / 首尾帧） | 同上 ・ 权重 https://huggingface.co/Comfy-Org/Wan-2.2 | 显存需求大，**【推断】24GB 以上更舒服，或上云** |
| 图生视频 + 同步音频 / 多镜头 | LTX-2.5 | https://docs.comfy.org/tutorials/video/ltx/ltx-2-5 ・ https://comfy.org/ltx-2.5/ ・ https://github.com/Lightricks/ComfyUI-LTXVideo | 22B 模型，有蒸馏 / int8 版；第三方文章称年营收低于 1000 万美元的组织可免费用（**【未核实】以官方许可为准**） |
| 带音频的开源视频模型 | MiniMax H3 | https://docs.comfy.org/tutorials/video/minimax/minimax-h3 | 第三方称其社区许可**不覆盖美国 / 欧盟 / 英国 / 韩国**（**【未核实】**）。如果要发 TikTok 美区，先查许可 |
| 闭源 API（按次付费，无需显卡） | Wan2.6 / 2.7 i2v、LTX-2.5 Fast/Pro、Kontext Pro 等 | https://docs.comfy.org/built-in-nodes/WanImageToVideoApi | 在 ComfyUI 内用 Partner/API 节点调用 |
| 数字人对口型 | Wan2.1 InfiniteTalk（原生节点） | https://docs.comfy.org/tutorials/video/wan/wan-infinitetalk | 仅当 IP 账号需要「虚拟讲解员」时使用。**抖音电商要求虚拟主播先注册并通过审核**（见第 5 节） |
| 中文 TTS / 克隆音色 | IndexTTS 节点 | https://github.com/chenpipi0807/ComfyUI-Index-TTS | 只用自有或已授权音色，禁止仿冒他人声音 |
| 视频读写 / 批量读图 | VideoHelperSuite | https://github.com/Kosinkadink/ComfyUI-VideoHelperSuite | 必装 |
| 插帧 | Frame-Interpolation（RIFE / FILM） | https://github.com/Fannovel16/ComfyUI-Frame-Interpolation | 16fps → 32fps 更顺 |
| 视频放大 | SeedVR2 | https://github.com/numz/ComfyUI-SeedVR2_VideoUpscaler ・ 官方视频放大页 https://docs.comfy.org/tutorials/utility/video-upscale | |
| Wan 高级用法（社区） | kijai WanVideoWrapper | https://github.com/kijai/ComfyUI-WanVideoWrapper | 原生节点不够用时再看 |

### 2.5 批量自动化

| 资源 | 链接 | 用途 |
|---|---|---|
| ComfyUI 服务端 API 示例（导出 API 格式 JSON → POST /prompt） | https://docs.comfy.org/development/comfyui-server/api-examples | **主线**：Python 改 JSON 里的节点输入后循环提交 |
| 服务端路由（/prompt、/history、WebSocket） | https://docs.comfy.org/development/comfyui-server/comms_routes | 轮询任务完成状态 |
| 运行工作流总览 / Comfy SDK（Python、TS） | https://docs.comfy.org/development/run-workflows/overview ・ https://docs.comfy.org/development/api-development/sdks | 官方 SDK：本地和 Comfy Cloud 用同一套代码 |
| comfy-cli | https://github.com/Comfy-Org/comfy-cli ・ https://docs.comfy.org/comfy-cli/getting-started ・ https://docs.comfy.org/agent-tools/cli | 命令行安装、`comfy run` 跑工作流 |
| ComfyScript（用 Python 写工作流） | https://github.com/Chaoses-Ib/ComfyScript | 可选，适合喜欢写代码的人 |
| 节点大全 | https://github.com/ComfyUI-Workflow/awesome-comfyui | 按需搜节点，不要通读 |

---

## 3. 分阶段计划表

> 节奏按每天 3–4 小时估算。原则：**每阶段先跑通官方模板 → 换成自己的商品图 → 存一份「我的版本」JSON**。

| 阶段 | 学什么 | 资源（按顺序） | 预计天数 | 验收产出 |
|---|---|---|---|---|
| **0. 环境安装与基础节点** | 选安装方式：国内 Win+N 卡用秋叶整合包，其他情况用 Comfy Desktop；显存不足直接上云（第 4 节）。学会 Manager 装节点、模型放哪个目录、更新、看报错日志；认识 Load Checkpoint / CLIP Text Encode / KSampler / VAE Decode / Save Image | 秋叶整合包公告 → 启动器说明（Wiki）或 Desktop 安装文档 → 第一次出图 → Manager 文档 → 官方示例首页（只看 txt2img / img2img 两页） | 2 | 截图：能出图；能装 VideoHelperSuite、RMBG、KJNodes；写一页「模型目录速查」笔记 |
| **1. 出图工作流** | 文生图、图生图、局部重绘、放大；提示词结构；seed 和 batch 的含义；竖屏 9:16 分辨率 | 官方 text-to-image / inpaint / upscale 教程 → Z-Image-Turbo 或 FLUX.2 Klein 模板 → 第三方合集 BV1yc5R6wEb2 第 2/3 集（按需） | 3 | 用模板出 20 张「宠物用品使用场景」空镜图（不含商品），存成 `t2i_scene_v1.json` |
| **2. 产品图处理（最关键）** | 抠图 → 换背景 / 合成 → 重新打光 → 局部修复；**保护商品像素**：用遮罩，最后把原商品层叠回去 | RMBG 节点 → Qwen-Image-Edit-2511 模板 → FLUX.1 Kontext Dev 模板 → IC-Light（kijai）→ MattAIstudio 换背景工作流（拆解）→ 秋叶合集第 12/14/15/16 集（UP 主未核实）→ BV14KreBrEYY「电商产品场景 / IC-Light / 万物迁移」几集 | 5 | 3 个真实宠物商品，每个 5 张场景图。逐张对照商品详情页检查**颜色、尺寸比例、Logo/文字不变**，存成 `product_scene_v1.json` |
| **3. 图生视频** | Wan2.2 5B I2V（本地入门）→ 首尾帧 → 运镜提示词（推拉摇移，只让镜头和环境动，商品不动）→ LTX-2.5 或 API 节点对比 → 插帧、放大 | Wan2.2 官方教程 → BV1yc5R6wEb2 第 10/11 集 → LTX-2.5 教程 → WanImageToVideoApi 文档 → Frame-Interpolation / SeedVR2 | 5 | 每个商品 3 条 5 秒竖屏镜头，共 9 条；记录每条耗时、显存 / 云成本、合格率（商品不变形算合格）；存成 `i2v_v1.json` |
| **4. 成片组装（配音、字幕、拼接）** | 3–4 个镜头 + 实拍或商家素材 → 15–30 秒成片；TTS 配音（自有音色）；字幕；授权 BGM；**画面内加「AI 生成」字样** | IndexTTS 节点 → VideoHelperSuite 合成 → ffmpeg 拼接和字幕（【推断】成片拼接用 ffmpeg 比在 ComfyUI 里做更稳） | 3 | 3 条完整成片，过一遍第 5 节合规清单 |
| **5. 批量自动化** | 「Export (API)」导出 JSON；Python 读 `products.csv`（商品 ID、图路径、卖点、场景提示词），替换 LoadImage 和提示词节点后提交 /prompt，轮询 /history；失败重试；输出按商品分文件夹；可选 comfy-cli / Comfy SDK | API 示例 → 服务端路由 → comfy-cli / SDK 文档 | 4 | 一条命令：10 个商品 × 2 场景 × 2 镜头 = 40 个镜头自动生成，再自动拼成 10 条成片；附生成日志 CSV（耗时、成本、失败原因） |
| **6. 接入带货场景** | 选品 → 素材授权 → 批量出片 → 人工质检 → 带 AI 标识发布 → 数据回填；迭代提示词模板 | 第 5 节；平台规则链接见第 5 节 | 3 | 一套 SOP 文档、质检清单、第一批 20–30 条待发视频 |
| **合计** | | | **≈25 天** | |

**提效建议【推断】**
- **复用胜过从零搭**：每阶段先从 Templates 面板或现成 JSON 开始，只改输入和参数；读懂别人的工作流，比看 20 集教程更快。
- **只学一套主力组合**：出图选 Z-Image-Turbo 或 Klein 其一；编辑用 Qwen-Image-Edit-2511；视频用 Wan2.2（本地）或 API 节点（云）。别同时追 5 个新模型。
- **视频里少让商品本身「动」**：平台禁止用 AI 让静态商品不真实地动起来或变形（见第 5 节）。镜头运动和环境变化最安全。
- **边学边记成本**：每张图、每秒视频花了多少钱（或显卡时长），决定后面用本地、云还是 API。
- 卡住就去 ComfyUI 文档的 troubleshooting 页，或查节点 GitHub 的 issues，别反复刷视频找答案。

---

## 4. 硬件与云端方案

| 情况 | 建议 | 链接 |
|---|---|---|
| N 卡 ≥16GB（如 4080/5070Ti/5080/4090/5090） | 本地全流程：Klein/Z-Image 出图、Qwen-Edit（fp8 / 量化）、Wan2.2 5B；A14B 和 LTX-2.5 用量化或 offload | 系统要求：https://docs.comfy.org/installation/system_requirements |
| N 卡 8–12GB | 本地学习够用（Klein 约 8.4GB；Wan2.2 5B 官方称 8GB 可跑），重任务上云 | 同上 |
| 无独显 / Mac / <8GB | 直接用云 | 见下 |
| RunningHub（国内 / 国际站） | 预装节点和模型，开箱即用，社区工作流多，适合**学习期和照抄模板** | https://www.runninghub.cn/ ・ https://www.runninghub.ai/ |
| AutoDL | 国内按小时租 GPU，可用社区 ComfyUI 镜像，需自己管理环境，适合**批量量产** | https://www.autodl.com/ |
| RunPod | 海外按小时租 GPU，有 ComfyUI 模板 | https://www.runpod.io/ |
| Comfy Cloud（官方） | 官方托管，订阅制，可配合 Comfy SDK 批量调用；文档称支持支付宝（美元结算） | https://docs.comfy.org/get_started/cloud ・ 价格 https://comfy.org/pricing/ |
| 平台对比（第三方） | RunPod vs RunningHub；17 个 ComfyUI 云平台对比 | https://myaiforce.com/runpod-vs-runninghub/ ・ https://xiangyugongzuoliu.com/comfyui-cloud-platform-guide/ |

**建议【推断】**：学习期（阶段 0–3）用 RunningHub 或本地，阶段 5 起批量生产用 AutoDL、RunPod 或 Comfy Cloud 跑自己的 API 脚本。具体价格变化快，以各平台价格页为准，本文不列单价。

---

## 5. 学完后第一批视频怎么发（衔接建议，只讲合规做法）

### 5.1 先看清平台规则（直接影响工作流设计）

**抖音**
- 国家《人工智能生成合成内容标识办法》（2025-09-01 施行）要求显式标识加元数据隐式标识，禁止删除、篡改标识：https://www.cac.gov.cn/2025-03/14/c_1743654684782215.htm
- 抖音 AI 内容标识：发布页依次点「高级设置 → 发文助手自主声明 → 内容由 AI 生成」；未声明的作品，平台检测到后会自动加「疑似使用了 AI 生成技术」：https://new.qq.com/rain/a/20250901A059O000 （平台规则更新见 https://95152.douyin.com/ ）
- 抖音电商 AIGC 管理规则（2023 年发布，以规则中心现行版为准）：发布 AI 内容要实名，视频页要显著标识；**虚拟主播须先在平台注册并通过审核才能出镜**；使用第三方形象或商标须获授权；内容须真实，不得虚假宣传：https://www.taofake.com.cn/article/douyin/75309.html （转载全文）
- 2025-09 抖音电商专项治理点名的违规：仿冒名人声音带货、**AI 生成与实物不符的商品展示**、**AI 虚构测评和体验**、未声明 AI：https://news.qq.com/rain/a/20250904A04FD300
- 《直播电商监督管理办法》2026-02-01 起施行，数字人主播须持续标识：https://www.samr.gov.cn/xw/mtjj/art/2026/art_b71cec6e11384080b91a779eb0bf7712.html

**TikTok / TikTok Shop（美区）**
- TikTok Shop《AI-Generated Content Restrictions and Requirements》（2026-09-02 版）：https://seller-us.tiktok.com/university/essay?knowledge_id=491489038501663&lang=en ，要点：
  - 完全或大幅用 AI 生成的内容须打开「AI-generated content」开关，或在画面 / 文案中注明
  - **不得用 AI 改变商品的尺寸、颜色、材质、数量、功能**
  - **不得把静态商品做成不真实的动画，不得把平面商品做成假 3D 或旋转展示**，不得添加商品本身没有的发光等效果
  - 允许的用法：换背景、调光调色、生成虚拟场景演示用法（须与实物一致）、AI 翻译和配音
  - 禁止虚构专家、医生身份背书
- TikTok 帮助中心 AIGC 标签：https://support.tiktok.com/en/using-tiktok/creating-videos/ai-generated-content ・ 社区准则「真实性」：https://www.tiktok.com/community-guidelines/en/integrity-authenticity

> **对工作流的直接影响【推断】**：「全 AI 生成商品」的路线风险很高。正确做法是**商品主体保持真实**（商家图或自己的实拍），AI 只负责场景、光线、镜头运动、配音和字幕。阶段 2 的遮罩保护和原图回贴、阶段 3 的「镜头动、商品不动」就是为此设计的。

### 5.2 带货权限门槛

| 平台 | 门槛（以 App 内显示为准） | 来源 |
|---|---|---|
| 抖音（个人号） | 实名认证、年满 18 岁；按 2025-04-17 修订规范，有效粉丝 <500 时只能开橱窗，≥500 可开视频带货（直播和图文另有门槛）；支持「0 元入驻」试运营，保证金之后补缴；连续 180 天无成交会被释放权限 | 规范征求意见稿全文：http://www.100ec.cn/detail--6648568.html ・ 精选联盟规则：https://school.jinritemai.com/doudian/web/articlev0/101665 ・ 学习中心：https://school.jinritemai.com/doudian/web/home |
| TikTok Shop 美区 Affiliate | 年满 18 岁、**身在美国**、≥1,000 粉、通过身份验证；<5,000 粉先进入至少 30 天试点期：每天最多 3 条带货视频，只能选店铺评分 ≥95% 的商品；全体每天带货短视频上限 30 条（2026-05-11 起） | https://seller-us.tiktok.com/university/essay?knowledge_id=6939143037667118&lang=en |

> 注意【推断】：TikTok Shop 美区要求创作者「based in the United States」。如果 Steven 不在美国，先从抖音精选联盟起步，或者确认自己所在地区的 TikTok Shop 是否开放 Affiliate 和相应门槛。

### 5.3 第一批视频执行方案【推断，建议值】

1. **账号**：每个平台一个宠物用品垂直账号，实名，简介写明「AI 辅助制作的宠物好物内容」。不买粉，不搬运别人的视频。
2. **涨粉期（达到带货门槛前）**：发不挂车的合规内容，比如宠物用品使用场景、选购知识科普，全部开启 AI 标识。粉丝靠真实内容积累。
3. **选品**（精选联盟或 TikTok Shop 商品库）：
   - 优先选商家提供素材或允许使用商品图、店铺评分高、评价真实、退货率低的日用宠物用品，比如牵引绳、食盆、猫抓板、梳子、收纳类
   - 避开需要功效宣称的品类，比如药品、保健品、驱虫等，容易涉及虚假功效
   - 能申样就申样，补几段实拍，最稳妥
4. **第一批规模**：5 个商品 × 4 条 = 20 条。每天发 1–2 条，抖音和 TikTok 各自做原生版本，前 2 周不追量。TikTok 试点期本来就限制每天最多 3 条带货视频。
5. **每条发布前质检**：
   - 商品外观、颜色、尺寸、配件、文字和详情页一致
   - 没有夸大效果，没有「我家狗用了三个月」这类**虚构亲身体验**
   - 不出现他人肖像或声音，没有未授权商标
   - 已开启平台 AI 标识，画面角落有「AI 生成」字样
   - 数字人出镜前，确认已完成平台虚拟主播注册（抖音）
6. **每条记录的数据**（建表，每周复盘）：

   | 类别 | 字段 |
   |---|---|
   | 内容 | 商品 ID、视频 ID、发布时间、使用的工作流版本 / 提示词模板 / 模型 |
   | 流量 | 播放、完播率、平均观看时长、点赞 / 评论 / 分享 / 收藏、涨粉 |
   | 转化 | 商品点击率、成交单数、GMV、预估佣金、退货 / 退款数 |
   | 合规 | 是否被平台加「疑似 AI」标签、违规 / 警告记录、创作者健康分（TikTok CHR）/ 抖音信用分 |
   | 成本 | 单条生成耗时、GPU 或 API 花费、人工质检时间 |

7. **迭代**：2 周后保留数据前 20% 的「场景 + 运镜 + 开头 3 秒」模板，固化进批量脚本的 CSV 字段，再放大到 5–10 条 / 天（不超过平台上限）。

---

## 6. 未能核实或需注意的事项

- B 站视频页对自动请求返回 412：BV1YQj66mEJh、BV1yc5R6wEb2、BV14KreBrEYY 这三个合集的**UP 主身份未核实**，「秋叶」字样可能是第三方借名。只确认了秋葉aaaki（mid 12566101）和 -Zho-（mid 484366804）两个主页。
- 抖音带货门槛引用的是 2025-04 修订规范的征求意见稿全文，原文对图文和直播的粉丝数写法有前后不一致，**以抖音 App「电商带货 → 权限」页面为准**。
- 抖音电商 AIGC 管理规则引用的是 2023 年版本的转载，现行版请在抖音电商规则中心核对。
- LTX-2.5、MiniMax H3、FLUX.2 系列的商用许可范围来自第三方文章，**商用前请读官方 LICENSE**。Wan2.2 为 Apache-2.0。
- 显存数字只取官方文档明确写出的（Klein 约 8.4GB、Wan2.2 5B 约 8GB、Z-Image-Turbo 16GB）；A14B 和 LTX-2.5 的显存建议是推断。
- 云平台价格变化快，本文不列单价。
- X（Twitter）上没搜到有价值的「ComfyUI 带货工作流」帖子，未引用。
