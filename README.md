# McuStudy — 单片机嵌入式学习静态站

从零开始学习单片机嵌入式的纯静态网站，面向机电、自动化、电子信息、机电一体化专业大学生与嵌入式零基础新手。重点覆盖 **51 单片机（STC89C52）**、**STM32 HAL 库**、**ESP32 物联网**、**Proteus 仿真** 与 **Keil C 语言编程**。

纯前端实现：**HTML5 + Tailwind CSS v3 + 原生 JavaScript**，无后端、无数据库、无构建步骤，可直接部署到 **Vercel / GitHub Pages**。

## 功能特性

- 9 个独立页面：首页 / 学习路线 / 51 教程目录 / STM32 教程目录 / ESP32 教程目录 / 在线工具 / 资料下载 / 实训毕设 / 教程文章模板
- 亮色 / 暗色主题一键切换，本地记忆用户选择（`js/theme.js` 全局统一管理）
- 全站响应式：电脑 / 平板 / 手机自适应，顶部导航固定
- 轻量级代码语法高亮（零依赖正则实现，支持 C / C++ / Python / Bash）+ 一键复制按钮
- 4 个本地实时计算工具：进制转换、51 定时器延时初值、ASCII/HEX 串口转换、CRC8/CRC16 校验
- 教程文章模板：左侧锚点目录自动生成、滚动高亮，正文五段式结构
- 视觉统一：主色 `#165DFF`、简约工科科技风、阅读优先

## 目录结构

```
mcustudy/
├── index.html             # 首页（Hero + 学习路线预览 + 热门教程 + 工具入口）
├── route.html             # 学习路线（纵向时间线，5 个阶段）
├── mcu51.html             # 51 单片机教程目录
├── stm32.html             # STM32 HAL 库教程目录
├── esp32.html             # ESP32 教程目录
├── tools.html             # 在线工具（进制 / 延时 / 串口 / CRC）
├── resource.html          # 资料下载（静态演示按钮）
├── project.html           # 实训 & 毕设案例
├── article-template.html  # 通用教程文章模板（复制即用）
├── css/
│   └── style.css          # 全站样式（CSS 变量管理亮暗主题）
├── js/
│   ├── tailwind.js        # Tailwind CSS v3 浏览器构建（本地化，无需外网 CDN）
│   ├── theme.js           # 主题切换（全局唯一入口）
│   └── app.js             # 移动菜单 / 语法高亮 / 复制按钮 / 目录生成
└── README.md
```

## 本地预览

直接双击 `index.html` 即可用浏览器打开（本地文件系统即可运行，无需服务器）。

如需开发服务器（可选）：

```bash
# 方式一：VS Code 安装 Live Server 插件后右键 Open with Live Server
# 方式二：Node 环境
npx serve .
```

## 常用自定义

### 1. 修改主题色

编辑 `css/style.css` 顶部 `:root` 中的变量，全站颜色同步生效：

```css
:root {
  --color-primary: #165DFF;   /* 主色 */
  --color-bg: #F5F7FA;        /* 页面背景 */
  --color-surface: #FFFFFF;   /* 卡片背景 */
  ...
}
```

暗色模式对应 `html.dark` 中的同名变量。

### 2. 占位链接约定

- 页面间导航（顶部菜单）使用真实相对路径（如 `href="route.html"`），保证页面互相可跳转；
  如需改为占位，直接替换 `href` 为 `#xxx`。
- 教程卡片、下载按钮、上一篇/下一篇等指向"未来内容"的链接统一使用占位
  `href="#xxx" data-placeholder`，按注释提示替换为真实页面/文件地址即可；
  替换后请同步移除 `data-placeholder` 属性。
- 点击占位链接会弹出 toast 提示（由 `js/app.js` 统一处理），发布前替换为真实地址即可消失。

### 3. 在 HTML 中书写代码的转义规则

代码块写在 `<pre><code class="language-xxx">` 内。由于 `<` `>` 会被浏览器当作标签解析，
书写代码时必须转义：

| 字符 | 转义写法 |
| --- | --- |
| `<` | `&lt;` |
| `>` | `&gt;` |
| `&` | `&amp;` |

不转义会导致代码被浏览器当成 HTML 吞掉。复制按钮复制的是转义后的原文（浏览器自动还原），不受影响。

### 4. 语法高亮支持的语言

在 `<code>` 上加 class：`language-c`、`language-cpp`、`language-python`、`language-bash`，
默认按 C 处理。关键词表在 `js/app.js` 的 `KEYWORDS` 对象中，可按需增补。

## Vercel 部署步骤

### 方式一：Git 导入（推荐）

1. 将 `mcustudy/` 目录内容推送到 GitHub / GitLab / Gitee 仓库（建议把目录内文件放到仓库根目录）；
2. 打开 [vercel.com](https://vercel.com)，用 GitHub 账号登录，点击 **Add New → Project**；
3. Import 你的仓库；
4. 框架预设选择 **Other**（本项目无构建步骤）：
   - Build Command：留空
   - Output Directory：留空（默认根目录）
5. 点击 **Deploy**，等待完成即可获得 `https://xxx.vercel.app` 访问地址。

### 方式二：Vercel CLI

```bash
npm i -g vercel
cd mcustudy
vercel          # 首次按提示登录并关联项目
vercel --prod   # 部署到生产环境
```

> 提示：本项目所有资源（含 Tailwind）均已本地化，部署不依赖任何外部 CDN。

## GitHub Pages 部署（可选）

1. 将 `mcustudy/` 内容推送到 GitHub 仓库；
2. 仓库 **Settings → Pages**，Source 选择 `Deploy from a branch`，分支选 `main`，目录选 `/ (root)`；
3. 保存后等待 1~2 分钟，访问 `https://<用户名>.github.io/<仓库名>/`。

## 新增教程页面方法

1. 复制 `article-template.html` 为新的教程文件，例如 `mcu51-led.html`；
2. 修改 `<title>`、`<h1>【替换为教程主题】</h1>`、摘要、难度标签与更新日期；
3. 按五段式结构填写：原理介绍 → 硬件接线 → 完整代码 → 现象演示 → 常见报错排查；
4. 回到对应目录页（如 `mcu51.html`），把对应卡片的 `href="#xxx"` 替换为新页面地址；
5. 可选：把新页面加入首页「热门教程」卡片或导航栏。

## 浏览器兼容

使用现代浏览器（Chrome / Edge / Firefox / Safari 最近两个大版本）。核心 JS 特性：`classList`、`TextEncoder/TextDecoder`、`padStart`、`:has()` 选择器（用于工具页选中态，旧浏览器自动降级为普通样式，不影响功能）。

## License

MIT License — 可自由学习、修改与二次发布。
