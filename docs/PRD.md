# Google TV Launcher 仿制需求分析

> **视频来源**：参考视频（61 秒）
> **分析日期**：2026-05-12
> **目标设备**：Android 机顶盒（非 TV 版，砍掉 HDMI Input）

---

## 一、视频内容逐帧拆解

### 1.1 启动画面（Frame 003）
- 设备：小米电视（底部 MI Logo）
- 初始状态：黑屏 → 启动中

### 1.2 Home 主界面（Frame 001, 005）

#### 顶部导航栏
| 元素 | 位置 | 说明 |
|------|------|------|
| Google 语音助手图标 | 左上 | "Click to speak" 语音搜索 |
| 搜索入口 | 左二 | "Search" 文字 |
| 标签页 | 居中 | "Home"（当前选中，底部有白线）/"Apps" |
| 设置图标 | 右上 | 齿轮图标 |
| 时间 | 右上 | 23:10 |

#### Favorite Apps 行
- 4 个卡片：Grid 图标 / Netflix / YouTube / +（添加）
- 卡片尺寸：方形，约 1:1 比例
- 可左右滚动

#### 推荐内容行
- "YouTube Recommended for you"
- 横向滚动的视频缩略图卡片
- 卡片比例：约 16:9，带视频标题
- 可见内容：音乐视频、综艺剪辑等

#### 自定义横幅
- "Customize your Home screen"
- "Choose the channels you'd like to see content from on your Home screen."
- "Choose channels" 按钮

### 1.3 Apps 界面（Frame 007）

#### Installed Apps 网格
- 2 行 × 6 列 = 12 个 App 图标
- 每行可滚动
- 可见 App：Netflix, YouTube, File Browser, Movie Player, Google Play Store, Prime Video, emoln, AirScreen, Magle TV, Aptoide TV
- 卡片带圆角矩形边框

#### 发现横幅
- "Find more apps and games"
- "Go to Play Store" 按钮

### 1.4 设置面板（Frame 010, 015）

#### 右侧滑入式设置面板
- 半透明遮罩背景
- 从右侧滑入，占据约 1/3 屏幕宽度

**Network & Internet 页面**：
- WiFi 网络名称 + 锁图标
- See all / Add new network
- Scanning always available（开关）
- Ethernet / WiFi MAC 地址
- Not connected
- Proxy settings / IP settings

**About 页面**：
- System update
- Device name: AndroidTV
- Restart
- Status
- Legal information
- Model: AndroidTV
- Version: 14.0
- Android security patch level: August 5, 2024
- Kernel version: 4.9.113
- Build: AndroidTV_14.0_20250419

### 1.5 交互流程总结
```
开机 → Home 界面
    → 点击 Apps 标签 → Apps 界面
    → 点击设置图标 → 右侧设置面板
        → 切换 Network / About 等子页面
    → 语音搜索 → 搜索界面
```

---

## 二、UI 组件清单

### 2.1 公共组件

| 组件 | 描述 | Android 对应 |
|------|------|-------------|
| TopBar | 顶部导航栏，含标签页切换 | Leanback / Custom View |
| AppCard | 方形 App 图标卡片，带圆角 | Leanback ImageCardView |
| ContentCard | 16:9 内容卡片，带标题 | Leanback ImageCardView |
| Banner | 全宽横幅，含标题+按钮 | Leanback HeaderItem + actions |
| SidePanel | 右侧滑入式设置面板 | Drawer / Fragment |
| FocusIndicator | 遥控器焦点指示器 | Leanback focus system |

### 2.2 Home 页面组件

| 组件 | 数量 | 说明 |
|------|------|------|
| VoiceSearchButton | 1 | 左上角语音入口 |
| TabLayout | 1 | Home/Apps 切换 |
| FavoriteAppsRow | 1 | 4 个固定位 + 滚动 |
| RecommendedRow | N | 多行推荐内容，每行横向滚动 |
| CustomizeBanner | 1 | 底部横幅 |

### 2.3 Apps 页面组件

| 组件 | 数量 | 说明 |
|------|------|------|
| InstalledAppsGrid | 1 | 2 行网格，每行可滚动 |
| DiscoverBanner | 1 | 跳转 Play Store |

---

## 三、技术实现建议

### 3.1 技术方案对比

| 方案 | 优点 | 缺点 | 推荐度 |
|------|------|------|--------|
| **Leanback Library** | Google 官方 TV 库，组件齐全 | 样式固定，自定义受限 | ⭐⭐⭐⭐ |
| **Jetpack Compose** | 灵活，现代化 | TV 适配需自己做 | ⭐⭐ |
| **Leanback + Compose 混合** | 核心用 Leanback，自定义用 Compose | 学习成本高 | ⭐⭐⭐⭐⭐ |

**推荐**：Leanback + Compose 混合方案

### 3.2 项目结构建议

```
GoogleTVLauncher/
├── app/
│   ├── src/main/java/com/example/launcher/
│   │   ├── ui/
│   │   │   ├── home/          # Home 页面
│   │   │   │   ├── HomeFragment.kt
│   │   │   │   ├── FavoriteAppsAdapter.kt
│   │   │   │   ├── RecommendedRowAdapter.kt
│   │   │   │   └── CustomizeBanner.kt
│   │   │   ├── apps/          # Apps 页面
│   │   │   │   ├── AppsFragment.kt
│   │   │   │   └── InstalledAppsAdapter.kt
│   │   │   ├── settings/      # 设置面板
│   │   │   │   ├── SettingsPanel.kt
│   │   │   │   ├── NetworkSettings.kt
│   │   │   │   └── AboutSettings.kt
│   │   │   └── search/        # 搜索
│   │   │       └── VoiceSearch.kt
│   │   ├── model/             # 数据模型
│   │   │   ├── AppInfo.kt
│   │   │   ├── ContentInfo.kt
│   │   │   └── RowInfo.kt
│   │   ├── repository/        # 数据源
│   │   │   ├── AppRepository.kt
│   │   │   └── ContentRepository.kt
│   │   └── MainActivity.kt
│   └── src/main/res/
│       ├── layout/            # Leanback 布局
│       ├── values/            # 颜色、尺寸
│       └── drawable/          # 图标、背景
├── build.gradle.kts
└── settings.gradle.kts
```

---

## 四、机顶盒定制建议（砍掉/修改）

### 4.1 必须砍掉的
| 功能 | 原因 |
|------|------|
| HDMI Input 切换 | 机顶盒本身是输入源 |
| Google Assistant 语音 | 国内无法使用 Google 服务 |
| Google Play Store | 国内机顶盒无 GMS |
| 海外流媒体（Netflix/YouTube） | 替换为国内内容源 |

### 4.2 建议替换的
| 原功能 | 替换方案 |
|--------|----------|
| YouTube 推荐 | 爱奇艺/腾讯视频/优酷推荐 |
| Prime Video | 芒果TV/哔哩哔哩 |
| 英文界面 | 中文界面（默认） |
| Google 语音搜索 | 科大讯飞/百度语音 |
| 设置面板 | 精简版系统设置 |

### 4.3 机顶盒特色功能（建议新增）
| 功能 | 说明 |
|------|------|
| IPTV 频道列表 | 直播频道入口 |
| 回看/时移 | 节目回看功能 |
|  Parental Control | 儿童模式 |
| 投屏 | DLNA/Miracast 投屏 |
| USB 播放 | U 盘媒体播放 |

---

## 五、开发优先级（MVP 路线）

### Phase 1：基础框架（2-3 周）
- [ ] 项目搭建，Leanback 基础配置
- [ ] Home 页面骨架（顶部栏 + 占位行）
- [ ] Apps 页面骨架（App 列表）
- [ ] 遥控器焦点导航

### Phase 2：内容填充（2 周）
- [ ] Favorite Apps 行（可配置）
- [ ] 推荐内容行（模拟数据）
- [ ] 自定义横幅
- [ ] 应用启动/切换

### Phase 3：设置面板（1 周）
- [ ] 右侧滑入面板
- [ ] 网络设置页面
- [ ] 关于页面
- [ ] 重启/关机功能

### Phase 4：机顶盒定制（2-3 周）
- [ ] 中文界面
- [ ] 国内内容源接入
- [ ] 语音搜索（百度/讯飞）
- [ ] IPTV 频道入口

### Phase 5：优化与发布（1-2 周）
- [ ] 性能优化
- [ ] 适配不同分辨率
- [ ] 测试与修复
- [ ] 打包发布

---

## 六、给 Claude Code 的提示词模板

```markdown
你是一个专业的 Android TV Launcher 开发者。请帮我创建一个仿 Google TV 风格的 Launcher 应用。

## 项目要求
- 使用 Android Leanback Library
- Kotlin + Compose 混合架构
- 目标 Android 版本：API 24+（机顶盒）
- 支持遥控器 D-Pad 导航

## 核心页面
1. Home 页：顶部标签栏 + Favorite Apps 行 + 推荐内容行
2. Apps 页：已安装应用网格列表
3. 设置面板：右侧滑入式，含网络和关于页面

## 参考设计
- 顶部导航：Search | Home | Apps
- 卡片风格：圆角矩形，带焦点高亮
- 颜色：深色主题，#121212 背景
- 字体：Roboto / Noto Sans SC

请先搭建项目骨架，然后逐模块实现。
```

---

## 七、关键技术点

### 7.1 Leanback 核心类
| 类 | 用途 |
|----|------|
| `BrowseSupportFragment` | 主页面容器 |
| `RowsSupportFragment` | 行列表容器 |
| `ListRow` | 单行数据模型 |
| `ImageCardView` | 图片卡片组件 |
| `CardPresenter` | 卡片渲染器 |
| `ArrayObjectAdapter` | 数据适配器 |

### 7.2 焦点管理
- Leanback 自动处理遥控器焦点
- 自定义 View 需实现 `onFocusChanged()`
- 焦点指示器默认蓝色，可自定义颜色

### 7.3 数据源
- 应用列表：`PackageManager.getInstalledApplications()`
- 推荐内容：可配置 JSON 或 CMS 接口
- 设置项：`Settings.System` / `Settings.Global`

---

*分析完成，等待领导确认需求后进入开发阶段*
