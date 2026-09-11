# Saki Panel 插件

本仓库是 Saki Panel 的官方示例与登记处。面板从这里安装示范插件，开发者也从这里对照清单、目录和加载方式。

当前收录：

| 目录 | 类型 | 作用 |
| --- | --- | --- |
| [plugins/saki-skin-maid](./plugins/saki-skin-maid) | skin | 全站 Saki 立绘换成法式女仆装 |
| [plugins/saki-theme-geo](./plugins/saki-theme-geo) | theme | 构成主义几何外壳 |
| [plugins/saki-game-fruit-slice](./plugins/saki-game-fruit-slice) | game | 切水果 |
| [plugins/saki-locale-ja](./plugins/saki-locale-ja) | locale | 日语界面 |

主题、形象同时只能启用一套。游戏、语言包、组件可多开。

---

## 目录

1. [仓库怎么放](#仓库怎么放)
2. [在面板里安装](#在面板里安装)
3. [清单 `saki-plugin.json`](#清单-saki-pluginjson)
4. [本地调试](#本地调试)
5. [主题 theme](#主题-theme)
6. [形象 skin](#形象-skin)
7. [游戏 game](#游戏-game)
8. [组件 widget](#组件-widget)
9. [语言 locale](#语言-locale)
10. [封面](#封面)
11. [登记与贡献](#登记与贡献)
12. [常见问题](#常见问题)

---

## 仓库怎么放

安装器会在仓库里递归查找 `saki-plugin.json`（最多五层），每个清单所在的目录就是一个独立插件。不要把清单塞进另一个插件的资源文件夹。

```
saki-plugins/
  README.md
  registry.json
  plugins/
    saki-skin-maid/
      saki-plugin.json
      preview.webp
      ...
    saki-theme-geo/
      saki-plugin.json
      theme.css
      preview.webp
    saki-game-fruit-slice/
      saki-plugin.json
      index.html
      preview.webp
      assets/
    saki-locale-ja/
      saki-plugin.json
      ja.json
```

目录名建议与清单里的 `name` 一致。`name` 只能是字母、数字、下划线和连字符，且必须以字母或数字开头，最长 81 个字符。

---

## 在面板里安装

打开 **扩展工坊**。

- 社区精选：读取本仓库根目录的 `registry.json`，一键安装。
- 手动：填写 `所有者/仓库`，例如 `EthanChan050430/saki-plugins`。整仓会装进所有找到的插件。
- 只装其中一个：`所有者/仓库:插件名`，例如 `EthanChan050430/saki-plugins:saki-theme-geo`。

网络不稳时，先在工坊里测速 GitHub 镜像再安装。默认分支按仓库实际情况使用 `master` 或 `main`。

装好之后：

| 类型 | 接下来做什么 |
| --- | --- |
| theme | 点「应用主题」。同时只生效一套。登录页也会读当前主题。 |
| skin | 点「更换形象」。同时只生效一套。未覆盖的路径仍用面板原图。 |
| game | 点「开始游玩」，或在 Saki 手机里打开。 |
| locale | 先启用扩展，再到 **系统设置 → 面板语言** 选对应语言。 |
| widget | 启用即可。面板用 iframe 挂载。 |

---

## 清单 `saki-plugin.json`

每个插件根目录必须有这份文件，缺 `name`、`type`、`displayName` 会被跳过。

```json
{
  "name": "saki-theme-geo",
  "version": "1.0.0",
  "displayName": "简约几何",
  "description": "一句话说明用途。",
  "author": "你的名字",
  "type": "theme",
  "icon": "preview.webp",
  "homepage": "https://github.com/you/your-plugin",
  "minPanelVersion": "3.5.0"
}
```

| 字段 | 必填 | 说明 |
| --- | --- | --- |
| `name` | 是 | 唯一标识，安装后作为插件 id 和目录名 |
| `version` | 建议 | 语义化版本。资源 URL 会带 `?v=`，改版本可刷新缓存 |
| `displayName` | 是 | 工坊卡片上的名称 |
| `description` | 建议 | 一两句话 |
| `author` | 建议 | 作者名 |
| `type` | 是 | `theme` `skin` `game` `widget` `locale` |
| `icon` | 建议 | 相对插件根目录的封面，见 [封面](#封面) |
| `minPanelVersion` | 建议 | 最低面板版本，示范插件写 `3.5.0` |
| `homepage` | 否 | 文档或主页 |
| `preview` | 否 | 额外预览图路径数组 |
| `permissions` | 否 | 预留，目前不作强制校验 |

不同类型再补一块配置：`theme`、`skin`、`game`、`widget`、`locale`。下面分节写。

JSON 用 UTF-8。不要注释。

---

## 本地调试

面板扫描的是运行目录下的 `data/plugins/<插件id>/`，不是本仓库的工作树。改完要拷过去才能在工坊里看到。

1. 在 `data/plugins/` 建与 `name` 同名的文件夹。
2. 放入 `saki-plugin.json` 和全部资源。
3. 刷新扩展工坊。面板会读盘同步清单。
4. 按类型启用或设为当前主题 / 形象。
5. 改 CSS、HTML、JSON 后刷新浏览器。静态资源若被缓存，把 `version` 加一。

不要改正在运行的 `data/plugins/plugins.json` 里别人的安装记录，除非你清楚自己在关主题或关形象。

---

## 主题 theme

对照 [plugins/saki-theme-geo](./plugins/saki-theme-geo)。

主题不是只换强调色。CSS 挂在 `<html>` 上，可以重画侧栏、顶栏、登录页、卡片、表格、对话框。登录页在未登录时也会请求当前主题。

```
saki-theme-example/
  saki-plugin.json
  preview.webp
  theme.css
  bg-light.webp
  bg-dark.webp
```

```json
{
  "type": "theme",
  "theme": {
    "css": "theme.css",
    "htmlClass": "saki-plugin-theme-example",
    "backgrounds": {
      "light": "bg-light.webp",
      "dark": "bg-dark.webp"
    }
  }
}
```

`htmlClass` 必须以 `saki-plugin-theme-` 开头，其余只能是字母、数字、下划线、连字符，总长不超过约 40 个字符。不合规则会被丢掉。

`theme.css` 在面板样式之后插入。选择器请带上自己的 class，以免和液态玻璃抢优先级：

```css
html.saki-plugin-theme-example {
  --primary: #1f4bff;
  --radius-control: 0px;
  --radius-modal: 0px;
}

html.saki-plugin-theme-example .sidebar { }

html.saki-plugin-theme-example[data-theme="dark"] { }
```

常用令牌（面板 `theme-liquid.css` / `variables.css` 里还有更多）：

| 令牌 | 用途 |
| --- | --- |
| `--primary` `--primary-hover` `--primary-light` | 主色 |
| `--radius-control` `--radius-modal` `--radius-card` | 圆角 |
| `--glass-bg` `--glass-border` `--glass-shadow` | 玻璃面 |
| `--surface-bg` `--surface-border` `--text-dark` `--text-secondary` | 表面与字色 |
| `--app-background-image` `--mobile-background-image` | 整页背景 |
| `--sidebar-accent` `--heading-gradient-from` `--heading-gradient-to` | 侧栏与标题 |

浅色、深色都要过一遍。`[data-theme="dark"]` 写在自己的 htmlClass 后面。

不要裁切 Saki 立绘：

```css
html.saki-plugin-theme-example .saki-character-art,
html.saki-plugin-theme-example img,
html.saki-plugin-theme-example video {
  clip-path: none;
}
```

同一时刻只有一套主题生效。关掉或换主题时，面板会卸掉 class 和样式表。

---

## 形象 skin

对照 [plugins/saki-skin-maid](./plugins/saki-skin-maid)。

形象按面板 `/assets/` 的相对路径覆盖文件。插件里没有的路径继续用原版，不要拿另一张姿势去顶。

```
saki-skin-example/
  saki-plugin.json
  preview.webp
  saki_click.webp
  sakiicon.webp
  expression/
    happy.webp
    anim/
      happy_f1.webp
      ...
      happy_f6.webp
  pet/
    idle.webp
    walk_f1.webp
```

```json
{
  "type": "skin",
  "skin": {
    "assetRoot": "",
    "files": [
      "saki_click.webp",
      "expression/happy.webp",
      "expression/anim/happy_f1.webp"
    ]
  }
}
```

`assetRoot` 是插件内的前缀。空字符串表示资源直接铺在插件根目录，且相对路径与 `/assets/` 对齐：`expression/happy.webp` 覆盖 `/assets/expression/happy.webp`。

务必写 `skin.files`。面板会把这份清单与文件接口的结果取并集；不写的话，接口漏列的路径会继续显示原图。

`skin.mapping` 可以把清单键指到另一份文件，只用于补路径，不要用它把缺失动作伪装成已覆盖。

### 建议覆盖的路径

根目录：

```
head.webp
sakiicon.webp
sakiicon2.webp
saki_click.webp
tiebian.webp
hang.webp
lie.webp
saki_files.webp
shuru.webp
shuru_sit.webp
```

`saki_click.webp` 是启动器悬停（眨眼挥手）。`sakiicon.webp` / `sakiicon2.webp` 是默认一对。

表情静图（`expression/`）：

```
normal  think  worry  happy  shy  wink  cry  surprised  pout
OK  sorry  sleepy  eating  working  reading  checkfiles
gaming  listen  waiting  writing  terminal  search  diagnose
rollback  blocked  singing  upset  middlefinger
pickup1  pickup2  speaking1  speaking2
empty_healthy  empty_instances  empty_tasks  empty_logs
daemon_offline  page_404
```

六帧循环（`expression/anim/<名>_f1.webp` … `_f6.webp`）：

```
happy  shy  wink  surprised  pout  sorry  cry  eating  sleepy
OK  think  worry  working  reading  checkfiles  upset  gaming
listen  waiting  writing  terminal  search  diagnose  rollback
blocked  middlefinger  singing
```

桌宠（`pet/`）：

```
idle  hover  sit  sleep  lie  hang  look  run  roll  walk
walk2  walk3  climb  fall  pickup  happy  shy  poke  yawn
pout  blink  drink  doctor  eat  bath
```

其中 walk / climb / bath / doctor / eat 另有 `_f1` 到 `_f6`。

图用 webp。透明底要扣干净，不要留白边。缺的文件保持原版即可。

同一时刻只有一套形象生效。

---

## 游戏 game

对照 [plugins/saki-game-fruit-slice](./plugins/saki-game-fruit-slice)。

游戏是一份能单独打开的 HTML，跑在沙箱 iframe 里。扩展工坊可以「开始游玩」，Saki 手机里也会出现图标。

```
saki-game-example/
  saki-plugin.json
  preview.webp
  index.html
  assets/
    sprite.webp
```

```json
{
  "type": "game",
  "game": {
    "entry": "index.html",
    "title": "切水果",
    "width": 720,
    "height": 540
  }
}
```

`width` / `height` 只影响弹窗初值，不是画布硬限制。全屏由面板提供。

iframe 沙箱：`allow-scripts allow-pointer-lock`。没有 `allow-same-origin`，也就没有对父页面 DOM、Cookie、本机存储的访问。资源用相对路径，相对 `index.html` 所在目录：

```html
<img src="assets/apple.webp" alt="" />
```

不要依赖外部登录或跨域 API。需要声音、指针锁定时在页内自己处理。刷新按钮会重载 iframe，等于重开一局。

---

## 组件 widget

组件同样是沙箱 HTML，清单如下。

```
saki-widget-example/
  saki-plugin.json
  preview.webp
  index.html
```

```json
{
  "type": "widget",
  "widget": {
    "entry": "index.html",
    "mountPoint": "dashboard",
    "width": 360,
    "height": 260
  }
}
```

`mountPoint` 为 `dashboard`、`sidebar` 或 `settings`。`height` 是卡片内容区高度，展开时面板会按 1.5 倍估算。

约束与游戏相同：相对路径、不要碰父页面。启用后即可加载；工坊里没有「应用」按钮，只有启用 / 停用。

本仓库暂无组件样例。按上面的目录提交即可。

---

## 语言 locale

对照 [plugins/saki-locale-ja](./plugins/saki-locale-ja)。

语言包给面板增加一个可选界面语言。内置三种：简体中文、繁体中文、English。插件语言会出现在 **系统设置 → 面板语言** 下拉框。

```
saki-locale-fr/
  saki-plugin.json
  fr.json
  preview.webp
```

```json
{
  "type": "locale",
  "locale": {
    "language": "fr-FR",
    "translations": "fr.json",
    "label": "Français"
  }
}
```

| 字段 | 说明 |
| --- | --- |
| `language` | BCP 47，如 `ja-JP`、`fr-FR`。会成为下拉框的值 |
| `translations` | 插件内 JSON 路径 |
| `label` | 下拉框显示名，缺省则用 `displayName` |
| `flag` | 可选，短标记 |

### 怎么切换

1. 在扩展工坊启用该语言包。
2. 打开 **系统设置**，找到 **面板语言**。
3. 选中插件登记的语言，例如 `ja-JP`。
4. 停用插件后，该选项会从列表里消失；若当前正用这种语言，请先改回简体或英文。

登录页的语言选择同样走这张表。装好并启用后即可在未登录时切换。

### 词典怎么写

JSON 是扁平的字符串表。键有两类，建议都写：

1. 面板的 i18n 键，给 `t("nav.dashboard")` 这类调用用。
2. 界面上的中文原句，给仍按 DOM 文本替换的区域用。

```json
{
  "nav.dashboard": "概要",
  "nav.instances": "インスタンス",
  "common.save": "保存",
  "auth.loginSubmit": "ログイン",
  "settings.language": "パネル言語",
  "实例管理": "インスタンス管理",
  "扩展工坊": "拡張ストア"
}
```

常用键前缀：`common`、`nav`、`auth`、`account`、`users`、`roles`、`settings`、`view`、`context`。完整列表见面板源码 `apps/web/src/i18n/translations.ts` 的 `zh-CN` 段。日语示范在 `plugins/saki-locale-ja/ja.json`。

缺的键会回退到简体中文。不必一次译完，先覆盖导航、登录、按钮，再按使用频率补。

键必须与源文或 i18n 键完全一致，包括标点。不要把两种键混成 nested 对象。

同一语言代码不要装两份；后注册的会盖住先注册的。

---

## 封面

`icon` 指向一张图，工坊卡片用 `object-fit: cover` 铺满。请统一 **4:3 横版** webp，建议 1600×1200。

- 主体放在画面中部，四周留一点，避免贴边后被裁掉。
- 不要用纯黑底立绘直接当封面，卡片里会裁得很怪。
- 图上不要写插件名或版本号，名称由清单显示。
- 浅色、深色工坊背景都要能看清。

---

## 登记与贡献

示范插件收在本仓库。合并后会出现在社区精选（取决于 `registry.json`）。

1. Fork 本仓库，从默认分支拉出题目分支。
2. 在 `plugins/<name>/` 放下完整插件，根目录必须有 `saki-plugin.json`。
3. 在 `registry.json` 追加一项：

```json
{
  "name": "saki-theme-example",
  "displayName": "显示名",
  "description": "一句话。",
  "author": "你的名字",
  "type": "theme",
  "version": "1.0.0",
  "repo": "EthanChan050430/saki-plugins",
  "ref": "master",
  "icon": "preview.webp",
  "featured": true
}
```

`name` 必须与清单里的 `name` 相同。`repo` / `ref` 在官方仓合并后保持指向本仓库即可；你自己的独立仓库则写成自己的 `所有者/仓库`。

4. 打开 Pull Request。说明类型、做了什么、在哪几个页面看过。
5. 维护者合并到默认分支。面板用 Git 提交或清单版本检测更新。

独立仓库也可以不进本仓：把仓库地址填进工坊的 GitHub 安装框。目录里有一份合法清单即可。Monorepo 同样支持。

请保持 diff 干净：不要提交编辑器备份、系统缩略图、超大无用原片。webp 体积适中。主题 CSS 不要误伤立绘。形象不要用错姿势顶缺图。

---

## 常见问题

**工坊里看不到本地插件。**  
文件夹必须在面板的 `data/plugins/<name>/`，且其中有合法清单。刷新工坊。`name` 含非法字符会被忽略。

**主题只换了颜色。**  
只改 `--primary` 不够。给 `htmlClass` 写侧栏、顶栏、卡片、登录页。登录页依赖当前主题接口，样式表要能覆盖玻璃和圆角。

**形象装上了，部分动作还是原版。**  
缺文件就会回退原版。补上对应路径，并写入 `skin.files`。悬停用 `saki_click.webp`，不是 `pet/hover.webp`。

**动画帧不播。**  
六帧路径必须是 `expression/anim/<名>_f1.webp` 到 `_f6.webp`，文件名与静图名一致（`think` 对应 `think_f1`）。

**游戏是白屏。**  
看 iframe 里的相对路径是否相对 `index.html`。沙箱进不去父页面。用浏览器开发者工具直接打开插件资源 URL 检查 404。

**语言包启用了，界面没变。**  
启用不等于切换。到 **系统设置 → 面板语言** 选插件登记的 `language`。词典键要和源键一致。

**改了文件浏览器还是旧的。**  
把清单 `version` 加一后刷新。主题、形象、封面的资源地址都带版本参数。

**如何只更新某一个插件。**  
工坊对有 `sourceRepo` 的项提供检查更新。独立仓库改版本并推送即可。本仓库则改对应子目录再推默认分支。
