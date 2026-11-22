---
type: task
owner: datastudio-diff
author: claude-code/upwork
updated: 2026-11-22
sources:
  - "human/tom 2026-11-22：落地页改成已上架版，回复“好”"
  - "Chrome Web Store 条目 gchgoconghjcidknkloaopklkhdjhpdj 已公开上线 0.1.1（11-22 子 agent 核查）"
confidence: confirmed
status: active
---
# 任务卡：落地页改成“已上架”版

- 派出：2026-11-22，Claude Code
- 仓库：datastudio-diff（TomXiaoYZ/datastudio-diff，main 直接发布 GitHub Pages）
- 工作树：仓库根下的 .worktrees/landing-live
- 分支：task/landing-live（基于 main，起点提交 6a6aa3854eb8aaceec9eee2349c2077c15d28bdc）
- 集成目标：PR 到 main；合并须 Tom 看过 PR 后当次批准
- Codex 终端：派出时登记

最多两轮。本仓库只有 `index.html`、`privacy.html` 和 `README.md`，`README.md` 即交接说明，不另做交接文档。

## 目标

Report Diff 扩展已在 Chrome Web Store 上线，但落地页仍写“Not built yet”、主按钮是等候名单表单，访客装不了扩展。
本卡把 `index.html` 改成已上架版：主按钮直达商店安装页，删掉等候名单与“还没做”的文案，价格段改为现行的 Free / Pro 说明。
文案已定（见验收标准），照抄，不另行改写。

## 授权

- 网络：否
- 额外可写目录：无
- MCP：关
- 模型：默认
- 强度：默认

## 输入清单

| 文件或数据 | 绝对路径 | 在工作树里？ | 用途 |
| --- | --- | --- | --- |
| 落地页 | index.html（main 区块开头的 a.cta 与 p.note、“Planned pricing”小节） | 是 | 唯一要改的文件 |
| 说明 | README.md | 是（只读） | 了解仓库 |

## 人工前置条件

无。

## 用户路径

1. 访客打开落地页，首屏看到原标题和导语，下面的主按钮写 `Add to Chrome — it's free`，点击在当前页跳到
   `https://chromewebstore.google.com/detail/gchgoconghjcidknkloaopklkhdjhpdj`。
2. 按钮下方一行说明：免费可用 3 份报表，Pro 解锁更多。页面任何地方都不再出现等候名单、Google 表单或“Not built yet”。
3. 往下到价格小节，标题是 `Pricing`，看到 Free 与 Pro 的内容和价格。

## 验收标准

1. a.cta 元素：`href="https://chromewebstore.google.com/detail/gchgoconghjcidknkloaopklkhdjhpdj"`，文字精确为
   `Add to Chrome — it's free`（破折号为 U+2014，撇号为 ASCII `'`）。不加 target 属性。
2. 紧随其后的 p.note 文字精确为：
   `Free for up to 3 reports. Pro adds unlimited reports and full snapshot history.`
3. 小节标题 `Planned pricing` 改为 `Pricing`；其下 div.price 的纯文字精确为：
   `Free: 3 reports, the 3 most recent snapshots of each, and the full change list between the latest two. Pro: $9 per user per month, or $19 lifetime — unlimited reports, 100 snapshots per report, compare any two snapshots. Payments are processed by ExtensionPay and Stripe.`
   其中 `$9 per user per month` 与 `$19 lifetime` 两段各用一个 strong 元素包住（与现页面相同的写法）。
4. `grep -c -i 'waitlist\|docs.google.com/forms\|Not built yet\|Planned pricing\|Nothing to pay now' index.html` 为 0；
   `grep -c 'chromewebstore.google.com/detail/gchgoconghjcidknkloaopklkhdjhpdj' index.html` 为 1。
5. 除上述三处外 `index.html` 一字不动（style、示例改动清单、Why、Who it's for、footer 不变）；`privacy.html`、`README.md` 不动。
6. `git diff 6a6aa38 --stat` 只有 `index.html`（任务卡除外）；主检出工作区干净。

## 改动预算

- 源码新增上限：10 行
- 源码文件上限：1 个
- `index.html`（三处替换）

## 必须通过的新测试

无。理由：静态页，仓库没有测试框架；验收用上面的 grep 与 `git diff` 核对，渲染由 Claude Code 验收。

## 编码要求

一条规则：**仓库内部的调用互相信任，失败就让它抛出来；只有下面列出的外部边界做校验，并照写的方式处理失败。**

- 校验边界：无（纯静态 HTML，无脚本）
- 失败处理：无
- 拿不准某个防护要不要加：不加，列进回执的 `omitted_guards`。
- 回合结束前有自检钩子跑 `diff_audit.py`：超预算、清单外文件会被退回修改，最多两次。

## 边界

- 只在工作树 `仓库根下的 .worktrees/landing-live` 内读写和执行 git；不碰主检出。
- 不 push、不改 `.github/`；不做破坏性 git 操作。
- 不改验收标准里给定的文案；发现与页面其他内容矛盾时停下问。
- 保留策略：无。
- 规格有矛盾或缺事实时停下：把**一个**具体问题写进回执的 `human_action`，附建议默认值。

## 测试入口

- 相关测试：验收标准 4–6 的 grep 与 `git diff` 命令。
- 本地验证：浏览器渲染核对不在本卡，由 Claude Code 验收。

## 完成定义

到“工作树内 `index.html` 三处已替换、验收命令通过、回执写出”为止。不要执行 `git add`/`git commit`：提交由 Claude Code 验收后完成。
如实报告：没做完的标 blocked；没跑的命令不写 pass。

## 回执格式

最后一条消息按以下结构写，不要省略字段：

```
status: done | blocked | failed
branch: task/landing-live
changed_files:
  - <路径>
tests_run:
  - <命令>: pass | fail
budget: 源码 +<行> / <文件数> 个文件（预算 10 / 1）
self_audit: <回合末自检通过 | 第 N 次后仍有 K 处，见 omitted_guards>
omitted_guards:
  - <考虑过但没加的防护，或 none>
blockers:
  - <或 none>
human_action: <none | 一个具体问题 + 建议默认值>
```

## 轮次

### 第 1 轮
