# Google TV Launcher 仿制项目

> 仿 Google TV 风格的 Android 机顶盒 Launcher 应用
> 分析日期：2026-05-12

---

## 📂 项目结构

```
google-tv-launcher/
├── docs/
│   ├── PRD.md          # 需求分析文档
│   └── storyboard.md   # UI 分镜头解析（含 21 帧图片）
├── frames/             # 21 帧提取图片
│   ├── frame_001.jpg
│   ├── frame_002.jpg
│   └── ...
└── README.md
```

---

## 📋 文档说明

| 文档 | 内容 |
|------|------|
| [PRD.md](docs/PRD.md) | 完整需求分析：UI 组件清单、技术方案、MVP 路线、机顶盒定制建议 |
| [storyboard.md](docs/storyboard.md) | 61 秒视频逐帧拆解，21 帧图片 + 详细标注 |

---

## 🎯 核心功能

- Home 页面：顶部标签栏 + 推荐内容行 + Favorite Apps
- Apps 页面：已安装应用网格
- 设置面板：右侧滑入式，含网络/关于等页面
- 遥控器 D-Pad 导航支持

---

## 🛠️ 技术栈

- **架构**：Kotlin + Jetpack Compose
- **TV 库**：Android Leanback Library
- **目标版本**：API 24+（Android 机顶盒）
- **语音方案**：百度语音（替代 Google Assistant）

---

## 📅 开发计划

| Phase | 内容 | 周期 |
|-------|------|------|
| Phase 1 | 基础框架 | 2-3 周 |
| Phase 2 | 内容填充 | 2 周 |
| Phase 3 | 设置面板 | 1 周 |
| Phase 4 | 机顶盒定制 | 2-3 周 |
| Phase 5 | 优化发布 | 1-2 周 |

---

*详细需求见 [PRD.md](docs/PRD.md)*
