# CLAUDE.md - AndroidProject-Kotlin

## 项目概述

**项目名称**: AndroidProject-Kotlin (安卓技术中台 Kotlin 版)
**项目地址**: https://github.com/getActivity/AndroidProject-Kotlin
**一句话描述**: 一个成熟的 Android 快速开发脚手架，封装了常用 UI 模板、基类架构、网络请求、AOP 切面等通用能力，用于新项目开发或旧项目重构。

## 技术栈

- **语言**: Kotlin (1.5.31)
- **最低 SDK**: 21 (Android 5.0)
- **目标 SDK / 编译 SDK**: 30 (Android 11)
- **构建工具**: Gradle (Groovy DSL) + AGP 4.1.2
- **架构模式**: 基类继承体系 (BaseActivity/BaseFragment/BindingActivity/BindingFragment) + AOP 切面编程
- **包名**: com.hjq.demo

### 关键依赖库

| 功能 | 库 |
|---|---|
| 网络请求 | EasyHttp (基于 OkHttp 3.12.13) |
| JSON 解析 | Gson + GsonFactory (容错处理) |
| 图片加载 | Glide 4.16.0 |
| 权限请求 | XXPermissions |
| 标题栏 | TitleBar |
| Toast | ToastUtils |
| Shape | ShapeView |
| AOP | AspectJ (aspectjrt 1.9.22.1) |
| 沉浸式 | ImmersionBar 3.0.0 |
| 下拉刷新 | SmartRefreshLayout 2.0.3 |
| KV 存储 | MMKV 2.0.0 |
| 日志 | Timber 5.0.1 |
| 崩溃上报 | Bugly (crashreport + nativecrashreport) |
| 内存泄漏 | LeakCanary (debug/preview) |
| 动画 | Lottie 6.6.3 |
| 协程 | kotlinx-coroutines 1.6.0 + lifecycle-runtime-ktx |
| 友盟 | UmengLogin / UmengShare (QQ + 微信) |

## 项目结构

```
AndroidProject-Kotlin/
├── app/                          # 主应用模块
│   └── src/main/java/com/hjq/demo/
│       ├── app/                  # Application 入口及全局基类
│       │   ├── AppApplication.kt # Application 类
│       │   ├── AppActivity.kt    # 业务 Activity 基类
│       │   ├── AppFragment.kt    # 业务 Fragment 基类
│       │   └── AppAdapter.kt     # 业务 Adapter 基类
│       ├── ui/                   # 界面层
│       │   ├── activity/         # 所有 Activity（23 个）
│       │   ├── fragment/         # 所有 Fragment（7 个）
│       │   ├── adapter/          # 列表适配器
│       │   ├── dialog/           # 自定义对话框
│       │   └── popup/            # 弹窗
│       ├── http/                 # 网络层
│       │   ├── api/              # API 接口定义（10 个）
│       │   ├── glide/            # Glide 自定义配置
│       │   └── model/            # 数据模型
│       ├── aop/                  # AOP 切面注解
│       │   ├── CheckNet.kt       # 网络检测注解
│       │   ├── Log.kt            # 日志注解
│       │   ├── Permissions.kt    # 权限请求注解
│       │   └── SingleClick.kt    # 防重复点击注解
│       ├── action/               # 行为接口（StatusAction/TitleBarAction/ToastAction）
│       ├── manager/              # 管理类（Activity 管理/缓存/对话框/输入管理等）
│       ├── other/                # 辅助工具类（崩溃处理/键盘监听/Toast 样式等）
│       ├── widget/               # App 内自定义控件（浏览器/播放器/状态布局等）
│       └── wxapi/                # 微信回调
├── library/
│   ├── base/                     # 基础库模块
│   │   └── src/main/java/com/hjq/base/
│   │       ├── BaseActivity.kt   # Activity 基类（含 DataBinding 支持）
│   │       ├── BaseFragment.kt   # Fragment 基类
│   │       ├── BaseDialog.kt     # Dialog 基类
│   │       ├── BaseAdapter.kt    # RecyclerView Adapter 基类
│   │       ├── BindingActivity.kt # ViewBinding Activity 基类
│   │       ├── BindingFragment.kt # ViewBinding Fragment 基类
│   │       └── action/           # 基础行为接口（ActivityAction/AnimAction/BundleAction/ClickAction/HandlerAction/KeyboardAction/ResourcesAction）
│   ├── widget/                   # 通用控件库模块
│   │   └── src/main/java/com/hjq/widget/
│   │       ├── view/             # 自定义 View（14 个：EditText/Button/SwitchBar/RatingBar 等）
│   │       └── layout/           # 自定义 Layout
│   └── umeng/                    # 友盟封装模块
│       └── src/main/java/com/hjq/umeng/
│           ├── UmengClient.kt    # 友盟初始化
│           ├── UmengLogin.kt     # 第三方登录（QQ/微信）
│           └── UmengShare.kt     # 社交分享
├── configs.gradle                # 构建配置（服务器地址/BuglyId/友盟Key 等）
├── common.gradle                 # 通用 Android 配置（compileSdk/minSdk/buildTypes/dependencies）
├── maven.gradle                  # Maven 仓库配置
└── settings.gradle               # 模块声明（app + library:base + library:widget + library:umeng）
```

## 构建与运行

### 环境要求
- Android Studio 4.x+
- JDK 8
- Gradle 与 AGP 4.1.2 兼容的版本

### 构建变体 (Build Variants)
项目定义三种构建类型:
- **debug**: 调试版，包名后缀 `.debug`，开启日志，指向测试服务器
- **preview**: 预览版，基于 debug 配置但无包名后缀，指向预发布服务器
- **release**: 正式版，开启混淆和资源压缩，指向正式服务器

### 常用命令
```bash
# 构建调试版
./gradlew assembleDebug

# 构建预览版
./gradlew assemblePreview

# 构建正式版
./gradlew assembleRelease

# 指定服务器类型构建
./gradlew assembleRelease -P ServerType="test"
```

### 签名配置
签名信息在 `local.properties` 中配置（StoreFile/StorePassword/KeyAlias/KeyPassword），需自行准备。

### 服务器配置
在 `configs.gradle` 中可切换服务器地址:
- 测试服: `HOST_URL` 指向 `https://www.test.baidu.com/`
- 预发布服: 指向 `https://www.pre.baidu.com/`
- 正式服: 指向 `https://www.baidu.com/`

## 关键类/模块说明

### 基类继承体系
项目的核心架构设计，所有业务页面均继承自基类:
- `BaseActivity` / `BaseFragment` -- 最底层基类，封装生命周期管理
- `BindingActivity` / `BindingFragment` -- 支持 ViewBinding 的中间基类
- `AppActivity` / `AppFragment` -- 业务层基类，集成状态栏/标题栏/Toast 等通用行为

### AOP 切面 (`com.hjq.demo.aop`)
通过 AspectJ 注解实现声明式编程，无需在业务代码中手动调用:
- `@CheckNet` -- 自动检测网络状态，无网络时提示
- `@Permissions` -- 声明式权限请求
- `@SingleClick` -- 防止按钮重复点击
- `@Log` -- 自动打印方法调用日志

### 行为接口 (`com.hjq.demo.action` + `com.hjq.base.action`)
通过接口组合实现功能解耦:
- `StatusAction` -- 状态栏管理
- `TitleBarAction` -- 标题栏操作
- `ToastAction` -- Toast 显示
- `ActivityAction` / `ClickAction` / `HandlerAction` / `KeyboardAction` 等基础行为

### 网络层 (`com.hjq.demo.http`)
- `api/` 目录下定义各业务 API 接口（登录/注册/验证码/用户信息等）
- 基于 EasyHttp 框架，底层使用 OkHttp 3.12.13
- 数据模型定义在 `model/` 目录

### 管理类 (`com.hjq.demo.manager`)
- `ActivityManager` -- Activity 栈管理
- `DialogManager` -- 对话框统一管理
- `InputTextManager` -- 输入框状态管理（如登录按钮启用/禁用联动）
- `CacheDataManager` -- 缓存数据管理

## 注意事项

1. **混淆规则**: 每个 library 模块自带 `proguard-xxx.pro` 混淆规则，主模块有 `proguard-app.pro` 和 `proguard-sdk.pro`
2. **资源优化**: 仅保留中文语种 (`zh`) 和 `xxhdpi` 密度资源
3. **AOP 配置**: `aspectjx` 仅对应用包名做 AOP 处理，避免与第三方库冲突
4. **多渠道打包**: 项目注释中提到使用 Walle 进行多渠道打包，但当前未在 dependencies 中引入
5. **Java 版本**: 同项目有 Java 版本 [AndroidProject](https://github.com/getActivity/AndroidProject)，结构类似
