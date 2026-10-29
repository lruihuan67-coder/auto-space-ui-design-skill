# 凹凸时空 UI 技能仓库（auto-space-ui-design-skill）

凹凸时空（AUTO SPACE）品牌 UI 设计技能的独立发布源。与 PPT 技能仓库（`lruihuan67-coder/auto-space-ppt-skill`）完全分离，安装、配置、日志与每日同步互不影响。

| 内容 | 说明 |
| --- | --- |
| `auto-space-ui-design-skill.zip` | 「凹凸-UI」技能压缩包（品牌 Token + React 组件库 starter + 设计规范） |
| `auto-space-ui-design-skill.zip.version` | 压缩包 SHA256 清单，供 `--ensure` / `--status` 快速比对 |
| `skills/auto-space-ui-design/` | 技能展开目录（SKILL.md、references、assets） |
| `skills/auto-space-ui-design-install/` | UI 专属安装器（Python 核心 + Windows/macOS 封装） |

## 安装（Windows）

```powershell
$repo = "lruihuan67-coder/auto-space-ui-design-skill"
$dir  = "$env:USERPROFILE\.agents\skills\auto-space-ui-design-install"
New-Item -ItemType Directory -Path "$dir\scripts" -Force | Out-Null

foreach ($f in "install.ps1","install.py","daily-sync.ps1","daily-sync.py") {
  Invoke-WebRequest "https://raw.githubusercontent.com/$repo/main/skills/auto-space-ui-design-install/scripts/$f" -OutFile "$dir\scripts\$f"
}

powershell -NoProfile -ExecutionPolicy Bypass -File "$dir\scripts\install.ps1" -Repo $repo -WithSchedule -ScheduleTime 09:10
```

## 安装（macOS）

```bash
repo="lruihuan67-coder/auto-space-ui-design-skill"
d="$HOME/.agents/skills/auto-space-ui-design-install/scripts"
mkdir -p "$d"
for f in install.sh install.py daily-sync.sh daily-sync.py; do
  curl -fsSL "https://raw.githubusercontent.com/$repo/main/skills/auto-space-ui-design-install/scripts/$f" -o "$d/$f"
done
bash "$d/install.sh" --repo "$repo" --with-schedule --schedule 09:10
```

安装后：

- 技能：`~/.agents/skills/auto-space-ui-design/`
- 定时任务：Windows 计划任务 `AutoSpaceUI-SkillSync`（每天 09:10）／macOS `~/Library/LaunchAgents/com.auto-space.auto-space-ui-design.sync.plist`
- 旧版本备份：`~/.agents/skills/.backup/`

## 从旧合并仓库迁移

旧仓库 `auto-space-ppt-skill` 已不再提供 UI 技能包。此前从旧仓库安装的机器，需要把安装器的仓库地址切到本仓库，否则每日同步会因 404 失败：

```powershell
# 1) 修改已安装安装器的配置
$cfg = "$env:USERPROFILE\.agents\skills\auto-space-ui-design-install\config.json"
$json = Get-Content $cfg -Raw | ConvertFrom-Json
$json.repo = "lruihuan67-coder/auto-space-ui-design-skill"
$json | ConvertTo-Json -Depth 5 | Set-Content $cfg -Encoding UTF8

# 2) 立即拉取一次新仓库的安装器与技能
powershell -NoProfile -ExecutionPolicy Bypass -File "$env:USERPROFILE\.agents\skills\auto-space-ui-design-install\scripts\install.ps1" `
  -Repo "lruihuan67-coder/auto-space-ui-design-skill"
```

macOS 同理：把 `config.json` 的 `repo` 改为本仓库，然后执行一次 `daily-sync.sh`。

## 手动更新 / 状态检查

```powershell
# Windows
powershell -NoProfile -ExecutionPolicy Bypass -File "$env:USERPROFILE\.agents\skills\auto-space-ui-design-install\scripts\daily-sync.ps1"
powershell -NoProfile -ExecutionPolicy Bypass -File "$env:USERPROFILE\.agents\skills\auto-space-ui-design-install\scripts\install.ps1" -Status
```

```bash
# macOS
bash "$HOME/.agents/skills/auto-space-ui-design-install/scripts/daily-sync.sh"
bash "$HOME/.agents/skills/auto-space-ui-design-install/scripts/install.sh" --status
```

哈希没变化就跳过；有变化才备份并覆盖。

## 每日同步做了什么

```
daily-sync（09:10）
  ├─ 1. 安装器自更新：拉 skills/auto-space-ui-design-install/scripts/ 下的核心脚本
  ├─ 2. 读 config.json（仓库 / 分支 / 技能目录 / 压缩包名）
  ├─ 3. 下载技能压缩包 → 比 SHA256
  └─ 4. 有变化才备份旧版并覆盖安装
```

## 发布新版本

1. 修改 `skills/auto-space-ui-design/` 下的技能资产（组件源码、Token、规范文档）
2. 以固定时间戳重打 `auto-space-ui-design-skill.zip`（顶层目录必须是 `auto-space-ui-design/`）
3. 同步更新 `auto-space-ui-design-skill.zip.version` 为新包 SHA256
4. 提交并推送 `main`

装过该技能的机器会在下一次 09:10 同步时凭 SHA256 变化自动更新。

## 相关仓库

- PPT 技能：`lruihuan67-coder/auto-space-ppt-skill`
