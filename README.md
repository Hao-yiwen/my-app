# my-app

[![Expo](https://img.shields.io/badge/Expo-52.0.23-000020?logo=expo&logoColor=white)](https://expo.dev)
[![React Native](https://img.shields.io/badge/React%20Native-0.76.5-61DAFB?logo=react&logoColor=white)](https://reactnative.dev)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.7.2-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org)

基于 **Expo SDK 52** 与 **Expo Router** 的 React Native 起步应用。界面来自 `create-expo-app` 的 Tabs 模板，用来演示文件路由、深浅色主题、自定义字体和 Reanimated 动画。仓库已经执行过 prebuild，带有可编译的 `android/` 与 `ios/` 工程；没有额外业务功能。

显示名 `my-app`，版本 `1.0.0`。Android 包名与 iOS Bundle Identifier 均为 `com.haoyiwen.myapp`，URL Scheme 为 `myapp`。

仓库里没有界面截图，只有应用图标、启动图和 React Logo，因此本文不放截图。

## 已有界面

| 路由 | 文件 | 内容 |
| --- | --- | --- |
| `/`（Home） | `app/(tabs)/index.tsx` | 视差头图、挥手动画，以及编辑文件、打开开发者菜单、重置项目的说明 |
| `/explore` | `app/(tabs)/explore.tsx` | 可折叠说明：文件路由、Android / iOS / Web、多倍图、自定义字体、深浅色、动画 |
| 未匹配路径 | `app/+not-found.tsx` | 标题为 “Oops!” 的占位页，可返回首页 |

底部两个标签由 `app/(tabs)/_layout.tsx` 配置。iOS 上标签栏使用模糊背景，按下时有轻微触感反馈；Android 与 Web 使用不透明底栏。

根布局 `app/_layout.tsx` 会加载 `SpaceMono` 字体，字体就绪前保持启动屏，并按系统浅色 / 深色切换 React Navigation 主题。

## 技术栈

版本取自 `yarn.lock` 的解析结果（`package.json` 里写的是兼容范围）。

| 类别 | 依赖 | 锁定版本 |
| --- | --- | --- |
| 运行时 | `expo` | 52.0.23 |
| 路由 | `expo-router` | 4.0.15 |
| UI | `react` / `react-dom` | 18.3.1 |
| 原生 | `react-native` | 0.76.5 |
| 导航 | `@react-navigation/native`、`bottom-tabs` | 7.0.14、7.2.0 |
| 动画 | `react-native-reanimated` | 3.16.6 |
| 手势 / 安全区 / 屏幕 | `react-native-gesture-handler`、`react-native-safe-area-context`、`react-native-screens` | 2.20.2、4.12.0、4.4.0 |
| Web | `react-native-web` | 0.19.13 |
| 图标 | `@expo/vector-icons`（Android / Web 回退为 Material Icons；iOS 用 `expo-symbols`） | 14.0.4 / 0.2.0 |
| 语言 | `typescript` | 5.7.2 |
| 测试 | `jest`、`jest-expo`、`react-test-renderer` | 29.7.0、52.0.2、18.3.1 |

`app.json` 打开了 New Architecture（`newArchEnabled: true`）和类型化路由（`experiments.typedRoutes`）。Android `gradle.properties` 与 iOS `Podfile.properties.json` 同步启用 New Architecture，JS 引擎为 Hermes。

Android：Gradle 8.10.2，`minSdk` 24，`compileSdk` 35，`targetSdk` 34，Kotlin 1.9.24，NDK 26.1.10909125。  
iOS：部署目标 15.1（`ios/Podfile`），支持 iPad。

## 目录结构

```text
.
├── app/                  # Expo Router 页面
│   ├── _layout.tsx       # 根 Stack、字体与主题
│   ├── +not-found.tsx
│   └── (tabs)/           # Home、Explore 与底部标签
├── components/           # 主题文本/容器、视差滚动、折叠面板、外链、触感标签
│   ├── ui/               # 跨平台图标与标签栏背景（.ios.tsx 为 iOS 实现）
│   └── __tests__/        # ThemedText 快照测试
├── constants/Colors.ts   # 浅色 / 深色色板
├── hooks/                # useColorScheme、useThemeColor（Web 有单独的水合实现）
├── assets/               # SpaceMono 字体、图标、启动图、React Logo
├── scripts/reset-project.js
├── android/              # 已生成的 Android 工程，applicationId com.haoyiwen.myapp
├── ios/                  # 已生成的 iOS 工程，target 名 myapp
├── app.json
├── package.json
├── tsconfig.json         # 路径别名 @/* → 仓库根目录
└── yarn.lock
```

路径别名示例：`@/components/ThemedText`、`@/hooks/useColorScheme`。

## 环境要求

- Node.js。仓库未声明 `engines`。本次在 **Node.js 22.23.4** 上完成安装、测试和 Web 启动。
- Yarn 1（仓库使用 `yarn.lock`，本次为 Yarn 1.22.22）。请用 Yarn 安装，避免另生成一份锁文件。
- 跑 Web 只需要上面两项。
- 跑 Android 还需要 Android SDK 与可用模拟器或真机。本次环境没有 `ANDROID_HOME`，**未验证** `yarn android`。
- 跑 iOS 需要 macOS、Xcode 与 CocoaPods。本次环境没有 Xcode，**未验证** `yarn ios`。

## 安装与运行

```bash
yarn install --frozen-lockfile
```

开发服务器（终端里可按 `w` / `a` / `i` 选择 Web、Android、iOS）：

```bash
yarn start
```

只开 Web：

```bash
yarn web
```

浏览器访问 Metro 打印的地址（默认 `http://localhost:8081`）。首页是 Home，Explore 在 `/explore`。

Android / iOS 原生运行（需要对应工具链，本次未验证）：

```bash
yarn android
yarn ios
```

导出 Web 静态文件（输出到 `dist/`，该目录已在 `.gitignore` 中）：

```bash
npx expo export --platform web
```

### 测试

`package.json` 里的 `yarn test` 是 `jest --watchAll`，会停在监听模式。非交互环境请关掉监听：

```bash
npx jest --watchAll=false --ci
```

当前只有一个用例：`components/__tests__/ThemedText-test.tsx` 渲染 `ThemedText` 并对比快照。

### 重置模板

`yarn reset-project` 会把 `app`、`components`、`hooks`、`constants`、`scripts` **移动**到 `app-example/`（该目录被 git 忽略），再生成只含空白页面的 `app/`。这是脚手架自带的清空操作，会改掉现有示例代码。

## 已在本环境验证

| 命令 | 结果 |
| --- | --- |
| `yarn install --frozen-lockfile` | 成功 |
| `npx jest --watchAll=false --ci` | 1 个测试套件、1 个用例、1 个快照通过 |
| `npx expo export --platform web` | 成功，包含 `/`、`/explore`、`/+not-found` 等静态路由 |
| `npx expo start --web --port 8081` | `/` 与 `/explore` 返回 200，页面含 Welcome / Explore；未知路径返回 404 |
| `yarn android`、`yarn ios` | 未验证（无 Android SDK、无 Xcode） |

## 备注

- 仓库没有 `babel.config.js`、`metro.config.js`，也没有 ESLint 配置或 `eslint` 依赖。上面的 Web 打包与开发服务器在缺省配置下可以完成。`yarn lint`（即 `expo lint`）在没有配置时会由 CLI 自动安装 ESLint 并写入 `.eslintrc.js` 与锁文件，因此不要把它当成开箱即用、且不改动仓库的命令。
- `npx tsc --noEmit` 目前不能通过：`app/+not-found.tsx` 使用了 JSX，但文件是模块且没有导入 React（`TS2686`）。项目没有单独的类型检查脚本。
- 启动 `expo start` 时，CLI 会提示若干包相对当前 Expo 版本偏旧（例如期望 `expo` ~52.0.49、`react-native` 0.76.9）。本文记录的是锁文件里的实际版本，没有升级依赖。
- 参考：[Expo 文档](https://docs.expo.dev/)、[Expo Router](https://docs.expo.dev/router/introduction/)、[Expo 教程](https://docs.expo.dev/tutorial/introduction/)。
