---
type: task
owner: datastudio-diff
author: claude-code/upwork
updated: 2026-11-21
sources:
  - "human/tom 2026-11-21：改落地页为 $19，First 50 buyers 删掉"
confidence: confirmed
status: active
---
# 任务卡：落地页买断价改为 $19，删掉前 50 名

- 派出：2026-11-21，Claude Code
- 仓库：datastudio-diff（TomXiaoYZ/datastudio-diff，main 直接发布 GitHub Pages）
- 工作树：仓库根下的 .worktrees/price-19
- 分支：task/price-19（基于 main，起点提交 cbe76bbf26fadb2e4d0771b7ca16e004a4e4ee0a）
- 集成目标：PR 到 main，由 Tom 当次批准后合并
- Codex 线程：派出时登记

最多两轮。本仓库只有 `index.html` 和 `README.md` 两个文件，`README.md` 即交接说明，不另做交接文档。

## 目标

Tom 把 Report Diff 的买断价从 $79 改为 $19，并取消“前 50 名”限量。落地页的定价段要和扩展、商店文案一致：
访客看到月付 $9、买断 $19，不再看到 first 50 users 的说法。页面其他内容一字不动。

## 授权

- 网络：否
- 额外可写目录：无
- MCP：关
- 模型：默认
- 强度：默认

## 输入清单

| 文件或数据 | 绝对路径 | 在工作树里？ | 用途 |
| --- | --- | --- | --- |
| 落地页 | index.html（标题 Planned pricing 下 class 为 price 的那一段） | 是 | 改这一句 |
| 说明 | README.md | 是（只读） | 了解仓库 |

## 人工前置条件

无。

## 用户路径

访客打开落地页，`Planned pricing` 下读到：`Early access: $9 per user per month, or $19 lifetime. Nothing to pay now; the waitlist is free.`
（`$9 per user per month` 和 `$19 lifetime` 仍各自加粗，与现在的写法一致。）

## 验收标准

1. `grep -c '\$79' index.html` 为 0；`grep -c -i 'first 50' index.html` 为 0。
2. `grep -c 'strong>\$19 lifetime' index.html` 为 1，`grep -c 'strong>\$9 per user per month' index.html` 为 1。
3. `git diff cbe76bb --stat` 只有 `index.html` 一个文件（任务卡除外），且只改那一行。
4. 主检出工作区干净；改动只在 `task/price-19` 分支。

## 改动预算

- 源码新增上限：2 行
- 源码文件上限：1 个
- `index.html`

## 必须通过的新测试

无。理由：静态单页，仓库没有测试框架；验收用上面的 grep 命令核对。

## 编码要求

一条规则：**仓库内部的调用互相信任，失败就让它抛出来；只有下面列出的外部边界做校验，并照写的方式处理失败。**

- 校验边界：无
- 失败处理：无
- 拿不准某个防护要不要加：不加，列进回执的 `omitted_guards`。
- 回合结束前有自检钩子跑 `diff_audit.py`：超预算、清单外文件会被退回修改，最多两次。

## 边界

- 只在工作树 `仓库根下的 .worktrees/price-19` 内读写和执行 git；不碰主检出。
- 不 push、不改 `.github/`；不做破坏性 git 操作。
- 不改页面其他文案、样式、标题、等候名单表单或 README。
- 保留策略：无。
- 规格有矛盾或缺事实时停下：把**一个**具体问题写进回执的 `human_action`，附建议默认值。

## 测试入口

- 相关测试：验收标准 1–3 的 grep 与 `git diff` 命令。
- 本地验证：用浏览器打开 `index.html` 不在本卡（无浏览器）。

## 完成定义

到“工作树内 `index.html` 改好、验收命令通过、回执写出”为止。不要执行 `git add`/`git commit`：提交由 Claude Code 验收后完成。
如实报告：没做完的标 blocked；没跑的命令不写 pass。

## 回执格式

最后一条消息按以下结构写，不要省略字段：

```
status: done | blocked | failed
branch: task/price-19
changed_files:
  - <路径>
tests_run:
  - <命令>: pass | fail
budget: 源码 +<行> / <文件数> 个文件（预算 2 / 1）
self_audit: <回合末自检通过 | 第 N 次后仍有 K 处，见 omitted_guards>
omitted_guards:
  - <考虑过但没加的防护，或 none>
human_action: <需要 Tom 决定的一个具体问题 + 建议默认值，或 none>
blockers:
  - <阻塞项>（没有写 none）
next: <下一步或 none>
```

## 轮次

### 第 1 轮

按本卡实现。
