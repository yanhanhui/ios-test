# 🍅 番茄时钟 iOS App

> 一款基于番茄工作法的原生iOS应用，使用SwiftUI + 原生技术栈开发

## 📋 项目概述

番茄时钟App是一款专注于提升工作效率的时间管理工具，基于著名的番茄工作法(Pomodoro Technique)设计。应用采用100%原生iOS技术栈，无第三方依赖，确保最佳性能和用户体验。

### 🎯 核心理念
- **简单易用**: 专为编程初学者设计，界面直观，操作简单
- **原生优先**: 完全使用Apple官方技术，无需额外环境配置
- **性能优异**: 轻量级设计，快速启动，流畅运行

## ⚡ 功能特性

### 🕒 核心计时功能
- ✅ 25分钟专注工作倒计时
- ✅ 5分钟短休息倒计时
- ✅ 15分钟长休息倒计时（每4个番茄后自动触发）
- ✅ 暂停/继续/重置功能
- ✅ 自动状态切换，无需手动干预

### 🎨 用户界面
- ✅ 直观的圆形进度指示器
- ✅ 大字体清晰时间显示
- ✅ 状态颜色自动切换
- ✅ 简洁现代的设计风格
- ✅ 支持深色模式

### 🔊 音效提醒
- ✅ 系统原生提示音效
- ✅ 工作开始/结束提醒
- ✅ 休息时间提醒

### 📊 数据统计
- ✅ 今日完成番茄数量统计
- ✅ 本地数据持久化存储

## 🏗️ 技术架构

### 技术栈选择
| 功能模块 | 技术选择 | 选择理由 |
|---------|---------|---------|
| UI框架 | SwiftUI | 原生声明式UI，代码简洁易懂 |
| 计时器 | Foundation Timer | 系统原生，精确可靠 |
| 音效 | AVFoundation | 系统音效，无需额外资源 |
| 数据存储 | UserDefaults | 轻量级，适合简单数据 |
| 动画 | SwiftUI Animation | 流畅原生动画效果 |

### 项目结构
```
test/
├── 📁 Models/                    # 数据模型层
│   ├── PomodoroTimer.swift       # 计时器核心模型
│   └── PomodoroState.swift       # 状态枚举定义
├── 📁 Views/                     # 视图层
│   ├── ContentView.swift         # 主界面容器
│   ├── TimerView.swift           # 计时器显示视图
│   ├── ControlsView.swift        # 控制按钮视图
│   └── ProgressRingView.swift    # 圆形进度条视图
├── 📁 ViewModels/                # 视图模型层
│   └── PomodoroViewModel.swift   # 主要业务逻辑
└── 📁 Utils/                     # 工具类
    ├── SoundManager.swift        # 音效管理器
    └── UserDefaultsManager.swift # 数据存储管理
```

### 核心组件设计

#### 1. 状态管理 (PomodoroState)
```swift
enum PomodoroState {
    case work       // 工作状态 - 25分钟
    case shortBreak // 短休息 - 5分钟  
    case longBreak  // 长休息 - 15分钟
    case idle       // 空闲状态
}
```

#### 2. 计时器模型 (PomodoroTimer)
```swift
/**
 * 番茄时钟核心计时器
 * 负责时间管理、状态切换、进度计算
 */
class PomodoroTimer: ObservableObject {
    @Published var timeRemaining: Int        // 剩余时间（秒）
    @Published var currentState: PomodoroState // 当前状态
    @Published var isRunning: Bool           // 是否运行中
    @Published var completedPomodoros: Int   // 已完成番茄数
    
    // 核心方法
    func start()    // 开始计时
    func pause()    // 暂停计时
    func reset()    // 重置计时器
    func nextState() // 切换到下一状态
}
```

#### 3. 视图模型 (PomodoroViewModel)
```swift
/**
 * 主要业务逻辑控制器
 * 连接UI和数据模型，处理用户交互
 */
class PomodoroViewModel: ObservableObject {
    private var timer: PomodoroTimer
    private var soundManager: SoundManager
    private var storageManager: UserDefaultsManager
    
    // UI绑定属性
    var displayTime: String      // 格式化显示时间
    var progressPercentage: Double // 进度百分比
    var currentStateTitle: String  // 当前状态标题
    var currentStateColor: Color   // 当前状态颜色
}
```

## 🎨 UI设计方案

### 界面布局
```
┌─────────────────────────┐
│     📱 番茄时钟          │ ← 导航栏
├─────────────────────────┤
│                         │
│    🍅 工作中             │ ← 状态标题
│                         │
│    ⭕ ┌─────────┐        │
│       │  25:00  │        │ ← 圆形进度环 + 时间显示
│       └─────────┘        │
│                         │
│   🎮 [暂停] [重置]       │ ← 控制按钮
│                         │
│   📊 今日完成: 3个番茄    │ ← 统计信息
│                         │
└─────────────────────────┘
```

### 色彩系统
| 状态 | 主色调 | 色值 | 设计理念 |
|-----|-------|------|---------|
| 工作中 | 番茄红 | #FF6B6B | 专注活力，提升注意力 |
| 短休息 | 清新绿 | #4ECDC4 | 放松舒缓，缓解疲劳 |
| 长休息 | 宁静蓝 | #45B7D1 | 深度休息，恢复精力 |
| 背景色 | 自适应 | 系统色 | 支持深色/浅色模式 |

### 交互设计
- **开始/暂停**: 大型主操作按钮，点击反馈明显
- **重置功能**: 辅助按钮，避免误操作
- **状态切换**: 全自动化，减少用户认知负担
- **进度显示**: 视觉化进度环，实时动画反馈

## 🚀 开发计划

### 第一阶段：核心功能 (Week 1)
- [ ] 创建基础项目结构
- [ ] 实现计时器核心逻辑
- [ ] 完成状态管理系统
- [ ] 基础UI框架搭建

### 第二阶段：界面优化 (Week 2)  
- [ ] 设计圆形进度条组件
- [ ] 实现状态颜色切换
- [ ] 添加流畅动画效果
- [ ] 适配不同屏幕尺寸

### 第三阶段：用户体验 (Week 3)
- [ ] 集成系统音效提醒
- [ ] 添加触觉反馈
- [ ] 实现数据持久化
- [ ] 支持深色模式

### 第四阶段：完善优化 (Week 4)
- [ ] 代码重构和注释完善
- [ ] 性能优化和测试
- [ ] 用户体验细节调优
- [ ] 应用图标和启动页

## 💡 设计亮点

### 1. 用户友好设计
- **零学习成本**: 界面直观，无需说明书
- **自动化操作**: 状态自动切换，减少用户操作
- **视觉反馈**: 颜色、动画、音效多重反馈

### 2. 技术优势
- **原生性能**: 100%原生代码，启动快速
- **内存友好**: 轻量级设计，不占用系统资源
- **电池优化**: 高效算法，延长设备续航

### 3. 代码质量
- **模块化设计**: 功能独立，易于维护扩展
- **完整注释**: 每个函数都有详细说明
- **最佳实践**: 遵循SwiftUI和iOS开发规范

## 🔧 开发环境要求

### 系统要求
- macOS 12.0+ (Monterey)
- Xcode 14.0+
- iOS 15.0+ (目标设备)

### 技能要求
- Swift基础语法
- SwiftUI基本概念
- iOS开发基础知识

## 📝 编码规范

### 注释标准
```swift
/**
 * 函数功能描述
 * @param 参数名 参数说明
 * @return 返回值说明
 * @note 特殊说明
 */
func exampleFunction() {
    // 代码逻辑说明
}
```

### 文件组织
- 每个功能模块独立文件
- 按照MVC/MVVM模式组织代码
- 保持文件简洁，单一职责

## 🎯 项目目标

### 短期目标
- 完成基础番茄时钟功能
- 实现美观易用的界面
- 确保应用稳定运行

### 长期愿景  
- 成为学习SwiftUI的最佳实践案例
- 为初学者提供完整的iOS开发参考
- 展示原生iOS开发的优雅和高效

---

**开发团队**: AI编程助手 + 用户协作  
**开发周期**: 4周  
**技术支持**: 100%原生iOS技术栈  
**设计理念**: 简洁、高效、用户友好 