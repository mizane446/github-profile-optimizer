# github-profile-optimizer

一个 Claude Code skill：对话一句“帮我美化 GitHub 主页”，看几个真实主页样例选一个风格，剩下的全自动——读你的仓库和 PR、照着样例版式写 Profile README、改简介、加贡献贪吃蛇，授权后直接发布。

## 安装

把整个文件夹放到 `~/.claude/skills/github-profile-optimizer/`，重开 Claude Code。

需要 [gh](https://cli.github.com)（GitHub 命令行工具）；没装的话 skill 会在发布前带你装。

## 使用

对 Claude 说：

> 帮我美化一下 GitHub 主页

然后：

1. 打开 Claude 给的样例链接，回一个编号（或贴你看中的任意主页链接）
2. 选语言：中英混排 / 全英文 / 全中文
3. 打开授权网址、填验证码（这一步就是同意发布）
4. 看发布后的主页，说满不满意；不满意可以继续改或撤回

只想先看看生成结果：说“只生成不发布”。

## 样例风格

| 风格 | 例子 |
| --- | --- |
| 综合展示 | https://github.com/mizane446 |
| 极简文字 | https://github.com/tw93 |
| 数据卡片 | https://github.com/anuraghazra |
| 动图表情 | https://github.com/Prince-Shivaram |

更多风格见 [awesome-github-profile-readme](https://github.com/abhisheknaiidu/awesome-github-profile-readme)。

## 没有 Claude？

可以用填表工具手动生成：https://rahuldkjain.github.io/github-profile-readme-generator/

## 许可

MIT
