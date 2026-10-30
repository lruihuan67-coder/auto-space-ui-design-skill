---
name: auto-space-ui-design-install
description: 凹凸-UI 技能索引与安装器。从 GitHub（lruihuan67-coder/auto-space-ui-design-skill）下载凹凸时空品牌 UI 设计技能包 auto-space-ui-design-skill.zip（品牌 Token 与 React 组件库 starter），落地为本地技能，并可注册每天自动拉取最新版的定时任务。当用户提到安装/更新/同步 凹凸-UI、凹凸时空 UI 技能、auto-space-ui-design、品牌组件库、从 GitHub 拉 UI 技能包时使用。
---

# 凹凸-UI 技能索引与安装器

本 skill 只负责 UI 设计技能 `auto-space-ui-design`。品牌演示稿技能由独立仓库的
`auto-space-ppt-install` 负责，两者配置、日志、定时任务完全分开。

| 资源 | 位置 |
| --- | --- |
| 仓库 | `lruihuan67-coder/auto-space-ui-design-skill`（分支 `main`） |
| 技能包 | `<repo>/auto-space-ui-design-skill.zip` |
| 技能源码 | `<repo>/skills/auto-space-ui-design/` |
| 安装后位置 | `~/.agents/skills/auto-space-ui-design/` |

## 一键安装 / 更新

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File "$env:USERPROFILE\.agents\skills\auto-space-ui-design-install\scripts\install.ps1"
```

首次从零安装：

```powershell
$repo = "lruihuan67-coder/auto-space-ui-design-skill"
$dir  = "$env:USERPROFILE\.agents\skills\auto-space-ui-design-install"
New-Item -ItemType Directory -Path "$dir\scripts" -Force | Out-Null
Invoke-WebRequest "https://raw.githubusercontent.com/$repo/main/skills/auto-space-ui-design-install/scripts/install.ps1"     -OutFile "$dir\scripts\install.ps1"
Invoke-WebRequest "https://raw.githubusercontent.com/$repo/main/skills/auto-space-ui-design-install/scripts/daily-sync.ps1" -OutFile "$dir\scripts\daily-sync.ps1"

powershell -NoProfile -ExecutionPolicy Bypass -File "$dir\scripts\install.ps1" -Repo $repo -WithSchedule -ScheduleTime 09:10
```

| 参数 | 默认值 | 说明 |
| --- | --- | --- |
| `-Repo` | `lruihuan67-coder/auto-space-ui-design-skill` | GitHub 仓库 |
| `-Ref` | `main` | 分支或 tag |
| `-SkillName` | `auto-space-ui-design` | 技能目录名 |
| `-AssetName` | `auto-space-ui-design-skill.zip` | 压缩包文件名 |
| `-SkillHome` | `~/.agents/skills` | 技能落地目录 |
| `-WithSchedule` | 关 | 注册每日同步（`AutoSpaceUI-SkillSync`） |
| `-ScheduleTime` | `09:10` | 每日同步时间（与 PPT 的 09:00 错峰） |
| `-SourceZip` | — | 直接用本地压缩包，跳过下载（离线安装） |
| `-Force` | 关 | 忽略哈希比对，强制重装 |

本安装器不部署智能体，UI 技能由当前对话按需加载使用。

## 脚本行为

1. 下载 `https://raw.githubusercontent.com/<repo>/<ref>/auto-space-ui-design-skill.zip`，失败回退 `github.com/<repo>/raw/...`。
2. 算 SHA256 与 `installed.json` 比对，一致则跳过（`-Force` 强制）。
3. 解压校验必须存在 `auto-space-ui-design/SKILL.md`。
4. 旧版移到 `<SkillHome>/.backup/auto-space-ui-design-<时间戳>`，再复制新版。
5. 写 `installed.json` 与 `config.json`（只登记本技能）。
6. 可选：注册 `AutoSpaceUI-SkillSync`。

日志：`<InstallerDir>/logs/install-YYYY-MM.log`、`daily-sync.log`。

## 每日自动同步

```powershell
schtasks /Query /TN "AutoSpaceUI-SkillSync" /V /FO LIST
schtasks /Run   /TN "AutoSpaceUI-SkillSync"
schtasks /Delete /TN "AutoSpaceUI-SkillSync" /F
Get-Content "$env:USERPROFILE\.agents\skills\auto-space-ui-design-install\logs\daily-sync.log" -Tail 20
```

## 校验

```powershell
Test-Path "$env:USERPROFILE\.agents\skills\auto-space-ui-design\SKILL.md"
Test-Path "$env:USERPROFILE\.agents\skills\auto-space-ui-design\assets\ui-starter\package.json"
Get-Content "$env:USERPROFILE\.agents\skills\auto-space-ui-design-install\installed.json"
```

## 常见问题

- **404**：确认仓库为 public，`-Ref` 分支存在，`auto-space-ui-design-skill.zip` 在仓库根目录。
- **解压后多套一层**：包内顶层目录必须是 `auto-space-ui-design/`；脚本按 `-SkillName` 校验。
- **不生效**：确认 `~/.agents/skills/auto-space-ui-design/SKILL.md` 存在，重启 SpaceAgents。
- **starter 跑不起来**：`npm ci`，Node 需 18+。
