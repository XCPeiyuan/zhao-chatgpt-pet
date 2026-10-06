# 照｜Codex 原生 v2 桌宠

[English](README.en.md)

「照」是可安装到 Codex Desktop 的动画桌宠资源，不是独立软件。原生包只包含 `pet.json` 和 `spritesheet.webp`；动作含义由 v2 图集布局决定，图片中不需要印文字或提示词。

## 快速安装

1. 下载 [zhao-codex-native-v2.zip](https://github.com/XCPeiyuan/zhao-chatgpt-pet/releases/download/v1.0.0/zhao-codex-native-v2.zip) 并解压到临时目录。
2. 将其中的 `pet.json` 和 `spritesheet.webp` 放进 `%USERPROFILE%\.codex\pets\zhao\`。不要把临时解压目录直接当作桌宠目录。
3. 在 Codex 的“设置 → Pets”刷新并选择“照”。若文件名已存在，先停止并检查，不要覆盖。

完整的 [Windows / macOS 安装说明及 Agent 提示词](INSTALL-zh-CN.md) 与 [English guide](INSTALL-en.md)。

## 原生桌宠包

- `zhao-codex-native-v2.zip`：可直接安装的 ZIP，根目录只有 `pet.json` 和 `spritesheet.webp`。
- `zhao-pets-upload-bundle.zip`：旧版 Work Pets 上传资料包，不是 Codex 原生安装包；请使用上方 Release。
- `codex-native/pet.json`：ID 为 `zhao`、名称为“照”、图集版本为 v2。
- `codex-native/spritesheet.webp`：无损 WebP，1536 × 2288 px，透明 RGBA，8 列 × 11 行，每格 192 × 208 px。
- `zhao-pet-v2.png`：同一图集的 PNG 校对源。
- `sprite-sheet-map.png`：逐行动作和帧格索引图，仅供查看，不要放进安装目录。
- `look-directions.png`：中性姿态与 16 向视线校对图，仅供查看。
- `animation-preview.gif`：动画预览。

## 图集行映射

行号从 1 开始。前 9 行是动画状态，最后两行是 16 个视线方向。动画行的帧数已按成品图集核对。

| 行 | 状态 | 帧数 | 含义 |
|---:|---|---:|---|
| 1 | `idle` | 6 | 待机、眨眼 |
| 2 | `running-right` | 8 | 向右蹦跳 |
| 3 | `running-left` | 8 | 向左蹦跳 |
| 4 | `waving` | 4 | 挥手问候 |
| 5 | `jumping` | 5 | 开心跳跃 |
| 6 | `failed` | 8 | 受阻、托腮困惑 |
| 7 | `waiting` | 6 | 举手等待回应，对话框问号 |
| 8 | `running` | 6 | 坐着操作平板，旁有加载图标 |
| 9 | `review` | 6 | 检查结果 |
| 10–11 | 16 向视线 | 8 + 8 | 顺时针每 22.5° 一帧，000°–337.5° |

![图集逐行与帧格映射](sprite-sheet-map.png)

![中性姿态与 16 向视线](look-directions.png)

「照」是粉发兔耳角色；手脚保持毛茸茸的兔子外形，没有肉垫。工作、等待、受阻等符号已经画进相应动画帧，校对图不会混进安装包。

## 校验

- `pet.json` 可解析，`id` 为 `zhao`，v2 图集路径为 `spritesheet.webp`。
- PNG 源图为 RGBA，尺寸为 1536 × 2288；alpha 范围为 0–255，具有真实透明像素。
- WebP 解码后的 RGBA 像素与 PNG 源图逐像素完全一致；8 × 11 网格与单格尺寸可整除。
- ZIP 布局已核对：仅包含 `pet.json` 和 `spritesheet.webp`。

## Windows 提示

如果文件安装正确、刷新 Pets 后仍未出现，先检查 Codex Desktop 是否使用 WSL 后端。不要为此修改 pet.json、转换图集或擅自切换后端；切换可能影响依赖 WSL 的工作流，应先确认。

## 来源与权利说明

这是非官方、AI 辅助制作的同人桌宠素材，创作参考包括维护者提供的角色设计图和游戏画面；本仓库不重新分发这些参考截图。游戏及其角色、名称、商标和相关素材的权利归各自权利人所有。本仓库不授予对第三方素材的许可，也不代表 HoYoverse 官方。仓库未附通用开源许可证。
