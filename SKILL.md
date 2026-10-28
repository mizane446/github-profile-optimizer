---
name: github-profile-optimizer
description: 一站式优化 GitHub 个人主页：先给用户看几种风格的真实主页（每种两个，点开即上线后的样子），用户回个编号或贴链接选风格、选语言，其余全部自动完成——读取公开仓库、PR 和本地简历，按所选主页的版式写出 Profile README（同名仓库）、简介和贡献贪吃蛇，自检后引导 gh 授权并发布。用户说“优化/美化/做一个 GitHub 主页”“主页不好看”“写 profile README”“像大佬那样的 GitHub 首页”“改 GitHub 简介”时都应使用，即使没提 README 这个词。不用于给某个具体项目写 README，也不用于找开源项目贡献。
---

# GitHub 主页优化

GitHub 主页右边显示的内容，来自同名仓库 `<login>/<login>` 的 README.md；左边是头像和简介（bio）。花哨效果都是 README 里引用的图片服务或 GitHub Actions 生成的图。

用户多半是小白，不知道主页能做成什么样，所以不问“想要什么风格”这类抽象问题，直接给真实主页让他看着选。能读到的都自己读；只有判断不了的才问，问的时候给选项。

用户参与只有四处：选样例 → 选语言 → 授权（即同意发布）→ 说满不满意。

## 流程

### 1. 给样例

把 [references/showcase.md](references/showcase.md) 里“发给用户的内容”写在**结束本轮的回复**里，请用户回编号（可混搭）或贴看中的主页链接。

用户名：`gh auth status` 已登录就直接用；否则顺带问一句 GitHub 用户名。

### 2. 选语言

问用户（有提问工具就用，没有就在回复里列编号选项，让用户回编号）：中英混排（标题英文、正文中文）/ 全英文 / 全中文。用户仓库说明以中文为主时，中英混排放第一并标推荐。

### 3. 读数据

```bash
gh api users/<login>
gh repo list <login> --limit 100 --json name,description,primaryLanguage,stargazerCount,isFork,isPrivate
gh search prs --author <login> --limit 30 --json repository,title,state,url
gh api repos/<样例>/<样例>/readme -H "Accept: application/vnd.github.raw"   # 所选样例的源码，直接返回原文
```

- 没装 gh：读数据可以改用 `curl https://api.github.com/...`（同样的路径），发布前再装 gh（见第 5 步）。
- 读候选代表项目 README 的前几十行，弄清它做什么、用了什么技术。
- 外部 PR 只算别人仓库里的，记下 open / merged。
- 用户给了简历路径，或当前目录有个人档案（如 `关于我.md`、简历文件），读来补技术方向；读之前告诉用户读的是哪个文件。主页是公开的：电话、邮箱、学校、城市等个人信息不从简历里搬，用户在对话里明确要写才写。
- 信息不够写出像样内容时（如公开项目少于 2 个、看不出技术方向），问用户一句：想突出哪些项目、做什么方向。

### 4. 生成

按样例的版式写：板块顺序、排版方式、用到的图片服务都跟它一致。版式细节和图片服务写法见 [references/services.md](references/services.md)。

- **板块对不上就替换**：样例有、用户没有的板块（如发布记录、博客、大量统计数字），换成同位置、用户有数据的内容（代表项目、开源 PR、技术栈）。
- **用户选的是自己现在的主页**：版式不变，只更新过时内容（PR 合并了、有新项目）；没有变化就告诉用户不用改，结束。
- **内容规则**：默认写给技术同行，不写“求职”“实习”；个人信息（学校、城市、邮箱）只写用户在对话里要求写的；代表项目 3—6 个，挑公开、非 fork、和主方向一致的；定位语一句话，同时用作简介（≤160 字符）。
- **只写有依据的事实**：数字、经历都要来自仓库、PR 或简历；PR 是 open 就写“review 中”，merged 才写“已合并”；私有仓库不出现。
- 不照抄样例的文字、图片和个人内容。

产物放 `<工作目录>/github-profile/`：`README.md`、`bio.txt`；用到贪吃蛇时把 [assets/snake.yml](assets/snake.yml) 放到 `.github/workflows/snake.yml`。工作目录默认用户的代码项目目录。

发布前自检：README 里每张外部图片请求返回 200（如 `curl -s -o /dev/null -w "%{http_code}" <url>`；Windows PowerShell 用 `curl.exe`，`-o NUL`）（贪吃蛇图发布后才有，跳过），仓库和 PR 链接都存在。

### 5. 授权即确认

在结束本轮的回复里写清要做什么，再给授权步骤：

> 接下来会：新建（或更新）公开仓库 `<login>/<login>`，简介改成“<bio>”，开启每天自动更新的贪吃蛇。线上旧 README 会先备份，不满意可以撤回。

- 没装 gh：Windows `winget install --id GitHub.cli`，macOS `brew install gh`，其他见 https://cli.github.com 。装完重开终端。
- 在后台运行授权命令，输出写到文件，几秒后从文件里读出验证码。命令不能等键盘输入：bash 结尾加 `< /dev/null`，PowerShell 前面加 `$null |`。
  - 没登录：`gh auth login -h github.com -p https -w -s user,workflow`
  - 已登录但 scopes 缺 `user` 或 `workflow`：`gh auth refresh -h github.com -s user,workflow`
- 告诉用户：打开 https://github.com/login/device → 填验证码 `XXXX-XXXX` → 点 Authorize，授权完就开始发布。
- 权限已经齐了：没有授权这一步，改为问一句“确认发布吗”。

演练模式（用户说“只生成不发布”“dry run”）到这一步之前结束，报告产物路径。

### 6. 发布

```bash
cd <工作目录>/github-profile
# 仓库已存在：先 gh repo clone <login>/<login> 到这里，旧 README 另存为本地 旧版/README-<日期>.md
git add README.md .github/workflows/snake.yml
git commit -m "Update profile README"
gh repo create <login>/<login> --public --source . --push    # 仓库已存在则 git push
gh api -X PATCH user -f bio="<bio>"
gh workflow run snake.yml -R <login>/<login>
```

### 7. 收尾

- 给出 `https://github.com/<login>`，说明做了什么。贪吃蛇 workflow 跑完（约 1 分钟）才会显示。
- 置顶仓库没有 API，告诉用户手动设：主页 Popular repositories 右上角 → Customize your pins → 勾选代表项目。
- 问满不满意（同第 2 步的提问方式）：满意 / 更简洁一些 / 换一种风格 / 撤回到旧版。要改就改完直接再推送；撤回就把备份的旧 README 推回去、简介改回原值。

## 界面提示

- 提问工具（如 Claude Code 的 AskUserQuestion）里的链接点不了，写在提问前面的正文用户也可能看不到。链接和验证码要写在结束本轮的回复里。
- 链接写完整 `https://...`，一行一个。
