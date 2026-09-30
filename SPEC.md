# SPEC — GitHub 个人主页（精简版 v2）

## 目标
精简后的 Profile README：只保留 5 个版块，删除其余全部装饰与数据图。
资源策略：能在线热链的走第三方服务；唯一本地生成资产是贪吃蛇 SVG。

## 保留版块（自上而下，hr.gif 分隔）
| # | 版块 | 实现 | 资产来源 |
|---|------|------|----------|
| 1 | 横幅 + 访问量 | capsule-render 波浪横幅 + komarev 访问计数徽章 | 在线 |
| 2 | Snake Animation | Platane/snk 生成，`<picture>` 亮/暗双主题自适应 | 本地 `profile-snake-contrib/`（snake.yml 生成） |
| 3 | About Me | readme-typing-svg 打字自我介绍 + 等级卡片 | 在线（typing SVG + awesome-github-stats） |
| 4 | Skills & Tools | skillicons.dev 图标墙（18 个图标，取自原"already"技能表） | 在线 |
| 5 | Contribution Stats | profile-summary-cards 三卡（stats/repos-per-language/most-commit-language）+ streak-stats 连续提交卡，tokyonight 主题 | 在线 |
| 6 | WakaTime | wakatime.com/share 两张 SVG（视图时自动刷新） | 在线（注意 WakaTime 账号名是 `@kaunghl`，非 `kuanghl`，勿"修正"） |
| 7 | 笑话 + 名人名言 | readme-jokes + quotes-github-readme，并排两列表格 | 在线 |

## 移除版块（README 对应代码段全部删除）
- Header：博客/访问量/代码行数徽章、打字 Hello、coding.gif
- activity-graph 活动图（github-readme-activity-graph）
- 奖杯（profile-trophy）、github-readme-stats 统计卡 + top-langs
- Skills 全节（already / plan / tool / skillicons）
- knowledge map（man.png、man_run.png、技术树）
- GitHub metrics 全节（base/stars/habits/skyline 等本地 SVG）
- 3D 贡献图、streak 连续提交、left/right 装饰图

## Workflow（清理后只保留一个）
| 文件 | 动作 |
|---|---|
| `snake.yml` | 保留并修缮（每日 0 点 + 手动 + push master 触发，Platane/snk/svg-only@v3 → `profile-snake-contrib/`）；2026-09-30 将 push 用的 `crazy-max/ghaction-github-pages` 从 v3.1.0 升到 v4.2.0（v3 系基于已弃用的 Node 16） |
| `metrics.yml` | 删除 |
| `profile-3d.yml` | 删除 |
| `waka.yml` | 删除（WakaTime 改用 wakatime.com/share 在线链接，视图时自动刷新，无需本地生成） |

## 目录结构（清理后）
```
.
├── README.md
├── SPEC.md
├── assets/hr.gif                  # 仅保留分隔图，其余 assets 图片删除
├── profile-snake-contrib/         # 唯一本地生成目录，勿手改
└── .github/workflows/snake.yml
```
删除目录：`github-metrics/`、`profile-3d-contrib/`；
删除文件：`assets/` 下 coding.gif、man.png、man_run.png、bar_graph.png 等未被保留版块引用的图片。

## 维护约定
- 改主页只动 `README.md`；`profile-snake-contrib/` 由 workflow 覆盖。
- 在线服务（typing、wakatime、jokes、quotes、awesome-github-stats）挂掉只影响单块，不级联。
- CDN 基址统一为 `https://cdn.jsdelivr.net/gh/kuanghl/<repo>/<path>`。

## 决策记录（2026-09-30 已执行）
1. **贪吃蛇 CDN 指向** → 已改指本仓库 `gh/kuanghl/kuanghl/profile-snake-contrib/...`，
   单点维护。`k` 仓库的 snake workflow 可停用（仓库侧操作）。
2. **About Me 等级卡片** → 保留。实测 awesome-github-stats 返回 200，服务存活，
   原"疑似下线"假设不成立。
3. **WakaTime** → 已删 `github-readme-stats.vercel.app/api/wakatime` 卡片（实测 503），
   保留 wakatime.com/share 两张 SVG（实测 200）。

## 二期扩展（2026-09-30 已执行，参考 awesome-github-readme-profile 模板）
- 新增：capsule-render 横幅、komarev 访问量、Skills & Tools（skillicons 18 图标）、
  Contribution Stats（profile-summary-cards 三卡 + streak-stats）。
- 贪吃蛇版块标题改为 `Snake Animation`。
- 实测：profile-summary-cards / streak-stats / komarev / skillicons / capsule-render 全部 200；
  经典 `github-readme-stats.vercel.app`（stats + top-langs）确认 503 已死，**勿再使用**。
- snake.yml 修缮：snk@v3 即当前最新大版本（无需动）；ghaction-github-pages v3.1.0 → v4.2.0。
