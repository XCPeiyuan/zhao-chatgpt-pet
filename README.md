# 照｜ChatGPT Pets 动画桌宠

[English](README.en.md)

这是供 Codex / ChatGPT Work 内置 Pets 功能使用的自定义桌宠素材。它不是独立桌宠软件，也不是可双击运行的程序。

## 快速使用

1. 下载并解压 `zhao-pets-upload-bundle.zip`，或者直接下载 `zhao-pet-v2.png`。
2. 在 Codex / ChatGPT Work 的 Pets 自定义桌宠流程中上传 `zhao-pet-v2.png`。
3. 按界面提示命名并保存、选择桌宠。

Pets 接收精灵图 PNG/WebP；ZIP 只是方便下载的资料包，不能直接上传到 Pets。

## 文件

- `zhao-pets-upload-bundle.zip`：上传用精灵图及中英文操作说明。
- `zhao-pet-v2.png`：Pets v2 精灵图集，1536 × 2288 px，透明 RGBA，8 列 × 11 行，每格 192 × 208 px。
- `animation-preview.gif`：各动画状态的预览。
- `look-directions.png`：中性姿态和 16 个视线方向的标注校对图。

## 16 向视线的位置

16 个方向已经包含在 `zhao-pet-v2.png` 中，不是额外的 16 张安装图片：图集第 10、11 行（从 1 开始计数）各有 8 帧。第 10 行依次为 000° 至 157.5°，第 11 行依次为 180° 至 337.5°，每帧间隔 22.5°。方向含义和逐帧预览见 `look-directions.png`。`look-directions.png` 也展示了中性姿态与全部方向：

![中性姿态与 16 向视线](look-directions.png)

## 校验

图集通过 Pets v2 结构预检和质量校验。它使用透明 RGBA，尺寸、网格及必需帧符合 v2 图集格式。SHA-256：

```text
bb7c4129ea4f415035494be44299ed0477bf65ff6e270418c726f460fda848eb
```

## 来源与权利说明

这是非官方、AI 辅助制作的同人桌宠素材，创作参考包括维护者提供的角色设计图和游戏画面；本仓库不重新分发这些参考截图。游戏及其角色、名称、商标和相关素材的权利归各自权利人所有。本仓库不授予对这些第三方内容的任何许可，也不代表 HoYoverse 官方。仓库未附 MIT、CC 等通用开源许可证。请查看 [HoYoverse 同人作品指南](https://support.hoyoverse.com/hc/en-us/articles/51005649400729-What-are-the-guidelines-for-creating-and-selling-fan-made-content)。