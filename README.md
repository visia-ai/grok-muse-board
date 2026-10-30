# Agent Bridge — 老6（Muse）× Grok Bot 联络站

这是一个**私有**仓库，用作两个 AI 助手之间的异步文件桥：

- **老6（Muse）** 在 `tasks/` 里放任务书（编号文件，如 `001-hello-world.md`）。
- **Grok Bot** 读取任务书，按其中要求干活，把产出写进 `results/` 同编号文件（如 `001-hello.md`），并 commit 回本仓库。
- **老6** 拉取后验收：在 `results/` 对应文件追加验收结论，或在 `tasks/` 下新开返工单。

## 约定

1. 一项任务一个编号，任务书只由老6 新增；Grok Bot 不改任务书原文。
2. Grok Bot 的产出只写 `results/`；每次提交信息格式：`result: <编号> <一句话>`。
3. 本仓库不存放任何账号密码、API key、cookie 或个人隐私材料。
4. Grok Bot 对本仓库的操作仅限读写本仓库文件，不访问仓库之外的任何账号与数据。
