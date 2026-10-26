# 常用主页图片服务

都是公共实例，可能限流或收费。每次生成后用 curl 检查，失败就删或换备选。状态为 2026-10 实测。

| 用途 | 写法 | 状态 |
| --- | --- | --- |
| 打字动画 | `https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=24&duration=3000&pause=800&color=2F81F7&center=true&vCenter=true&width=620&lines=Line+one;Line+two` | 可用。只写英文，中文字体渲染不稳 |
| 技术图标墙 | `https://skillicons.dev/icons?i=python,fastapi,redis,docker&perline=12` | 可用。图标名见 skillicons.dev，没有的技术写成文字 |
| 徽章 | `https://img.shields.io/badge/<名字>-<颜色>?logo=<simpleicons名>&logoColor=white` | 可用 |
| 统计卡片 | `https://github-readme-stats.vercel.app/api?username=<login>&show_icons=true` | 可用但公共实例常限流；数据少时别用 |
| 连续提交 | `https://streak-stats.demolab.com?user=<login>` | 可用 |
| 活动曲线 | `github-readme-activity-graph.vercel.app` | 2026-10 返回 402，不用 |
| 贪吃蛇 | 由 assets/snake.yml（Platane/snk@v3）生成到 output 分支 | 可用，需 workflow 权限 |

贪吃蛇 README 写法：

```html
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/<login>/<login>/output/github-snake-dark.svg" />
  <img src="https://raw.githubusercontent.com/<login>/<login>/output/github-snake.svg" alt="Contribution snake" />
</picture>
```

风格样例见 showcase.md。
