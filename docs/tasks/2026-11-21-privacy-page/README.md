---
type: task
owner: datastudio-diff
author: claude-code/upwork
updated: 2026-11-21
sources:
  - "human/tom 2026-11-21：tom.xiao@hotmail.com 公开隐私政策也"
confidence: confirmed
status: active
---
# 任务卡：落地页新增隐私政策页 privacy.html

- 派出：2026-11-21，Claude Code
- 仓库：datastudio-diff（TomXiaoYZ/datastudio-diff，main 直接发布 GitHub Pages）
- 工作树：仓库根下的 .worktrees/privacy-page
- 分支：task/privacy-page（基于 main，起点提交 eab916a84c1fba802225fa9706b0882159e13566）
- 集成目标：PR 到 main，Tom 已批准本项合并
- Codex 终端：派出时登记

最多两轮。本仓库只有 `index.html` 和 `README.md`，`README.md` 即交接说明，不另做交接文档。

## 目标

Chrome Web Store 提交 Report Diff 0.1.0 需要一个公开的隐私政策网址。Tom 决定发布在
`https://tomxiaoyz.github.io/datastudio-diff/privacy.html`，联系邮箱公开为 `tom.xiao@hotmail.com`。
本卡新增静态页 `privacy.html`，内容与扩展仓库的隐私政策逐句一致；并在 `index.html` 页脚加一个指向它的链接。

## 授权

- 网络：否
- 额外可写目录：无
- MCP：关
- 模型：默认
- 强度：默认

## 输入清单

| 文件或数据 | 绝对路径 | 在工作树里？ | 用途 |
| --- | --- | --- | --- |
| 隐私政策正文（Markdown，邮箱已替换） | /Users/tomxiao/.agents/work-plans/evidence/delegate/datastudio-diff-landing/privacy-page/privacy-policy-source.md | 否（只读） | privacy.html 的全部正文来源 |
| 落地页 | index.html（style 段与 footer 段） | 是 | 复用样式；页脚加链接 |
| 说明 | README.md | 是（只读） | 了解仓库 |

## 人工前置条件

无。

## 用户路径

1. 访客（或 Chrome Web Store 审核员）打开 `privacy.html`：看到标题 `Privacy Policy — Report Diff for Data Studio`、
   `Last updated: November 21, 2026`、来源 Markdown 里的全部小节（Report definitions、Local storage and deletion、
   Anonymous usage count、Payments and paid status、Data use、Limited Use、Contact），正文文字与来源逐句相同，
   Contact 段的邮箱是可点击的 `mailto:tom.xiao@hotmail.com` 链接；两个外链（extensionpay.com、stripe.com）可点击。
   页面有一个返回首页 `index.html` 的链接。
2. 访客在落地页底部页脚看到 `Privacy policy` 链接，点击到 `privacy.html`。

## 验收标准

1. `privacy.html` 是独立完整的 HTML 文档：doctype、`lang="en"`、charset 与 viewport meta、title 元素为
   `Privacy Policy — Report Diff for Data Studio`；样式复用 `index.html` 的颜色变量（含暗色模式）、字体与 `main` 版心，不引外部脚本、字体或样式表。
2. 正文一致：把 `privacy.html` 去标签后的文字与来源 Markdown 去掉 Markdown 记号后的文字比较，每个句子都在、无增删改
   （小节标题用 h1/h2 元素，Markdown 的加粗 `**off by default**` 用 strong 元素，代码记号用 code 元素）。
3. `grep -c 'mailto:tom.xiao@hotmail.com' privacy.html` ≥ 1；`grep -c CONTACT_EMAIL privacy.html` 为 0。
4. `index.html` 只在 footer 元素内新增一行链接到 `privacy.html`（文字 `Privacy policy`），页面其他内容一字不动。
5. `git diff eab916a --stat` 只有 `index.html` 与新文件 `privacy.html`（任务卡除外）。
6. 主检出工作区干净；改动只在 `task/privacy-page` 分支。

## 改动预算

- 源码新增上限：120 行
- 源码文件上限：2 个
- `privacy.html`（新增）、`index.html`（页脚 1 行）

## 必须通过的新测试

无。理由：静态页，仓库没有测试框架；验收用上面的 grep、`git diff` 与正文比对核对。

## 编码要求

一条规则：**仓库内部的调用互相信任，失败就让它抛出来；只有下面列出的外部边界做校验，并照写的方式处理失败。**

- 校验边界：无（纯静态 HTML，无脚本）
- 失败处理：无
- 拿不准某个防护要不要加：不加，列进回执的 `omitted_guards`。
- 回合结束前有自检钩子跑 `diff_audit.py`：超预算、清单外文件会被退回修改，最多两次。

## 边界

- 只在工作树 `仓库根下的 .worktrees/privacy-page` 内读写和执行 git；不碰主检出。
- 不 push、不改 `.github/`；不做破坏性 git 操作。
- 不改隐私政策措辞（发现与来源矛盾或疑似事实错误，停下问，不自行改写）；不改 README。
- 保留策略：无。
- 规格有矛盾或缺事实时停下：把**一个**具体问题写进回执的 `human_action`，附建议默认值。

## 测试入口

- 相关测试：验收标准 1–5 的 grep、`git diff` 命令和正文比对。
- 本地验证：浏览器渲染核对不在本卡（无浏览器），由 Claude Code 验收。

## 完成定义

到“工作树内 `privacy.html` 新增、`index.html` 页脚加链接、验收命令通过、回执写出”为止。不要执行 `git add`/`git commit`：提交由 Claude Code 验收后完成。
如实报告：没做完的标 blocked；没跑的命令不写 pass。

## 回执格式

最后一条消息按以下结构写，不要省略字段：

```
status: done | blocked | failed
branch: task/privacy-page
changed_files:
  - <路径>
tests_run:
  - <命令>: pass | fail
budget: 源码 +<行> / <文件数> 个文件（预算 120 / 2）
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
