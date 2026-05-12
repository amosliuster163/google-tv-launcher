# ChatGPT 提示词 - Google TV Launcher 项目分析

> 直接复制以下全部内容发送给 ChatGPT

---

```
你是一个资深 Android TV 开发工程师，精通 Leanback Library、Jetpack Compose、遥控器交互设计和 Google Assistant 集成。

## 项目背景

我要仿制 Google TV 的 Launcher 界面，做一个 Android 机顶盒的 Launcher 应用。
- **参考视频**：61 秒的 Google TV UI 演示
- **目标设备**：海外 AOSP 机顶盒（完整保留 Google 服务，含 Google Assistant）
- **目标市场**：海外（默认英文，支持多语言）
- **技术方案**：Kotlin + Leanback Library + Jetpack Compose 混合

## 项目资料

项目仓库：https://github.com/amosliuster163/google-tv-launcher

关键文件：
- `README.md` — 项目概览
- `docs/PRD.md` — 完整需求分析（UI 组件清单、技术方案、MVP 路线、海外定制建议）
- `docs/storyboard.md` — 61 秒视频的 21 帧分镜头解析，每帧有详细标注
- `frames/` — 21 帧原始截图
- `videos/reference-video.mp4` — 原始参考视频

## 核心保留功能（与国内版本不同）

| 功能 | 说明 |
|------|------|
| ✅ Google Assistant | 完整集成，通过 GMS 认证 |
| ✅ Google Play Store | 应用分发入口 |
| ✅ Netflix/YouTube/Prime Video | 海外主流内容源 |
| ✅ Chromecast Built-in | 投屏接收 |
| ✅ Google Home 集成 | 智能家居控制入口 |
| ✅ 英文界面 | 默认英文，支持多语言 |

## 唯一砍掉的功能
- ❌ HDMI Input 切换（机顶盒本身是输入源）

## 请你帮我做以下分析

### 1. 项目架构设计
- 推荐的 Android 项目架构（MVVM/MVI？）
- 核心 Activity + Fragment 的划分方案
- 数据流设计（应用列表、推荐内容、设置项从哪里来）
- Module 拆分建议（app / core / ui / data？）

### 2. UI 实现方案（重点）
- **Home 页**：如何用 Leanback 实现顶部标签栏 + 横向滚动推荐行 + Favorite Apps 行
- **Apps 页**：已安装应用网格如何布局
- **设置面板**：右侧滑入式面板用什么组件实现
- **遥控器焦点管理**：Leanback 的焦点系统怎么用，自定义焦点指示器怎么做
- **深色主题**：Google TV 的深色风格如何实现

### 3. Google Assistant 集成方案
- AOSP 上如何集成 Google Assistant（开发阶段 vs 正式产品）
- 开发阶段：OpenGApps mini 集成方案
- 正式产品：GMS 认证流程（MADA 申请、CTS/GTS 测试）
- 语音搜索入口的 UI 实现和 Intent 调用方式
- 如果暂时不过 GMS 认证，有什么替代方案

### 4. 关键技术点
- 如何获取已安装应用列表并展示
- 推荐内容如何动态加载（模拟数据 → 真实 CMS）
- 焦点动画和过渡效果
- Leanback 的 BrowseSupportFragment 和 RowsSupportFragment 用法

### 5. MVP 开发计划
请给出 Phase 1（基础框架，2-3 周）的详细开发任务拆解，精确到：
- 每个文件的创建
- 代码量预估
- 依赖库版本
- 测试策略

### 6. 踩坑预警
基于你的经验，这个项目中最大的技术坑可能是什么？如何提前规避？
重点关注：
- Leanback 的坑
- 遥控器焦点的坑
- GMS 集成的坑
- 性能优化的坑

## 输出要求
1. 给出具体代码示例（Kotlin）
2. 标注每个方案对应的 Android 官方文档链接
3. 如果 Leanback 不适合某个场景，给出替代方案（如 Compose for TV）
4. 给出完整的 build.gradle.kts 依赖配置
5. 如果可能，给出一个最小可运行的 MainActivity 代码
```

---

## 使用建议

1. **第一次发送**：把上面整个提示词发给 ChatGPT，让它做全面分析
2. **追问方向**：
   - "请详细展开第 3 点 Google Assistant 集成方案，给出 OpenGApps 的具体集成步骤"
   - "请给出 Phase 1 的完整代码，从创建项目到 Home 页面能跑起来"
   - "Leanback 的焦点管理有没有最佳实践？给一个完整的示例"

3. **如果需要分模块深入**：
   - 可以每次只问一个方向，比如专门问 Google Assistant 集成
   - 或者让它分步骤输出，避免回答太长被截断