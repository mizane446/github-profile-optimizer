---
name: github-profile-optimizer
description: One-stop GitHub profile optimization — show users real profile examples (2 per style), let them pick style/language/audience in one window, then automate everything else: read public repos & PRs, generate Profile README (username/username repo) matching the chosen layout, write bio and contribution snake, verify links/images, guide gh auth and publish. Use when user says “optimize/beautify GitHub profile”, “profile looks bad”, “write profile README”, “GitHub homepage like the pros”, “update GitHub bio” — even without mentioning README. Not for project READMEs or finding open-source contributions. | 一站式优化 GitHub 个人主页：先给用户看几种风格的真实主页（每种两个，点开即上线后的样子），用户在一个窗口里选风格、语言、给谁看,其余全部自动完成——读取公开仓库、PR 和本地简历，按所选主页的版式写出 Profile README（同名仓库）、简介和贡献贪吃蛇，自检后引导 gh 授权并发布。用户说”优化/美化/做一个 GitHub 主页””主页不好看””写 profile README””像大佬那样的 GitHub 首页””改 GitHub 简介”时都应使用，即使没提 README 这个词。不用于给某个具体项目写 README，也不用于找开源项目贡献。
---

# GitHub Profile Optimizer

**English**: Transform your GitHub profile into a pro-level showcase in one conversation.

**中文**：一句话，让你的 GitHub 主页变成大佬同款。

---

## English Workflow

This skill automates GitHub profile optimization through a guided conversation:

1. **Style Selection**: Show 4 style categories with real live examples. User picks one or provides their own reference URL.
2. **Customization**: Ask language preference (mixed EN/ZH, full English, full Chinese), target audience (peers, recruiters, open-source community), and promotional credit opt-in.
3. **Data Gathering**: Read user's public repos, merged PRs, and local resume (if provided) via `gh` CLI. Extract representative projects and tech stack.
4. **Generation**: Write Profile README matching the chosen style's layout, craft concise bio (3–5 keywords), set up daily contribution snake via GitHub Actions.
5. **Validation**: Verify all external images return 200, check repo/PR links exist. Backup old profile before publishing.
6. **Authorization & Publishing**: Guide user through `gh auth` device flow, create/update `username/username` repo, update bio, enable snake workflow.
7. **Satisfaction Check**: Ask if user wants adjustments (simpler, different style, rollback to backup).

**Key Rules**:
- Content is fact-based: merged PRs only, no invented stats, exclude private repos.
- Layout follows chosen example's structure (section order, image services, formatting).
- Sections not applicable to user (e.g., blog, npm packages) are replaced with relevant content (projects, PRs, tech stack).
- Personal info (school, city, email, phone) only included if user explicitly requests it in conversation.
- Representative projects: 3–6 public, non-fork repos aligned with user's main tech direction.
- Bio is concise: 3–5 keywords separated by “ · “, fitting in ~60 characters (e.g., `Building LLM Agents · Multi-Agent · RAG`).

**Commands**:
```bash
gh api users/<login>
gh repo list <login> --limit 100 --json name,description,primaryLanguage,stargazerCount,isFork,isPrivate
gh search prs --author <login> --limit 30 --json repository,title,state,url
gh api repos/<example-owner>/<example-repo>/readme -H “Accept: application/vnd.github.raw”  # Fetch example README source
```

For detailed Chinese workflow, see below.

---

# GitHub 主页优化（中文详细流程）

GitHub 主页右边显示的内容，来自同名仓库 `<login>/<login>` 的 README.md；左边是头像和简介（bio）。花哨效果都是 README 里引用的图片服务或 GitHub Actions 生成的图。

用户多半是小白，不知道主页能做成什么样，所以不问“想要什么风格”这类抽象问题，直接给真实主页让他看着选。能读到的都自己读；只有判断不了的才问，问的时候给选项。

用户参与只有三处：在一个窗口里回答四个问题（风格、语言、给谁看、推广小字）→ 授权（即同意发布）→ 说满不满意。

## 流程

### 1. 一次问完四个问题

用一个提问窗口同时问四个问题（Claude Code 用 AskUserQuestion，一次传 4 个 question）；没有提问工具就在一条回复里列出四个带编号的问题，让用户一条消息回完。授权放到后面第 4 步。

1. **选风格**：选项是 [references/showcase.md](references/showcase.md) 里的 4 种风格，每个选项的说明里写上两个样例主页的完整网址，用户可以复制打开看。用户想照别的主页做，可以在“其他”里贴链接。
2. **选语言**：中英混排（标题英文、正文中文）/ 全英文 / 全中文。用户仓库说明以中文为主时，中英混排放第一并标推荐。
3. **给谁看**：主页主要给谁看？技术同行（默认，标推荐）/ 招聘方 / 开源社区。
4. **推广小字**：这个工具免费开源，愿不愿意在主页最底部加一行小字帮作者推广？选项：加 / 不加。

样例链接两处都放：弹窗前的正文里列一遍可点击的完整链接（一行一个），弹窗选项说明里也写上网址方便复制。

**四个问题每次都要问**，不要因为用户选了自己现在的主页、之前有过的设置或历史提交就跳过。

用户名：`gh auth status` 已登录就直接用；否则提问窗口已满，在回复正文里顺带请用户报一下 GitHub 用户名。

用户同意推广，就在 README 末尾加：

```html
<p align="center"><sub>Made with <a href="https://github.com/mizane446/github-profile-optimizer">github-profile-optimizer</a></sub></p>
```

### 2. 读数据

```bash
gh api users/<login>
gh repo list <login> --limit 100 --json name,description,primaryLanguage,stargazerCount,isFork,isPrivate
gh search prs --author <login> --limit 30 --json repository,title,state,url
gh api repos/<样例>/<样例>/readme -H "Accept: application/vnd.github.raw"   # 所选样例的源码，直接返回原文
```

- 没装 gh：读数据可以改用 `curl https://api.github.com/...`（同样的路径），发布前再装 gh（见第 4 步）。
- 读候选代表项目 README 的前几十行，弄清它做什么、用了什么技术。
- 外部 PR 只算别人仓库里的，记下 open / merged。
- 用户给了简历路径，或当前目录有个人档案（如 `关于我.md`、简历文件），读来补技术方向；读之前告诉用户读的是哪个文件。主页是公开的：电话、邮箱、学校、城市等个人信息不从简历里搬，用户在对话里明确要写才写。
- 信息不够写出像样内容时（如公开项目少于 2 个、看不出技术方向），问用户一句：想突出哪些项目、做什么方向。

### 3. 生成

按样例的版式写：板块顺序、排版方式、用到的图片服务都跟它一致。版式细节和图片服务写法见 [references/services.md](references/services.md)。

- **板块对不上就替换**：样例有、用户没有的板块（如发布记录、博客、大量统计数字），换成同位置、用户有数据的内容（代表项目、开源 PR、技术栈）。
- **用户选的是自己现在的主页**：版式不变，只更新过时内容（PR 合并了、有新项目），语言和推广小字按第 1 步的回答来；没有变化就告诉用户不用改，结束。
- **内容规则**：按第 1 步“给谁看”的回答写。技术同行：讲做了什么、怎么做的，不写“求职”“实习”；招聘方：定位语和代表项目突出岗位方向，可以写在找什么机会，项目写清楚个人负责的部分；开源社区：突出开源 PR、维护的项目和欢迎协作的方式。个人信息（学校、城市、邮箱）只写用户在对话里要求写的；代表项目 3—6 个，挑公开、非 fork、和主方向一致的；定位语一句话，放在 README 里。
- **简介（bio）单独写**：3—5 个关键词用“ · ”分隔，一行能放下（约 60 字符内），例：`Building LLM Agents · Multi-Agent · RAG · AI pipelines`。不要把定位语长句塞进简介，左边栏很窄，长句会挤成好几行。
- **只写有依据的事实**：数字、经历都要来自仓库、PR 或简历；PR 是 open 就写“review 中”，merged 才写“已合并”；私有仓库不出现。
- 不照抄样例的文字、图片和个人内容。

产物放 `<工作目录>/github-profile/`：`README.md`、`bio.txt`。这个文件夹已经存在时，先把里面的旧文件移进这个文件夹下的 `旧版/`（文件名带日期），再按所选样例重新生成，不要在旧 README 上改；只有用户选的是自己现在的主页时，才在旧内容上更新。用到贪吃蛇时把 [assets/snake.yml](assets/snake.yml) 放到 `.github/workflows/snake.yml`。工作目录默认用户的代码项目目录。

发布前自检：README 里每张外部图片请求返回 200（如 `curl -s -o /dev/null -w "%{http_code}" <url>`；Windows PowerShell 用 `curl.exe`，`-o NUL`）（贪吃蛇图发布后才有，跳过），仓库和 PR 链接都存在。

### 4. 授权即确认

在结束本轮的回复里写清要做什么，再给授权步骤：

> 接下来会：新建（或更新）公开仓库 `<login>/<login>`，简介改成“<bio>”，开启每天自动更新的贪吃蛇。线上旧 README 会先备份，不满意可以撤回。

- 没装 gh：Windows `winget install --id GitHub.cli`，macOS `brew install gh`，其他见 https://cli.github.com 。装完重开终端。
- 在后台运行授权命令，输出写到文件，几秒后从文件里读出验证码。命令不能等键盘输入：bash 结尾加 `< /dev/null`，PowerShell 前面加 `$null |`。
  - 没登录：`gh auth login -h github.com -p https -w -s user,workflow`
  - 已登录但 scopes 缺 `user` 或 `workflow`：`gh auth refresh -h github.com -s user,workflow`
- 告诉用户：打开 https://github.com/login/device → 填验证码 `XXXX-XXXX` → 点 Authorize，授权完就开始发布。
- 权限已经齐了：没有授权这一步，改为问一句“确认发布吗”。

演练模式（用户说“只生成不发布”“dry run”）到这一步之前结束，报告产物路径。

### 5. 发布

```bash
cd <工作目录>/github-profile
# 仓库已存在：先 gh repo clone <login>/<login> 到这里，旧 README 另存为本地 旧版/README-<日期>.md
git add README.md .github/workflows/snake.yml
git commit -m "Update profile README"
gh repo create <login>/<login> --public --source . --push    # 仓库已存在则 git push
gh api -X PATCH user -f bio="<bio>"
gh workflow run snake.yml -R <login>/<login>
```

### 6. 收尾

- 给出 `https://github.com/<login>`，说明做了什么。贪吃蛇 workflow 跑完（约 1 分钟）才会显示。
- 置顶仓库没有 API，告诉用户手动设：主页 Popular repositories 右上角 → Customize your pins → 勾选代表项目。
- 问满不满意（同第 1 步的提问方式）：满意 / 更简洁一些 / 换一种风格 / 撤回到旧版。要改就改完直接再推送；撤回就把备份的旧 README 推回去、简介改回原值。

## 界面提示

- 提问工具（如 Claude Code 的 AskUserQuestion）里的链接点不了，所以链接要同时写在正文里。验证码写在结束本轮的回复里。
- 链接写完整 `https://...`，一行一个。
