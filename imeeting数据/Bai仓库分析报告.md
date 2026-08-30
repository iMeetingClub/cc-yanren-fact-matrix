# Bai 仓库分析报告：提交历史 + 实际开发内容 ↔ S7-4 需求对应

> 分析人：repo-analyst（AgentTeams / matrix-enhance）
> 分析对象：https://github.com/nantang-dao/Bai.git
> 本地克隆：`imeeting数据\Bai_repo\`（全量克隆，含全部分支与完整历史）
> 需求对照基准：`提案原始文件\S7-4任务系统平台2.0升级开发提案.md`（提案日期 2026-05-11，预算 2000U）
> 本报告只列事实，全部结论附 git 证据（commit hash / 文件路径 / 行号）。

---

## 0. 克隆与仓库概况

- 直连 github.com:443 不通（`Failed to connect ... Could not connect to server`），经本机代理 `127.0.0.1:7890` 完成 `git clone`（exit=0，全量非浅克隆）。
- 默认分支 `main`，`git rev-list --count HEAD` = **118 条提交**。
- 远程分支 5 个：`origin/main`（2026-07-27）、`origin/bai-ui`（2026-03-30）、`origin/elorze`（2026-06-08）、`origin/feat/proof-image-compression`（2026-07-27）、`origin/flyio-new-files`（2026-05-12）。
- 仓库顶层结构：
  - `mycoseed-frontend/` — Nuxt 3 + Vue 3 + Tailwind 前端（pages/components/stores/utils）
  - `mycoseed-backend/` — Express + TypeScript + Supabase 后端（controllers/routes/services/db migrations）
  - `contracts-project/` — Hardhat 合约项目（含 `RemarkLogicV1.sol`，见 `改动记录.md` L31）
  - `README.md`（2 行简介）、`改动记录.md`（MycoSeed Dev 链上转账/活动付款改动文档，584 行）

### 作者身份识别（main 分支，`git shortlog -sne HEAD`）

| 提交数 | 作者 | 邮箱 | 身份推断 |
|---|---|---|---|
| 62 | Elorze | 2270938474@qq.com | 另一开发者（任务系统后端/OAuth/合约重构主力） |
| **45** | **Cecilia-Yuyan-Chen** | **cw71707@gmail.com** | **CC（唯一 GitHub 身份，未见第二个账号/邮箱）** |
| 3 | Fly.io | noreply@fly.io | 部署平台自动生成 |
| 3 | MycoSeed Dev | bai-activity@mycoseed.dev | 「bai-activity」分支专用账号（活动付款追踪） |
| 3 | fly-io[bot] | 52462049+fly-io[bot]@users.noreply.github.com | 部署机器人（合并 PR） |
| 1 | Crocs | hongyang.xiang1120@gmail.com | 邮箱前缀 hongyang.xiang（与「虹阳」姓名拼音一致） |
| 1 | pingyangzangse | 136684647+...@users.noreply.github.com | 「Add files via upload」网页上传 |

### CC 身份核验（应用户确认要求补查，全分支 `--all`）

- 用户确认 CC 的 GitHub 提交名称为 **Cecilia-Yuyan-Chen**；核查全部分支（main/bai-ui/elorze/feat/proof-image-compression/flyio-new-files）后：
  - **作者（author）身份全仓库仅 7 个，CC 只有 1 个：`Cecilia-Yuyan-Chen <cw71707@gmail.com>`，无任何 email 变体**（`git log --all --format="%an|%ae" | sort -u`）。
  - **提交者（committer）维度**：CC 的 45 条中 43 条 committer 为本人同邮箱；另 2 条经 GitHub 网页端操作，committer 为 `GitHub <noreply@github.com>`（`fcc7e71` Merge PR #3、`b0be0c1` Update README.md），author 仍为 CC 本人。
  - 模糊检索 `cecilia|chen|yuyan|cw71707|语嫣|陈` 命中全部 45 条且仅命中该身份，无遗漏、无冒名迹象。
  - **CC 的 45 条提交全部已合入 main**（`git log --all --author=Cecilia --not HEAD` 为空），无遗留在其他分支的 CC 提交。
  - 全分支去重后各作者总量：Elorze 78、CC 45、Fly.io 3、fly-io[bot] 3、MycoSeed Dev 3、pingyangzangse 1、Crocs 1。其中 main 分支为 Elorze 62 / CC 45 / 其他 11（Elorze 有 16 条提交未合入 main，留在 origin/elorze 等分支）。

---

## 1. 提交历史全貌（main，118 条，按时间升序；**【CC】** 标注 Cecilia-Yuyan-Chen 提交）

| # | hash | 作者 | 日期时间 | message |
|---|---|---|---|---|
| 1 | aa32462 | **【CC】** | 2026-02-01 02:33 | release version 1.0 |
| 2 | 5293099 | **【CC】** | 2026-02-01 03:42 | first commit |
| 3 | b0be0c1 | **【CC】** | 2026-02-01 03:44 | Update README.md |
| 4 | 13e9749 | **【CC】** | 2026-02-01 05:47 | updated ui |
| 5 | fcc7e71 | **【CC】** | 2026-02-01 06:04 | Merge pull request #3 from Cecilia-Yuyan-Chen/cc-develop |
| 6 | f99594a | **【CC】** | 2026-02-04 14:26 | add full project files |
| 7 | 62a02a4 | Elorze | 2026-02-04 17:00 | 合并 frontend 和 backend 到统一仓库 |
| 8 | b6c3041 | Elorze | 2026-02-04 17:03 | 删除实现情况说明.md |
| 9 | 51a4b8a | **【CC】** | 2026-02-04 18:19 | 解除前后端子模块，转为统一管理 |
| 10 | 542b745 | **【CC】** | 2026-02-04 18:27 | frontend ui develop |
| 11 | 13a900c | Elorze | 2026-02-05 19:26 | 合并 cc-develop: 采用新的 UI，保留 main 的配置 |
| 12 | a4c1dae | Elorze | 2026-02-05 20:13 | 修复个人页面钱包地址获取：从后端API获取真实钱包地址 |
| 13 | 60b9909 | Elorze | 2026-02-05 20:27 | 修复积分计算逻辑：从总积分模式改为每人积分模式 |
| 14 | af9a750 | Elorze | 2026-02-05 20:38 | 隐藏个人页面编辑按钮并添加点击提示 |
| 15 | 5cce957 | Elorze | 2026-02-05 20:43 | 更新个人页面提示文字并删除未加入社区提示 |
| 16 | 5bd8c95 | **【CC】** | 2026-02-05 20:51 | add setting and repair task flow |
| 17 | da64f87 | **【CC】** | 2026-02-05 21:12 | repair modify profile |
| 18 | 67cd0c7 | Elorze | 2026-02-05 21:20 | 修复TypeScript类型错误：params.reward已经是number类型 |
| 19 | 57cee9b | Fly.io | 2026-02-05 13:23 | New files from Fly.io Launch |
| 20 | 70e7a63 | Elorze | 2026-02-05 21:38 | 修复审核后数据刷新问题 |
| 21 | e721d49 | Elorze | 2026-02-05 21:50 | 修复标记转账失败问题：使用taskId查找提交而不是索引 |
| 22 | 997863a | Elorze | 2026-02-05 22:02 | 修复console.log错误：避免打印循环引用对象 |
| 23 | 89d393e | Elorze | 2026-02-05 22:12 | 修复标记转账后UI不更新问题 |
| 24 | 8574346 | Elorze | 2026-02-05 22:30 | 优化标记转账交互流程：移除弹窗，自动跳转任务详情 |
| 25 | 6501dfa | Elorze | 2026-02-06 16:35 | feat: 添加本地开发环境支持，自动加载 .env.development |
| 26 | 07b9061 | Elorze | 2026-02-06 17:58 | feat: 前端添加本地开发环境支持，使用 loadEnv |
| 27 | 049d7d1 | Elorze | 2026-02-07 11:45 | 修复手机号验证码登录问题 |
| 28 | a826c0c | Elorze | 2026-02-07 12:10 | 修复 URL 参数解析问题：添加防御性处理 |
| 29 | cae03af | Elorze | 2026-02-09 20:59 | Update: 最新代码更新 |
| 30 | 329eec8 | Elorze | 2026-02-09 20:59 | Update: 最新代码更新 |
| 31 | 510454a | **【CC】** | 2026-02-09 22:31 | improve task card ui and task flow |
| 32 | df2a81f | **【CC】** | 2026-02-09 22:39 | 精确提示词 |
| 33 | 9d6e9cc | **【CC】** | 2026-02-10 19:13 | 紧急修改前端隐藏未开发内容 |
| 34 | 4627e02 | **【CC】** | 2026-02-10 21:04 | repair |
| 35 | 75ade89 | **【CC】** | 2026-02-10 21:54 | another repair |
| 36 | 2b5aee3 | fly-io[bot] | 2026-02-10 14:40 | Merge pull request #1 from Elorze/flyio-new-files |
| 37 | ac6c23a | Fly.io | 2026-02-10 15:00 | New files from Fly.io Launch |
| 38 | 2857d20 | **【CC】** | 2026-02-10 23:07 | chore: update package-lock.json to sync with package.json |
| 39 | 3a64c0d | fly-io[bot] | 2026-02-10 15:07 | Merge pull request #2 from Elorze/flyio-new-files |
| 40 | 0048845 | **【CC】** | 2026-02-12 22:47 | fix task card frontend |
| 41 | 4b6cfff | **【CC】** | 2026-02-12 22:48 | Merge branch 'main' of github.com:Elorze/bai |
| 42 | ec4d4d0 | Elorze | 2026-02-14 19:09 | Merge branch 'main' into elorze |
| 43 | c0b730b | Elorze | 2026-02-18 23:03 | feat: 完善社区圈功能 - 发帖、点赞、评论和图片上传 |
| 44 | 1587ac8 | Elorze | 2026-02-19 09:05 | chore: 后端端口使用 PORT 环境变量，适配 Fly.io |
| 45 | 321ef0f | Elorze | 2026-02-19 09:05 | Merge main into elorze: PORT 环境变量适配 |
| 46 | 227906a | Elorze | 2026-02-19 09:16 | fix: 补全 src/types/post.ts 类型定义 |
| 47 | 3ef05bc | Elorze | 2026-02-19 09:37 | fix: Dockerfile 默认 PORT=8080 |
| 48 | 97511ed | Elorze | 2026-02-19 09:47 | fix: 生产环境默认监听 8080 |
| 49 | f6afa2d | Elorze | 2026-02-19 09:51 | fix: 生产环境强制监听 8080 |
| 50 | ad7b7a1 | Elorze | 2026-02-19 13:57 | 评论回复某人：点评论即回复；后端 replyTo 支持；迁移 015 |
| 51 | 6c4d5be | **【CC】** | 2026-02-19 17:41 | admin and community init |
| 52 | fb7624e | **【CC】** | 2026-02-19 21:58 | repair admin |
| 53 | b8afaab | **【CC】** | 2026-02-19 23:19 | remove default community, repair community switch, performance optimization of community,admin |
| 54 | f084e23 | **【CC】** | 2026-02-25 18:32 | 更新社区大厅用户头像和姓名,添加社区头像功能,修复切换社区bug |
| 55 | 651ace8 | Elorze | 2026-03-13 21:25 | 添加 contracts-project、020 迁移与 remark 相关、ShareToCommunityModal |
| 56 | 79c4e74 | Crocs | 2026-03-15 21:31 | Upload local version to UI branch |
| 57 | 36d44cf | **【CC】** | 2026-03-16 16:15 | 更新社区头像和背景上传方式,隐藏社区积分,更新管理员权限,修复相关bug |
| 58 | 4068c16 | Elorze | 2026-03-19 10:24 | Merge branch 'UI' into elorze |
| 59 | 882a7c8 | Elorze | 2026-03-27 15:49 | refactor contracts project layout and deployment scripts |
| 60 | be86faf | Elorze | 2026-03-27 15:49 | update task backend types for new taskpool flow |
| 61 | 518c2cd | Elorze | 2026-03-27 15:49 | update frontend task pages and API wiring |
| 62 | 50f0cab | Elorze | 2026-03-27 16:42 | fix transfer remark fallback when reopening tasks |
| 63 | 7beffdc | Elorze | 2026-03-27 17:15 | add long-press tipping in community feed |
| 64 | e2c084b | **【CC】** | 2026-04-09 22:07 | 初步完成FAQs消息推送与相关设置任务与社区圈帖子的删除撤回功能 |
| 65 | 92d40d1 | **【CC】** | 2026-04-10 19:50 | merge branch 'main' of github.com:nantang-dao/Bai |
| 66 | 6560d01 | **【CC】** | **2026-05-04 18:27** | **提升系统安全性初步完成商城和活动功能** |
| 67 | 68305f1 | **【CC】** | 2026-05-07 20:54 | improve markettplace and activity |
| 68 | 6cbbaae | Elorze | 2026-05-08 11:31 | Merge remote-tracking branch 'nantang/main' |
| 69 | 5b07866 | **【CC】** | 2026-05-08 17:42 | fully checked on activity and marketplace, also repair GPS Function |
| 70 | 9b5c908 | **【CC】** | 2026-05-08 17:43 | Merge branch 'main' of github.com:nantang-dao/Bai |
| 71 | 6424dd1 | Elorze | 2026-05-09 08:52 | feat(auth): Semi OAuth2 code+PKCE with HttpOnly session |
| 72 | aff0518 | Elorze | 2026-05-09 08:55 | Merge oauth-login into main |
| 73 | 252a982 | **【CC】** | 2026-05-11 21:28 | repair tags |
| 74 | aa5e5d4 | **【CC】** | 2026-05-11 21:28 | Merge branch 'main' of github.com:nantang-dao/Bai |
| 75 | 1a2a3ea | **【CC】** | 2026-05-11 21:56 | add excel download function for activity |
| 76 | 6a78902 | **【CC】** | 2026-05-11 22:36 | repair excel download function for activity |
| 77 | 3b5f67f | **【CC】** | 2026-05-11 23:23 | repair excel download function for activity |
| 78 | 885982f | **【CC】** | 2026-05-12 02:39 | repair login samesite cookie, fully done excel download, repair calendar register window bug, add registed ✓ 报名logic |
| 79 | 0d5d81b | **【CC】** | 2026-05-12 04:01 | 更新完善前端消息中心的显示和设置页面的迁移优化相关前端显示 |
| 80 | 55fc060 | Fly.io | 2026-05-12 04:36 | New files from Fly.io Launch |
| 81 | 52ec7ff | Elorze | 2026-05-14 17:20 | fix(auth): Semi login omit redirect_uri query |
| 82 | ed83250 | **【CC】** | 2026-05-19 18:04 | 显式允许跨域 cookie,添加 credentials: include |
| 83 | 93d0e06 | **【CC】** | 2026-05-19 18:24 | 更新 downloadExcel 函数以使用动态导入 |
| 84 | 62a4453 | fly-io[bot] | 2026-05-19 10:29 | Merge pull request #5 from nantang-dao/flyio-new-files |
| 85 | ae866d2 | Elorze | 2026-05-20 09:18 | feat(auth): multi-domain CORS and OAuth return_origin |
| 86 | ac8da63 | Elorze | 2026-05-20 09:23 | Merge nantang/main: integrate remote before multi-domain push |
| 87 | b8ffdfe | MycoSeed Dev | 2026-05-23 17:10 | feat: 添加链上转账记录查询功能 |
| 88 | 224a88b | MycoSeed Dev | 2026-05-23 19:41 | feat: 活动付款精确追踪 — 后端模糊匹配升级为精确匹配 |
| 89 | ffc1a90 | pingyangzangse | 2026-05-23 19:57 | Add files via upload |
| 90 | cc63a75 | MycoSeed Dev | 2026-05-24 14:39 | feat: 活动付款精确追踪、任务状态优化与 transactions 降级处理 |
| 91 | 7c0b1cc | Elorze | 2026-05-25 16:29 | Merge activity/master (bai-activity) via unrelated histories. |
| 92 | 6501a14 | Elorze | 2026-06-01 15:50 | fix(auth): 同源反代 OAuth，避免跨域 Session Cookie 失效 |
| 93 | 66db0c4 | Elorze | 2026-06-01 19:21 | fix(proxy): forward Set-Cookie on OAuth 302 redirects |
| 94 | 6ad66fc | **【CC】** | 2026-06-01 22:38 | try to fix pic upload fail |
| 95 | a7dd486 | Elorze | 2026-06-02 12:09 | feat(frontend): restore long-press tipping in community feed |
| 96 | e4e2327 | Elorze | 2026-06-02 13:55 | feat(frontend): allow comment authors to delete own comments in feed |
| 97 | c6a789c | **【CC】** | 2026-06-05 00:14 | add:multi-task show in canlenda |
| 98 | 6b98c49 | **【CC】** | 2026-06-05 00:19 | Merge branch 'main' of github.com:nantang-dao/Bai |
| 99 | 35053e4 | **【CC】** | 2026-06-05 00:21 | add: muultitask show in calendar, fix: transfer display the right nt_token amount |
| 100 | a81cd51 | Elorze | 2026-06-05 20:33 | fix(proxy): disable Nitro /api proxy in production |
| 101 | 839a962 | **【CC】** | 2026-06-08 14:42 | fix: transac amount and activity mine option |
| 102 | 2a630e2 | Elorze | 2026-06-08 16:28 | fix(frontend): 活动付款 Semi 链接补 task_uuid 以对齐链上 Remark |
| 103 | a20540a | **【CC】** | **2026-06-16 22:12** | add task tag, search chunk, task sort. fix reminder clear due, sticky submit button（CC 最后一条非合并提交） |
| 104 | 2938c7e | **【CC】** | **2026-06-16 22:13** | Merge branch 'main' of github.com:nantang-dao/Bai（**CC 最后一条提交**） |
| 105 | 797dd45 | Elorze | 2026-06-18 17:59 | feat(frontend): 提交页自动保存提交说明草稿到 localStorage |
| 106 | fb25258 | Elorze | 2026-06-18 19:22 | feat: 接包者撤回提交（submitted → unsubmit） |
| 107 | 491234b | Elorze | 2026-06-20 10:51 | refactor(frontend): 个人中心发布/领取任务 Tab 与状态筛选对齐 |
| 108 | 046c39a | Elorze | 2026-06-22 10:34 | feat(frontend): 任务描述支持轻量 Markdown 编辑与展示 |
| 109 | 1df9d83 | Elorze | 2026-06-22 10:59 | fix(frontend): 子目录组件显式 import |
| 110 | b89e9a5 | Elorze | 2026-06-22 22:26 | fix(frontend): 加粗按钮与标签同行 |
| 111 | 41f10dd | Elorze | 2026-06-22 23:34 | fix(frontend): 移除 Markdown 依赖，改用纯文本换行展示 |
| 112 | 9ebc60d | Elorze | 2026-06-23 15:19 | feat: 待确认付款（pending-see）与任务列表组装 |
| 113 | aac2087 | Elorze | 2026-06-29 22:28 | fix(frontend): enable Nitro API proxy in production |
| 114 | 43bddeb | Elorze | 2026-06-29 23:43 | Merge main into feat/pending-see |
| 115 | df5344f | Elorze | 2026-06-30 09:20 | feat: 个人中心三 Tab，任务与活动混排展示 |
| 116 | e639937 | Elorze | 2026-06-30 23:01 | fix: show task load error with retry on profile page |
| 117 | 170bc98 | Elorze | 2026-07-01 07:10 | feat: task plaza cursor pagination to fix 1000-row Supabase cap |
| 118 | 4d5b1fe | Elorze | **2026-07-27 17:39** | feat: compress task proof images on upload and add backfill script（**仓库最后一条提交**） |

---

## 2. 实际开发内容盘点 与 S7-4 第二章逐项对照

### 2.0 代码总览

- 前端页面 38 个（`mycoseed-frontend/pages/`），其中本提案相关：
  - 活动中心：`pages/community/[id]/events/`（index.vue 列表+日历、create.vue、[eventId].vue 详情/报名、calendar-settings.vue 日历标签管理）
  - 商城：`pages/community/[id]/marketplace/`（index.vue 瀑布流、create.vue、[listingId].vue 详情/购买/评价、settings.vue 商城标签管理）
  - 全局配置：`pages/community/[id]/settings.vue`（社区功能设置总面板）、`pages/community/[id]/tasks/tag-settings.vue`（任务标签）
- 后端控制器：`communityEventsController.ts`（823 行）、`marketplaceController.ts`（703 行）、`communitiesController.ts`、`myParticipationsController.ts` 等；路由 `routes/communityEvents.ts`、`routes/marketplace.ts`、`routes/communities.ts`；权限中间件 `middleware/communityAdmin.ts`（requireSuperAdmin / requireCommunityAdmin / requireCommunityMember）。

### 2.1 功能模块一：活动中心 —— **已实现（2 个子项未见/降级）**

| 需求子项 | 状态 | 代码证据 |
|---|---|---|
| 发布单一活动 | ✅ 已实现 | create.vue L75 `template v-if="kind === 'single'"`；后端 communityEventsController.ts L516 `['single','composite','pack']` 校验 |
| 复合活动（多个子选项） | ✅ 已实现 | create.vue L92、L105 `子选项`、L192 `{ v: 'composite', l: '复合活动' }` |
| 周期性活动包（每日站会/每周读书会） | ✅ 已实现 | create.vue L116-137、L193 `{ v: 'pack', l: '活动包' }`，频率 daily/weekly + 自定义星期（L124 `packFrequency === 'weekly'` 选星期、L487 packCustomWeekdays）；后端 L631-657 期次展开（pack_frequency/pack_range_start/end/pack_custom_weekdays），occurrence 期次表 `community_event_occurrences`（L33、L150） |
| 活动置顶 | ✅ 已实现 | 后端 pinEvent L700-711（is_pinned/pinned_at）；路由 communityEvents.ts L18；列表排序 L173-178 pinned 在前 |
| 撤回未报名活动 | ✅ 已实现 | deleteEvent L682 `if (n > 0) return ... '已有报名，无法删除'`（即有报名不可删，未报名可删）；路由 L17 需 requireCommunityAdmin |
| 查看报名名单及备注 | ✅ 已实现 | 后端 L396-470 汇总参与者（occurrence_id/option_id/remark/status）；前端 [eventId].vue L60「我的报名」区、L71/L592-668 downloadExcel 导出「参与矩阵.xlsx」（xlsx 动态导入，1a2a3ea/885982f 两个 CC 提交） |
| 日/周/月日历视图 | ✅ 已实现 | events/index.vue L375-379 `calScale: 'month'|'week'|'day'`，L123 月视图、L170 周视图、L208 日视图；后端 listEventsCalendar（路由 L13） |
| 点击日期弹窗查看详情+一键报名 | ✅ 已实现 | index.vue 日期弹窗（modalDayItems，L347）；registerEvent 后端 L722-770（报名校验「不在报名时间内」「该期次已报名」），路由 L19 |
| 取消报名（活动开始前） | ✅ 已实现 | cancelRegistration L794-815（`报名已截止，无法取消`），路由 L20-25 |
| 「我的报名」视图 | ✅ 已实现 | events/index.vue L34-35 `mineOnly`「我的活动」勾选过滤（mine 参数传到后端 L617/L694/L715）；[eventId].vue L60；另 2026-06-30 Elorze df5344f 加个人中心任务+活动混排 Tab（myParticipationsController.ts） |
| 活动卡片标签颜色区分类型 | ✅ 已实现 | calendar-settings.vue 日历标签管理（colorHex，L107 createCalendarTag 带 colorHex）；index.vue L28 `:style="{ backgroundColor: t.colorHex ... }"` |
| 列表虚拟滚动 | ❌ **未见** | 全仓库 grep `虚拟滚动/virtual scroll/VirtualScroll` 0 命中（仅 yarn.lock 依赖名误命中） |
| 已过期活动自动变灰沉底 | ✅ 已实现（部分为「变淡」而非灰色） | 后端 L179-189 ended 排到最后；前端 index.vue L49 `:class="{ 'opacity-60': isEnded(ev) }"`（透明度变灰效果） |

### 2.2 功能模块二：技能/物品商城（C2C） —— **已实现（1 个子项为降级实现）**

| 需求子项 | 状态 | 代码证据 |
|---|---|---|
| 全员发布图文商品、NT 定价 | ✅ 已实现 | marketplace/create.vue；后端 createListing，marketplaceController.ts L13 字段 `seller_id, title, description, price, status...`；路由 L15 仅需 requireCommunityMember |
| 主图/标题/详情/价格/标签 | ✅ 已实现 | 同上字段 + listing 图片（uploadController.ts）；标签关联 marketplace_tags（L89 select `id, name, color_hex, sort_order, archived`） |
| 首页双列瀑布流 | ⚠️ 部分实现 | marketplace/index.vue L59 `grid grid-cols-2 md:grid-cols-4 gap-3`——双列**网格**布局，非真正的瀑布流（masonry）；全仓库 grep `瀑布/masonry/columns-2` 无命中 |
| 关键词搜索 | ✅ 已实现 | index.vue L17-30 搜索框 `placeholder="搜索标题、描述、卖家、标签…"`，L159/180 `q` 参数传后端 |
| 标签筛选 | ✅ 已实现 | index.vue L33-43 标签条（`t.archived` 过滤），L160/181 `tagId` 参数 |
| 购买→支付NT→卖家确认收款→已售出变灰 | ✅ 已实现 | 后端流程：lockListing（L508，买家锁定）→ confirmSold（L546-564：L559 `仅卖家可操作`、L560 `当前状态不可确认收款`、L564 `status: 'sold', sold_at`）→ cancelLock（L588）；前端 index.vue L65 `'opacity-60 grayscale': item.status === 'sold'` 已售出变灰、L85 售出标记 |
| 交易完成后文字评价+星级评分、公开显示 | ✅ 已实现 | submitReview L604-621（L620 `仅已成交可评价`、L621 `仅买家可评价`）；[listingId].vue L104 `'★'.repeat(existingReview.rating)` 五星显示、L122 评价列表；路由 L22/L24 |
| 未交易商品发布者撤回修改 | ✅ 已实现 | withdrawListing L483；路由 L18；updateListing L17（PATCH） |
| 总管理员管理标签库 | ✅ 已实现 | 路由 L9-12 标签 CRUD 均挂 `requireSuperAdmin`；marketplace/settings.vue 管理页 |

### 2.3 模块三：社区全局配置（仅总管理员可见） —— **部分实现**

| 需求子项 | 状态 | 代码证据 |
|---|---|---|
| 集中管理面板（仅总管理员） | ✅ 已实现 | community/[id]/settings.vue L7「社区功能设置」、L15 `无权限`、L17 `v-if="community.myRole === 'super_admin'"` |
| 社区可见性：公开/非公开（邀请码+审批） | ✅ 已实现 | settings.vue L18-27 可见性勾选「公开（未勾选则需邀请码加入并审批）」+ 邀请码展示/复制；后端 communitiesController.ts L193-211 join-by-invite（`邀请码无效`/`已提交申请，等待审批`）、L218-238 join（slug 校验）；api.ts L1472-1473 `slug 邀请码 / isPublic 是否公开` |
| 三类标签统一管理（商城/活动/任务） | ✅ 已实现 | settings.vue L30-53 三个入口：商城标签（marketplace/settings.vue）、活动标签（events/calendar-settings.vue）、任务标签（tasks/tag-settings.vue，2026-06-16 CC 提交 a20540a 新增） |
| 标签增删改查+颜色设置 | ✅ 已实现 | calendar-settings.vue L100-139（create/update/deleteCalendarTag，colorHex）；marketplace 路由 L9-12 CRUD |
| 归档删除（历史保留标签名，新发布不显示） | ✅ 已实现 | marketplaceController.ts L176 注释 `archive instead of hard delete`、L183 `.update({ archived: true })`；L91 查询 `archived.is.null,archived.eq.false` 过滤 |
| 功能包预留（匿名投票、分组讨论接口） | ❌ **未见（仅有占位 UI）** | settings.vue L55-61「社区功能包管理」区块内容为 `opacity-50 cursor-not-allowed` + `待完善`，无任何后端接口；全仓库 grep `投票/匿名/分组讨论/poll/vote` 在功能代码中 0 命中（仅 frontend README.md L15 宣传语「DAO 投票」与无关「预留字段」注释） |

---

## 3. 开发时间线（git 证据）

### 3.1 CC 的提交分布

- **首次提交**：`aa32462` 2026-02-01 02:33「release version 1.0」（仓库创始人提交）。
- **最后提交**：`2938c7e` 2026-06-16 22:13（merge）/ 最后非合并提交 `a20540a` 2026-06-16 22:12。
- 按月分布：2026-02 共 22 条、03 共 1 条、04 共 2 条、05 共 13 条、06 共 7 条，07 共 **0** 条。
- CC 在 main 共 45/118 条（38.1%）；Elorze 62 条（52.5%）。

### 3.2 三大功能模块的提交时间段

| 模块 | 首次出现的提交 | 主要开发时段 | 收尾提交 |
|---|---|---|---|
| 活动中心 + 商城（后端控制器、前端页面、024 迁移） | **`6560d01`（CC，2026-05-04 18:27）「提升系统安全性初步完成商城和活动功能」**——`git log --diff-filter=A` 确认 communityEventsController.ts / marketplaceController.ts / events 页面 / marketplace 页面 / 024_create_marketplace.sql 均首见于此提交 | 5/4 初成 → `68305f1` 5/7「improve markettplace and activity」→ `5b07866` 5/8「fully checked on activity and marketplace」→ 5/11-12 报名 Excel 导出与报名逻辑（1a2a3ea、885982f）→ 5/23-24 活动付款精确追踪（MycoSeed Dev b8ffdfe/224a88b/cc63a75，bai-activity 分支，5/25 由 Elorze `7c0b1cc` 合入） | 6/5 日历多任务显示（c6a789c/35053e4，CC）、6/8 activity mine option（839a962，CC）、6/30 个人中心活动混排（df5344f，Elorze） |
| 社区全局配置（可见性/权限） | 权限中间件 communityAdmin.ts 首见于 `6c4d5be`（CC，2026-02-19「admin and community init」）；communitiesController.ts 首见于 `79c4e74`（Crocs，2026-03-15 UI 分支上传）；设置总面板 settings.vue 首见于 `6560d01`（CC，5/4） | 商城/活动标签管理随 5/4-5/7 一起落地 | 任务标签管理 `tag-settings.vue` 首见于 `a20540a`（CC，**2026-06-16**）——晚于提案「5/15 前完成全局配置联调」的里程碑 |

### 3.3 与 S7-4 时间线的直接对照

- 提案日期 **2026-05-11**（S7-4 L4）；商城+活动模块代码首次提交 **2026-05-04**（6560d01），**早于提案日期 7 天**；5/8 已「fully checked」。
- 提案里程碑（L46-48）：5/15 前全局配置联调、5/20 前部署+手册+培训、5/25 前修复交付。
  - 5/11-5/12 CC 仍在加报名 Excel 导出与修报名窗口 bug（1a2a3ea、6a78902、3b5f67f、885982f）；
  - 5/23-5/25 活动付款精确追踪（MycoSeed Dev 3 条 + Elorze 合入）；
  - 任务标签管理（全局配置三类标签之一）6/16 才提交（a20540a）；
  - 功能包预留接口至仓库最后提交（7/27）仍为「待完善」占位。
- 仓库最后提交：`4d5b1fe` 2026-07-27 17:39（Elorze，任务凭证图片压缩）。CC 6/16 之后无任何提交；6/17-7/27 期间 14 条提交全部来自 Elorze。

---

## 4. 仓库文档/部署配置摘录（与开发范围相关原文）

- 根 `README.md`（全文 2 行）：
  > "Community-assistant---Bai — Your community helper: Bai, an open-source community management platform. Help communities publish tasks, announcements & events. Visualize activity & manage members."
- `mycoseed-frontend/README.md` L15：「**社区治理**：DAO 投票、社区管理」（宣传性描述，代码中未见投票实现）。
- 根 `改动记录.md` L9-12（MycoSeed Dev 两条提交说明表）：
  > | `b8ffdfe` | `master` | 2026-05-23 | feat: 添加链上转账记录查询功能（模糊匹配） |
  > | `224a88b` | `master` | 2026-05-23 | feat: 活动付款精确追踪（精确匹配升级） |
  并注明（L34）「本次提交包含整个项目的初始提交（196 个文件）」——即 bai-activity 分支为一份整项目快照。
- 部署配置：`mycoseed-backend/Dockerfile`、`mycoseed-backend/fly.toml`、`mycoseed-frontend/DEPLOYMENT_GUIDE.md`，Fly.io 自动提交 3 条（57cee9b/ac6c23a/55fc060）+ bot 合并 3 条，证实生产部署在 Fly.io。
- 后端另有文档：`mycoseed后端开发文档.md`、`PLAN.md`、`USER_IDENTITY_SYSTEM.md`、`SUPABASE_MIGRATION_GUIDE.md`、`RESTART_GUIDE.md`。

---

## 5. 小结（仅事实）

1. 仓库 main 共 118 条提交（2026-02-01 ~ 2026-07-27），CC（Cecilia-Yuyan-Chen, cw71707@gmail.com，唯一身份）45 条，最后一条 2026-06-16；6/17 之后全部由 Elorze 提交。
2. S7-4 三大模块中：活动中心、商城两大模块主体功能均已实现（证据见 §2.1/§2.2）；未实现项仅 3 处：活动列表虚拟滚动、严格意义的瀑布流（现为双列网格）、功能包预留接口（仅 UI 占位「待完善」）。
3. 商城+活动模块代码首见于 2026-05-04 的 CC 提交 6560d01，早于 S7-4 提案日期（5/11）一周；任务标签管理 6/16 才补齐，功能包预留至 7/27 最后提交仍未实现。
