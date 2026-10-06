# 安装「照」到 Codex Desktop（Windows / macOS）

原生 v2 安装只需要 `pet.json` 和 `spritesheet.webp`。本仓库的 `zhao-codex-native-v2.zip` 根目录正好只有这两个文件。

## 可直接交给 Agent 的安装提示词

```text
请帮我在这台电脑上安装 Codex 原生 v2 桌宠“照”。

请按以下步骤操作：
1. 识别当前系统是 macOS 还是 Windows。
2. 从 https://github.com/XCPeiyuan/zhao-chatgpt-pet/releases/download/v1.0.0/zhao-codex-native-v2.zip 下载并解压到临时目录。
3. 找到 pet.json 和 spritesheet.webp。ZIP 根目录只有这两个文件；不要把解压目录当作最终桌宠目录。
4. 安装目录：
   - macOS：~/.codex/pets/zhao/
   - Windows：%USERPROFILE%\.codex\pets\zhao\
5. 只复制 pet.json 和 spritesheet.webp；不要删除或修改其他桌宠文件。
6. 检查 pet.json 的 id 为 zhao、spriteVersionNumber 为 2、spritesheetPath 为 spritesheet.webp，并确认 spritesheet.webp 存在且可读取。
7. 如果目标位置已存在 pet.json 或 spritesheet.webp，列出冲突文件并停止，先询问我是否覆盖；不要擅自覆盖。
8. 告诉我实际安装路径和检查结果，并提醒我在 Codex“设置 → Pets”刷新列表后选择“照”。不要中断正在运行的任务；如果需要重启 Codex，先说明并等当前工作安全结束。
9. 如果 Windows 安装后刷新仍看不到“照”，先检查 Codex Desktop 是否使用 WSL 后端。不要修改 pet.json、转换图集、替换 spritesheet.webp 或擅自切换后端。若确认是 WSL，说明切换为 Windows 原生后端可能影响依赖 WSL 的工作流，先征求我的同意；得到同意且任务已安全停止后，才切换后端、从系统托盘完整退出并重启 Codex，再刷新 Pets。集成终端仍可继续使用 WSL。
```

## Windows

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

## macOS

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
