# 凹凸时空 UI 设计技能（auto-space-ui-design）

凹凸时空（AUTO SPACE）品牌 UI 设计技能的源码仓库，同时提供技能包与索引/安装 skill。

| 路径 | 说明 |
| --- | --- |
| `auto-space-ui-design-skill.zip` | 可下载的技能包（顶层目录 `auto-space-ui-design/`） |
| `skills/auto-space-ui-design/SKILL.md` | 技能说明、两种工作模式与交付要求 |
| `skills/auto-space-ui-design/references/design-rules.md` | 品牌规范：色彩、字号 30/26/16/14/12、4px 间距、按钮 36/44/52、图标 16/24 + 1.5 描边 |
| `skills/auto-space-ui-design/assets/ui-starter/` | 可运行的 React + TypeScript + Tailwind 4 starter：组件、Token、Logo、后台示例与组件展示页 |
| `skills/auto-space-ui-design/assets/foundation-reference.png` | 色彩与品牌气质参考图 |
| `skills/auto-space-ui-design/agents/openai.yaml` | 智能体配置 |
| `skills/auto-space-ui-design-install/` | 索引/安装 skill：从本仓库下载技能包并安装 |

## 使用（手动）

- 作为技能使用：把 `skills/auto-space-ui-design/` 整个目录放到技能目录（例如 `~/.agents/skills/auto-space-ui-design/`），或直接解压 `auto-space-ui-design-skill.zip`。
- 作为组件库使用：复制 `assets/ui-starter/` 到项目里，运行 `npm ci` 与 `npm run dev`；`/` 是后台示例，`/library` 是组件展示页。
- 组件公开导出见 `assets/ui-starter/src/index.ts`，Token 见 `src/styles.css`，品牌 Logo 见 `public/brand/`。

## 使用（索引 skill：安装 + 每日自动更新）

```powershell
$repo = "lruihuan67-coder/auto-space-ui-design-skill"
$dir  = "$env:USERPROFILE\.agents\skills\auto-space-ui-design-install"
New-Item -ItemType Directory -Path "$dir\scripts" -Force | Out-Null
Invoke-WebRequest "https://raw.githubusercontent.com/$repo/main/skills/auto-space-ui-design-install/scripts/install.ps1"     -OutFile "$dir\scripts\install.ps1"
Invoke-WebRequest "https://raw.githubusercontent.com/$repo/main/skills/auto-space-ui-design-install/scripts/daily-sync.ps1" -OutFile "$dir\scripts\daily-sync.ps1"

# 安装技能 + 每日同步（09:10，与 PPT 的 09:00 错峰）
powershell -NoProfile -ExecutionPolicy Bypass -File "$dir\scripts\install.ps1" -Repo $repo -WithSchedule -ScheduleTime 09:10
```

- 任务名：`AutoSpaceUI-SkillSync`，每天 09:10 拉取最新 `auto-space-ui-design-skill.zip`
- 哈希没变就跳过；有变化才覆盖，旧版备份在 `~/.agents/skills/.backup/`
- 离线安装：加 `-SourceZip <本地 zip 路径>`
- 查看：`schtasks /Query /TN "AutoSpaceUI-SkillSync" /V /FO LIST`

本安装器不部署智能体。PPT 技能在独立仓库 `auto-space-ppt-skill`，由它自己的索引 skill 管理，与本仓库互不影响。

## 发布新版本

替换 `auto-space-ui-design-skill.zip` 与 `skills/auto-space-ui-design/` 后提交推送，第二天的定时任务自动装上。
