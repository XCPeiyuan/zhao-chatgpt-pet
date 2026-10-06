# Alternative installation methods for Zhao

The native v2 pet needs only `pet.json` and `spritesheet.webp`. The [Release ZIP](https://github.com/XCPeiyuan/zhao-chatgpt-pet/releases/tag/v1.0.0) contains those two files at its root.

## Manual installation

### Windows

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

### macOS

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

## Install through the Pets creation skill

This alternative uses the Pets plugin in a supported ChatGPT Work environment. It is a separate workflow from the local native Codex installation above. Download [zhao-pet-v2.png](zhao-pet-v2.png), attach it to the agent, and use this prompt. If the skill is unavailable, use the recommended native installation method.

```text
Use the Pets plugin create-pet skill (or hatch-pet if provided by this environment) to install the attached zhao-pet-v2.png as the pet “照”.
Use the finished v2 sheet with its nine animation states and sixteen gaze directions.
Validate the sheet and show its motion first. If a pet with the same name exists, check whether it uses the same sheet to avoid creating a duplicate.
After validation, follow the skill-supported upload, create, and select workflow. Verify the returned pet ID and active state.
```
