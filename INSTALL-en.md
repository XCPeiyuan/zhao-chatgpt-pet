# Install Zhao in Codex Desktop (Windows / macOS)

The native v2 pet needs only `pet.json` and `spritesheet.webp`. The root of `zhao-codex-native-v2.zip` contains exactly those two files.

## Copyable agent-install prompt

```text
Please install the native Codex v2 pet “照” on this computer.

Follow these steps:
1. Identify whether this system is macOS or Windows.
2. Download and extract https://github.com/XCPeiyuan/zhao-chatgpt-pet/releases/download/v1.0.0/zhao-codex-native-v2.zip to a temporary folder.
3. Locate pet.json and spritesheet.webp. The ZIP root contains only these two files; do not use the extraction folder itself as the final pet directory.
4. Install to:
   - macOS: ~/.codex/pets/zhao/
   - Windows: %USERPROFILE%\.codex\pets\zhao\
5. Copy only pet.json and spritesheet.webp. Do not delete or modify any other pet files.
6. Verify pet.json has id zhao, spriteVersionNumber 2, and spritesheetPath spritesheet.webp; confirm spritesheet.webp exists and is readable.
7. If either destination file already exists, list the conflicting file and stop to ask whether I want it replaced. Never overwrite it without approval.
8. Report the actual install path and checks, then remind me to refresh Pets in Codex Settings and select “照”. Do not interrupt a running task; if restarting Codex is necessary, explain first and wait until current work is safe to stop.
9. If the pet remains missing after refresh on Windows, first check whether Codex Desktop is using a WSL backend. Do not edit pet.json, convert the sheet, replace spritesheet.webp, or switch backends on your own. If WSL is confirmed, explain that moving tasks to the Windows-native backend may affect workflows that depend on WSL and ask for my approval. Only after approval and once tasks are safe to stop, switch the task backend, fully quit Codex from the system tray, relaunch it, and refresh Pets. The integrated terminal can continue using WSL.
```

## Windows

Download the ZIP to your Downloads folder, then run this in PowerShell:

```powershell
$zip = Join-Path $env:USERPROFILE 'Downloads\zhao-codex-native-v2.zip'
$stage = Join-Path $env:TEMP ('zhao-codex-' + [guid]::NewGuid().ToString('N'))
Expand-Archive -LiteralPath $zip -DestinationPath $stage

$petDir = Join-Path $env:USERPROFILE '.codex\pets\zhao'
$names = @('pet.json', 'spritesheet.webp')
$existing = @($names | Where-Object { Test-Path -LiteralPath (Join-Path $petDir $_) })
if ($existing.Count -gt 0) { throw "Destination files already exist: $($existing -join ', '). Inspect them first; nothing was overwritten." }

$manifest = Get-Content -LiteralPath (Join-Path $stage 'pet.json') -Raw | ConvertFrom-Json
if ($manifest.id -ne 'zhao' -or $manifest.spriteVersionNumber -ne 2 -or $manifest.spritesheetPath -ne 'spritesheet.webp') { throw 'pet.json does not match this package.' }
if (-not (Test-Path -LiteralPath (Join-Path $stage 'spritesheet.webp'))) { throw 'spritesheet.webp is missing.' }

New-Item -ItemType Directory -Path $petDir -Force | Out-Null
Copy-Item -LiteralPath (Join-Path $stage 'pet.json') -Destination $petDir
Copy-Item -LiteralPath (Join-Path $stage 'spritesheet.webp') -Destination $petDir
Get-Item -LiteralPath (Join-Path $petDir 'pet.json'), (Join-Path $petDir 'spritesheet.webp') | Select-Object Name, Length
```

Other files in `$petDir` are left untouched. If either destination filename already exists, the script stops; inspect it and decide whether to replace it.

Refresh Pets in Codex Settings and select “照”. If it remains missing, first check whether Codex Desktop uses a WSL backend; do not switch without approval because it may affect workflows that depend on WSL. Do not modify the app installation directory.

## macOS

```bash
unzip ~/Downloads/zhao-codex-native-v2.zip -d "${TMPDIR:-/tmp}/zhao-codex-native"
pet_dir="$HOME/.codex/pets/zhao"
for name in pet.json spritesheet.webp; do
  if [ -e "$pet_dir/$name" ]; then echo "Already exists: $pet_dir/$name; stopping without overwrite."; exit 1; fi
done
mkdir -p "$pet_dir"
cp "${TMPDIR:-/tmp}/zhao-codex-native/pet.json" "$pet_dir/"
cp "${TMPDIR:-/tmp}/zhao-codex-native/spritesheet.webp" "$pet_dir/"
```

Reopen Codex, go to Settings → Pets, refresh the list, and select “照”.
