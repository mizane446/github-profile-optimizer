<div align="center">

# GitHub 主页优化.skill

**一句话，让你的 GitHub 主页变成大佬同款。**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Claude Code Skill](https://img.shields.io/badge/Claude%20Code-Skill-d97757)](https://docs.claude.com/en/docs/claude-code/skills)
[![语言](https://img.shields.io/badge/语言-中文-blue)](#使用)

不懂 README、不懂徽章和动画也没关系：看几个真实主页，选一个喜欢的，<br>
剩下的——读你的仓库和 PR、写介绍、改简介、加贡献贪吃蛇、发布——全交给 AI。

[看效果](#看效果) · [安装](#安装) · [使用](#使用) · [风格样例](#风格样例)

</div>

---

## 看效果

<p align="center">
  <img src="assets/before-after.png" alt="改前改后对比" width="100%">
</p>

右边就是作者自己的主页，用这个 skill 生成：https://github.com/mizane446

## 它怎么做

| 你做的 | AI 做的 |
| --- | --- |
| **① 选风格**：点开样例链接，回一个编号 | 发 4 种风格的真实主页，点开就是做好后的样子 |
| **② 选语言**：中英混排 / 全英文 / 全中文 | 读你的公开仓库、开源 PR，照着样例版式写 README 和简介，检查图片和链接 |
| **③ 授权**：打开网址、填验证码 | 建同名仓库、改简介、开启每日更新的贪吃蛇 |

内容只写有出处的事实：PR 没合并就写“review 中”，不编数字，私有仓库不出现。旧主页会先备份，不满意可以撤回。

## 风格样例

<p align="center">
  <img src="assets/styles.png" alt="四种风格" width="100%">
</p>

| 风格 | 主页 |
| --- | --- |
| 1 综合展示 | https://github.com/mizane446 · https://github.com/DenverCoder1 |
| 2 极简文字 | https://github.com/tw93 · https://github.com/simonw |
| 3 数据卡片 | https://github.com/anuraghazra · https://github.com/ouuan |
| 4 动图表情 | https://github.com/Prince-Shivaram · https://github.com/innng |

还想看更多（代码风、游戏互动、二次元……）：[awesome-github-profile-readme](https://github.com/abhisheknaiidu/awesome-github-profile-readme)，看中哪个把链接贴给 AI 就行。

## 安装

最省事：把这句话发给你的 AI Agent（Claude Code、Codex 等）：

> 帮我安装这个 skill：https://github.com/mizane446/github-profile-optimizer ，装好后用它美化我的 GitHub 主页

也可以手动克隆到 skills 文件夹：

```bash
# Claude Code
git clone https://github.com/mizane446/github-profile-optimizer ~/.claude/skills/github-profile-optimizer

# Codex
git clone https://github.com/mizane446/github-profile-optimizer ~/.agents/skills/github-profile-optimizer
```

如果 Agent 没认出这个 skill，重开一下。发布要用 [gh](https://cli.github.com)，没装的话 skill 会带你装。

## 使用

对 Agent 说：

> 帮我美化一下 GitHub 主页

只想先看生成结果、不发布：

> 帮我美化一下 GitHub 主页，只生成不发布

## 没有 AI Agent？

可以用填表工具手动生成：https://rahuldkjain.github.io/github-profile-readme-generator/

## 许可

[MIT](LICENSE)。样例截图来自上述公开主页，版权归各自作者。
