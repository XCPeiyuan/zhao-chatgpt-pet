# 「照」的备用安装方式

原生 v2 安装只需要 `pet.json` 和 `spritesheet.webp`。[Release 安装包](https://github.com/XCPeiyuan/zhao-chatgpt-pet/releases/tag/v1.0.0)的 ZIP 根目录包含这两个文件。

## 自己安装

### Windows

将 ZIP 下载到“下载”文件夹，在 PowerShell 中执行：

```powershell
$zip = Join-Path $env:USERPROFILE 'Downloads\zhao-codex-native-v2.zip'
$stage = Join-Path $env:TEMP ('zhao-codex-' + [guid]::NewGuid().ToString('N'))
Expand-Archive -LiteralPath $zip -DestinationPath $stage

$petDir = Join-Path $env:USERPROFILE '.codex\pets\zhao'
$names = @('pet.json', 'spritesheet.webp')
$existing = @($names | Where-Object { Test-Path -LiteralPath (Join-Path $petDir $_) })
if ($existing.Count -gt 0) { throw "目标文件已存在：$($existing -join ', ')。请先检查，不要覆盖。" }

$manifest = Get-Content -LiteralPath (Join-Path $stage 'pet.json') -Raw | ConvertFrom-Json
if ($manifest.id -ne 'zhao' -or $manifest.spriteVersionNumber -ne 2 -or $manifest.spritesheetPath -ne 'spritesheet.webp') { throw 'pet.json 与此安装包不匹配。' }
if (-not (Test-Path -LiteralPath (Join-Path $stage 'spritesheet.webp'))) { throw '缺少 spritesheet.webp。' }

New-Item -ItemType Directory -Path $petDir -Force | Out-Null
Copy-Item -LiteralPath (Join-Path $stage 'pet.json') -Destination $petDir
Copy-Item -LiteralPath (Join-Path $stage 'spritesheet.webp') -Destination $petDir
Get-Item -LiteralPath (Join-Path $petDir 'pet.json'), (Join-Path $petDir 'spritesheet.webp') | Select-Object Name, Length
```

若 `$petDir` 中其他文件已存在，上面的步骤只复制这两个文件，不会删除或修改其他文件。若任一同名目标文件已存在，脚本会停止；请先检查并决定是否替换。

然后在 Codex 的“设置 → Pets”刷新列表并选择“照”。如果仍未显示，先检查 Codex Desktop 是否使用 WSL 后端；不要擅自切换。切换可能影响依赖 WSL 的工作流，应先得到用户确认。无需修改应用安装目录。

### macOS

```bash
unzip ~/Downloads/zhao-codex-native-v2.zip -d "${TMPDIR:-/tmp}/zhao-codex-native"
pet_dir="$HOME/.codex/pets/zhao"
for name in pet.json spritesheet.webp; do
  if [ -e "$pet_dir/$name" ]; then echo "目标文件已存在：$pet_dir/$name；停止安装，不覆盖。"; exit 1; fi
done
mkdir -p "$pet_dir"
cp "${TMPDIR:-/tmp}/zhao-codex-native/pet.json" "$pet_dir/"
cp "${TMPDIR:-/tmp}/zhao-codex-native/spritesheet.webp" "$pet_dir/"
```

安装后重新打开 Codex，前往 Settings → Pets 刷新并选择“照”。

## 通过 Pets 创建技能安装

这是支持 Pets 插件的 ChatGPT Work 环境中的备用方式，与上面的 Codex 本地原生目录安装属于不同流程。下载仓库的 [zhao-pet-v2.png](zhao-pet-v2.png)，附给 Agent 后使用下面的提示词；若当前环境没有该技能，请使用推荐的原生安装方式。

```text
请使用 Pets 插件的 create-pet 技能（或当前环境提供的 hatch-pet），将附件 zhao-pet-v2.png 作为已完成的 v2 精灵图集安装为桌宠“照”。
使用现成图集，保留九个动画状态和十六向视线。
先验证图集并展示动画预览；如果已存在同名桌宠，先检查是否为同一图集，避免重复创建。
通过校验后，按技能支持的上传、创建、选择流程操作，并核对返回的桌宠 ID 与激活状态。
```
