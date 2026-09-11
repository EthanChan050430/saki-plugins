<p align="center">
  <img src="./docs/header-zh.svg" width="1100" alt="Saki Panel 插件开发">
</p>

<p align="center">
  <a href="./README.md"><img src="./docs/pill-zh-on.svg" height="32" alt="中文"></a>
  &nbsp;
  <a href="./README.en.md"><img src="./docs/pill-en-off.svg" height="32" alt="English"></a>
</p>

<p align="center">
  <img src="./docs/rule.svg" width="1100" alt="">
</p>

插件不是仓库里那几份样例的别名。样例只是已经写好的成品。你要交的是：一个目录、一份清单、以及面板按类型去读的文件。下面按「从空文件夹写到工坊能点」来讲。写完某一类，再用样例对照自己漏了什么。

<p align="center">
  <a href="#0-十分钟做出能被面板看见的东西">十分钟</a>
  ·
  <a href="#1-面板究竟怎么加载插件">加载</a>
  ·
  <a href="#2-清单要写什么">清单</a>
  ·
  <a href="#3-写主题">主题</a>
  ·
  <a href="#4-写形象">形象</a>
  ·
  <a href="#5-写游戏">游戏</a>
  ·
  <a href="#6-写组件">组件</a>
  ·
  <a href="#7-写语言包">语言</a>
  ·
  <a href="#8-封面">封面</a>
  ·
  <a href="#9-上架与贡献">上架</a>
</p>

## 0. 十分钟做出能被面板看见的东西

面板不读本 git 仓库的工作树。它读运行目录里的 `data/plugins/<插件id>/`。所以开发循环是：在那个目录里写文件，刷新扩展工坊。

新建：

```
data/plugins/saki-theme-demo/
  saki-plugin.json
  theme.css
  preview.webp
```

`saki-plugin.json`：

```json
{
  "name": "saki-theme-demo",
  "version": "0.1.0",
  "displayName": "我的第一份主题",
  "description": "只改主色，用来确认加载链路。",
  "author": "你的名字",
  "type": "theme",
  "icon": "preview.webp",
  "minPanelVersion": "3.5.0",
  "theme": {
    "css": "theme.css",
    "htmlClass": "saki-plugin-theme-demo"
  }
}
```

`theme.css`：

```css
html.saki-plugin-theme-demo {
  --primary: #1f4bff !important;
  --primary-hover: #1636c9 !important;
  --primary-light: rgba(31, 75, 255, 0.14) !important;
}
```

`preview.webp` 先放一张 **4:3 横版** 图，1600×1200 即可，没有图工坊卡片会很难看，但不影响加载。

然后打开扩展工坊，应能看到「我的第一份主题」。点 **应用主题**。主色从粉变成蓝，说明清单、CSS、htmlClass 三条链路都通了。再往下加侧栏、登录页、圆角，都是在这份 CSS 里接着写。

`name` 会变成插件 id 和文件夹名。只允许字母、数字、下划线、连字符，必须以字母或数字开头。装好后不要随便改 `name`，等于换了一个插件。

> [!IMPORTANT]
> 改 CSS / 图 / JSON 之后浏览器可能仍拿旧文件。把 `version` 从 `0.1.0` 改成 `0.1.1` 再刷新。主题、形象、封面的地址都带 `?v=`。

## 1. 面板究竟怎么加载插件

安装器在 GitHub 仓库里递归找 `saki-plugin.json`（最多五层）。找到一份，那一层就是插件根，**不再往这个根里面搜**。所以不要把第二份清单塞进第一份的 `assets/`。

一个仓可以放很多插件，本仓库就是这样：

```
saki-plugins/
  registry.json
  plugins/
    saki-skin-maid/      ← 这里有清单
    saki-theme-geo/
    saki-game-fruit-slice/
    saki-locale-ja/
```

工坊里填 `所有者/仓库` 会把找得到的插件都装上。只装一个：`所有者/仓库:插件名`，例如 `EthanChan050430/saki-plugins:saki-theme-geo`。

装完以后：

| 你写的 type | 面板做什么 | 你要再点一下 |
| --- | --- | --- |
| `theme` | 插入 `<link>` 指向你的 CSS，给 `<html>` 加 `htmlClass` | **应用主题**。同时只能一套。登录页未登录也会拉当前主题。 |
| `skin` | 按 `/assets/` 相对路径换图 | **更换形象**。同时只能一套。没提供的路径仍是原图。 |
| `game` | 用沙箱 iframe 打开 `game.entry` | **开始游玩**，或在 Saki 手机里开。 |
| `widget` | 同样是沙箱 iframe | **启用**。没有「应用」。 |
| `locale` | 把 JSON 词典登记进语言列表 | **启用**，再到 **系统设置 → 面板语言** 里选。 |

主题和形象是「当前正在用的那一套」。游戏、语言、组件是「启用了就挂着」。

静态文件的 URL 是：

```
/api/plugins/<id>/assets/<相对插件根的路径>?v=<version>
```

游戏、主题、封面都走这条。相对路径写错就是 404，iframe 里白屏。

## 2. 清单要写什么

文件名必须是 `saki-plugin.json`，放在插件根，UTF-8，不能有注释。缺 `name`、`type`、`displayName` 会被当成无效。

```json
{
  "name": "saki-theme-demo",
  "version": "0.1.0",
  "displayName": "显示在卡片上的名字",
  "description": "一句话。",
  "author": "你的名字",
  "type": "theme",
  "icon": "preview.webp",
  "homepage": "https://github.com/you/repo",
  "minPanelVersion": "3.5.0"
}
```

`type` 只能是 `theme` `skin` `game` `widget` `locale`。然后补上对应那一块对象。示范插件把 `minPanelVersion` 写成 `3.5.0`。

下面五节分别是：你要新建哪些文件、里面写什么、面板读完之后会发生什么、你怎么确认它生效。

## 3. 写主题

目标不是换一个粉色。是交出一份 CSS，让侧栏、顶栏、按钮、对话框、登录页按你的规则画。

目录最小集：

```
saki-theme-demo/
  saki-plugin.json
  preview.webp
  theme.css
```

清单里的主题块：

```json
{
  "theme": {
    "css": "theme.css",
    "htmlClass": "saki-plugin-theme-demo",
    "backgrounds": {
      "light": "bg-light.webp",
      "dark": "bg-dark.webp"
    }
  }
}
```

`css` 是相对插件根的路径。`htmlClass` 必须以 `saki-plugin-theme-` 开头，后面只能是字母、数字、下划线、连字符，总长有上限。写错了面板不会往 `<html>` 上加 class，你的选择器全部落空。

`backgrounds` 可选。写了之后，面板会在 `<html>` 上设：

```css
--plugin-theme-bg-light: url("/api/plugins/.../bg-light.webp");
--plugin-theme-bg-dark: url("/api/plugins/.../bg-dark.webp");
```

你在 CSS 里把它接到 `--app-background-image` 上。不写就继续用面板自己的背景。

### 第一刀：令牌

面板自己的液态玻璃写了很多 `!important`。你的表在它后面插入，但选择器还是要够具体，令牌建议自己也带 `!important`：

```css
html.saki-plugin-theme-demo {
  --primary: #1f4bff !important;
  --primary-hover: #1636c9 !important;
  --primary-light: rgba(31, 75, 255, 0.14) !important;
  --radius-control: 0px !important;
  --radius-modal: 0px !important;
  --radius-card: 0px !important;
  --glass-bg: #fffaf0 !important;
  --glass-border: #12141a !important;
  --surface-bg: #fffaf0 !important;
  --text-dark: #12141a !important;
  --app-background-image: var(--plugin-theme-bg-light) !important;
}

html.saki-plugin-theme-demo[data-theme="dark"] {
  --primary: #6b8cff !important;
  --app-background-image: var(--plugin-theme-bg-dark) !important;
}
```

只写到这里，按钮主色和圆角会跟着变，侧栏多半还是玻璃。这是正常的：很多规则写死了选择器，不经过令牌。

### 第二刀：点名要改的壳

把你看见的块直接写掉。class 都在面板样式里，常见的有 `.sidebar`、`.topbar-inner`、`.primary-button`、`.modal-panel`、`.login-container`、`.glass-panel`、`.instance-card`。

```css
html.saki-plugin-theme-demo .sidebar {
  background: #12141a !important;
  border-radius: 0 !important;
  backdrop-filter: none !important;
}

html.saki-plugin-theme-demo .primary-button {
  background: var(--primary) !important;
  border: 2px solid #12141a !important;
  border-radius: 0 !important;
}
```

深色模式用 `html.saki-plugin-theme-demo[data-theme="dark"]`。不要只写 `[data-theme="dark"]`，会和面板自己的深色规则缠在一起。

立绘不要裁：

```css
html.saki-plugin-theme-demo .saki-character-art,
html.saki-plugin-theme-demo .saki-character-art img {
  clip-path: none !important;
}
```

### 你怎么知道写对了

应用主题后看：侧栏背景、主按钮、任意一个对话框、退出后再看登录页。四处都变了，才算主题而不是换色。关掉主题，粉玻璃应完整回来。

## 4. 写形象

形象不是「交一套立绘文件夹」这么笼统。面板把 `/assets/expression/happy.webp` 这类路径换成你的插件文件。机制是：清单（加上文件接口）里出现过的相对路径才覆盖；没出现的继续用原图。

先做最小可运行的形象——只换悬停图。启动器鼠标移上去用的是 `saki_click.webp`。

```
saki-skin-demo/
  saki-plugin.json
  preview.webp
  saki_click.webp
```

```json
{
  "type": "skin",
  "skin": {
    "assetRoot": "",
    "files": ["saki_click.webp", "preview.webp"]
  }
}
```

`assetRoot` 是插件内部前缀。空字符串表示 `saki_click.webp` 就在插件根上，对应面板的 `/assets/saki_click.webp`。如果你把图都放在 `art/` 下，就写 `"assetRoot": "art"`，文件则是 `art/saki_click.webp`。

把插件拷进 `data/plugins/saki-skin-demo/`，刷新工坊，点 **更换形象**。鼠标移到 Saki 启动器上，应看到你那张图。看不到：检查 `files` 有没有写、路径是不是和 `/assets/` 对齐、`version` 有没有加。

> [!WARNING]
> 不要用 `skin.mapping` 把缺失的动作指到另一张姿势上。缺文件就让原版留下来。悬停也不是 `pet/hover.webp`。

### 继续加表情

静图放在 `expression/<名>.webp`。六帧循环必须是：

```
expression/anim/<名>_f1.webp
expression/anim/<名>_f2.webp
...
expression/anim/<名>_f6.webp
```

`think` 的帧是 `think_f1`，不是 `thinking_f1`。每加一张，都写进 `skin.files`。面板拿这份清单做覆盖，不写等于没交。

<details>
<summary>面板会读的路径</summary>

根：`head.webp`，`sakiicon.webp`，`sakiicon2.webp`，`saki_click.webp`，`tiebian.webp`，`hang.webp`，`lie.webp`，`saki_files.webp`，`shuru.webp`，`shuru_sit.webp`。

`expression/`：`normal`，`think`，`worry`，`happy`，`shy`，`wink`，`cry`，`surprised`，`pout`，`OK`，`sorry`，`sleepy`，`eating`，`working`，`reading`，`checkfiles`，`gaming`，`listen`，`waiting`，`writing`，`terminal`，`search`，`diagnose`，`rollback`，`blocked`，`singing`，`upset`，`middlefinger`，`pickup1`，`pickup2`，`speaking1`，`speaking2`，`empty_healthy`，`empty_instances`，`empty_tasks`，`empty_logs`，`daemon_offline`，`page_404`。

六帧循环：`happy`，`shy`，`wink`，`surprised`，`pout`，`sorry`，`cry`，`eating`，`sleepy`，`OK`，`think`，`worry`，`working`，`reading`，`checkfiles`，`upset`，`gaming`，`listen`，`waiting`，`writing`，`terminal`，`search`，`diagnose`，`rollback`，`blocked`，`middlefinger`，`singing`。

`pet/`：`idle`，`hover`，`sit`，`sleep`，`lie`，`hang`，`look`，`run`，`roll`，`walk`，`walk2`，`walk3`，`climb`，`fall`，`pickup`，`happy`，`shy`，`poke`，`yawn`，`pout`，`blink`，`drink`，`doctor`，`eat`，`bath`。其中 `walk` `climb` `bath` `doctor` `eat` 另有 `_f1`–`_f6`。

</details>

图用 webp。透明边扣干净。同一时刻只能亮一套形象。

## 5. 写游戏

游戏是一份能单独在浏览器打开的 HTML。面板把它放进 iframe，沙箱是 `allow-scripts allow-pointer-lock`，**没有** `allow-same-origin`。你碰不到父页面、Cookie、`localStorage`（父域的）。分数要存，只能记在内存里，刷新就没了。

```
saki-game-demo/
  saki-plugin.json
  preview.webp
  index.html
  assets/
    player.webp
```

```json
{
  "type": "game",
  "game": {
    "entry": "index.html",
    "title": "我的游戏",
    "width": 720,
    "height": 540
  }
}
```

`entry` 相对插件根。`width` / `height` 只是弹窗初始大小，不是画布上限。工坊的刷新会重载 iframe。

`index.html` 里的资源必须相对这份 HTML：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>demo</title>
  <style>
    html, body { margin: 0; height: 100%; background: #111; color: #fff; }
  </style>
</head>
<body>
  <canvas id="c"></canvas>
  <script>
    const img = new Image();
    img.src = "assets/player.webp";
  </script>
</body>
</html>
```

不要写 `/assets/player.webp`，那是面板自己的树，不是你的插件。白屏时用开发者工具直接打开：

```
/api/plugins/saki-game-demo/assets/index.html
/api/plugins/saki-game-demo/assets/assets/player.webp
```

404 就是路径写错。游戏启用后也会进 Saki 手机的列表，图标用清单的 `icon`。

不要去调需要登录的跨域 API。指针锁定在用户点进画布之后再请求。

## 6. 写组件

和游戏同一套 iframe 沙箱。清单换成 `widget`：

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

`mountPoint` 为 `dashboard`、`sidebar` 或 `settings`。`height` 是卡片内容高度，展开时面板按大约 1.5 倍来估。工坊里只有启用，没有「应用」。资源路径规则与游戏相同。本仓库暂时没有组件成品，按这套交即可。

## 7. 写语言包

语言包不是「翻译 README」。它往面板的语言列表里加一个选项。内置三种：简体中文、繁体中文、English。你的包启用后，**系统设置 → 面板语言**（登录页同一处）会出现 `label`。

先写最小包，只翻几条，确认下拉框和 `t()` 通了，再慢慢补词典。

```
saki-locale-fr/
  saki-plugin.json
  fr.json
  preview.webp
```

```json
{
  "name": "saki-locale-fr",
  "version": "0.1.0",
  "displayName": "Français",
  "description": "法语界面。",
  "author": "你的名字",
  "type": "locale",
  "icon": "preview.webp",
  "minPanelVersion": "3.5.0",
  "locale": {
    "language": "fr-FR",
    "translations": "fr.json",
    "label": "Français"
  }
}
```

`language` 用 BCP 47，会成为下拉框的值。`translations` 是插件内 JSON 路径。

`fr.json` 是扁平对象，不要嵌套：

```json
{
  "nav.dashboard": "Aperçu",
  "nav.instances": "Instances",
  "common.save": "Enregistrer",
  "common.cancel": "Annuler",
  "auth.loginSubmit": "Connexion",
  "settings.language": "Langue du panneau",
  "扩展工坊": "Atelier d’extensions"
}
```

键有两种，两种都写更稳：

1. 给 `t("nav.dashboard")` 用的 i18n 键。完整列表在面板源码 `apps/web/src/i18n/translations.ts` 的 `zh-CN` 段。常用前缀：`common`、`nav`、`auth`、`account`、`users`、`roles`、`settings`、`view`、`context`。
2. 页面上仍按原文替换的中文句子，例如 `扩展工坊`。键必须和源文完全一致，包括标点。

缺的键退回简体。不必一次译完。

切换步骤，少一步界面都不会变：

1. 扩展工坊里 **启用** 这份语言包。
2. 打开 **系统设置**，找到 **面板语言**。
3. 选 `fr-FR`（或你写的 `language`）。
4. 停用插件前，如果正在用这种语言，先改回简体或英文，否则选项消失后界面会停在一种已经卸掉的词典上。

同一个 `language` 不要装两份，后登记的会盖住先登记的。日语成品在 `plugins/saki-locale-ja/ja.json`，可当词典密度的参考，不是必须抄结构。

## 8. 封面

`icon` 被工坊卡片用 `object-fit: cover` 铺满。写成 **4:3 横版** webp，建议 1600×1200。主体放中间。纯黑底立绘在卡片里会裁成一块难看的切片。不要把插件名画进图里，名字走 `displayName`。

## 9. 上架与贡献

本地验证过之后，有两条路。

**自己的 GitHub 仓。** 把仓地址填进工坊。能搜到清单就能装。一个仓多份插件也可以。

**进本仓库的社区精选。** Fork，从 `master` 开分支，在 `plugins/<name>/` 放下能独立安装的一份。再在根目录 `registry.json` 加一项，`name` 与清单相同：

```json
{
  "name": "saki-theme-demo",
  "displayName": "我的第一份主题",
  "description": "一句话。",
  "author": "你的名字",
  "type": "theme",
  "version": "0.1.0",
  "repo": "EthanChan050430/saki-plugins",
  "ref": "master",
  "icon": "preview.webp",
  "featured": true
}
```

合进本仓之后 `repo` 保持指向这里。Pull Request 写清：类型、你点过哪些页面、预期看到什么。不要交编辑器备份和未压缩原片。主题不要切到脸上。形象不要拿错姿势顶空缺。改文件就加 `version`，面板靠提交或版本号判断更新。

写不下去时再打开 `plugins/` 里对应类型的成品，看它的清单和文件是怎么对上上面这些规则的。
