# 凹凸时空 UI 设计技能（auto-space-ui-design）

凹凸时空（AUTO SPACE）品牌 UI 设计技能的源码仓库。本仓库只保留技能内容，不包含安装器、每日自动更新脚本或打包产物。

| 路径 | 说明 |
| --- | --- |
| `skills/auto-space-ui-design/SKILL.md` | 技能说明、两种工作模式与交付要求 |
| `skills/auto-space-ui-design/references/design-rules.md` | 品牌规范：色彩、字号 30/26/16/14/12、4px 间距、按钮 36/44/52、图标 16/24 + 1.5 描边 |
| `skills/auto-space-ui-design/assets/ui-starter/` | 可运行的 React + TypeScript + Tailwind 4 starter：组件、Token、Logo、后台示例与组件展示页 |
| `skills/auto-space-ui-design/assets/foundation-reference.png` | 色彩与品牌气质参考图 |
| `skills/auto-space-ui-design/agents/openai.yaml` | 智能体配置 |

## 使用

- 作为技能使用：把 `skills/auto-space-ui-design/` 整个目录放到你的技能目录（例如 `~/.agents/skills/auto-space-ui-design/`）。
- 作为组件库使用：复制 `assets/ui-starter/` 到项目里，运行 `npm ci` 与 `npm run dev`；`/` 是后台示例，`/library` 是组件展示页。
- 组件公开导出见 `assets/ui-starter/src/index.ts`，Token 见 `src/styles.css`，品牌 Logo 见 `public/brand/`。

本仓库不再提供在线安装、每日同步或版本清单；更新时直接替换对应源码即可。
