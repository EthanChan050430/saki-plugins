中文 · [English](./README.en.md)

# Saki Panel 插件

插件就是一个文件夹。根上放 `saki-plugin.json`，面板就会认。

这个仓库装着我们自己写的几份样例，也装着扩展工坊「社区精选」读的 `registry.json`。想做新的，先把对应那一份拷出来改，比从空白清单起步要快。

现在仓库里有：女仆装（形象）、简约几何（主题）、切水果（游戏）、日本語（语言）。主题和形象同时只能亮一套；游戏和语言包可以一起开。

<p>
  <a href="#放在哪里">放置</a>
  · <a href="#装到面板里">安装</a>
  · <a href="#清单">清单</a>
  · <a href="#主题">主题</a>
  · <a href="#形象">形象</a>
  · <a href="#游戏与组件">游戏</a>
  · <a href="#语言">语言</a>
  · <a href="#封面">封面</a>
  · <a href="#送来">贡献</a>
</p>

## 放在哪里

安装器会在仓库里往下找 `saki-plugin.json`，最多五层。找到一份，那一层就是插件根，不再往里面搜。所以不要把清单塞进另一份插件的 `assets/`。

```
saki-plugins/
  README.md
  README.en.md
  registry.json
  plugins/
    saki-skin-maid/
    saki-theme-geo/
    saki-game-fruit-slice/
    saki-locale-ja/
```

文件夹名最好和清单里的 `name` 一样。`name` 只允许字母、数字、下划线和连字符，必须以字母或数字起头。

## 装到面板里

打开扩展工坊。社区精选走本仓库的 `registry.json`。自己填地址时，`EthanChan050430/saki-plugins` 会把仓里能找到的插件都装上；只想要其中一个，写成 `EthanChan050430/saki-plugins:saki-theme-geo`。

GitHub 慢的话，先在工坊里测镜像。分支用仓库真实的默认分支，这个仓是 `master`。

装完还没完。主题要点「应用主题」，形象要点「更换形象」。语言包先启用，再到 **系统设置 → 面板语言** 里选，光启用界面不会变。游戏在工坊里开，也会出现在 Saki 的手机里。

改自己的插件时，拷到面板运行目录的 `data/plugins/<name>/`，不是拷到本仓库的工作树。刷新工坊。图和 CSS 被浏览器记住了，就把清单里的 `version` 加一。

## 清单

每个插件根上必须有 `saki-plugin.json`，UTF-8，不能写注释。`name`、`type`、`displayName` 缺一个，安装器会当它不存在。

```json
{
  "name": "saki-theme-geo",
  "version": "2.0.3",
  "displayName": "简约几何",
  "description": "构成主义几何外壳。",
  "author": "DreamStarry",
  "type": "theme",
  "icon": "preview.webp",
  "minPanelVersion": "3.5.0"
}
```

`type` 是 `theme`、`skin`、`game`、`widget`、`locale` 之一。然后按类型再写一块：`theme`、`skin`、`game`、`widget` 或 `locale`。`version` 会进资源 URL 的查询参数，用来冲掉缓存。`minPanelVersion` 示范插件写成 `3.5.0`。

## 主题

样例在 [`plugins/saki-theme-geo`](./plugins/saki-theme-geo)。只改 `--primary` 会看起来像换了个强调色，侧栏和登录页仍是原来的玻璃。要把 class 挂到 `<html>` 上，连登录页也会在未登录时把这份 CSS 拉下来。

```
saki-theme-geo/
  saki-plugin.json
  preview.webp
  theme.css
  bg-light.webp
  bg-dark.webp
```

```json
{
  "theme": {
    "css": "theme.css",
    "htmlClass": "saki-plugin-theme-geo",
    "backgrounds": {
      "light": "bg-light.webp",
      "dark": "bg-dark.webp"
    }
  }
}
```

`htmlClass` 必须以 `saki-plugin-theme-` 开头，后面跟字母、数字、下划线或连字符。不合这个形状，面板不会往 `<html>` 上加。

样式表插在面板自己的 CSS 之后。选择器带着自己的 class 写，浅色深色都要看：

```css
html.saki-plugin-theme-geo {
  --primary: #1f4bff;
  --radius-control: 0px;
  --radius-modal: 0px;
}

html.saki-plugin-theme-geo .sidebar { }

html.saki-plugin-theme-geo[data-theme="dark"] {
  --primary: #6b8cff;
}
```

圆角、玻璃、表面、字色、整页背景，令牌都在面板的 `theme-liquid.css` 里。`--app-background-image` 管桌面，`--mobile-background-image` 管窄屏。

切角、clip-path 不要落到立绘上。`.saki-character-art`、普通 `img` 和 `video` 保持原样。同时只能亮一套主题，换掉或关掉时 class 和 `<link>` 都会卸走。

## 形象

样例在 [`plugins/saki-skin-maid`](./plugins/saki-skin-maid)。面板按 `/assets/` 的相对路径找文件：插件里有 `expression/happy.webp`，就把原来的开心脸换掉；没有的路径仍用原版。缺一张就空着，别拿另一张姿势去填。

```json
{
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

`assetRoot` 是插件内部的前缀，空字符串表示资源直接铺在插件根上。`skin.files` 请写全。面板拿它和文件接口做并集，漏写的路径会继续显示原图。`skin.mapping` 只用来补路径，不要拿它假装覆盖了不存在的动作。

启动器默认是 `sakiicon.webp` 和 `sakiicon2.webp`，鼠标悬停是 `saki_click.webp`，不是 `pet/hover.webp`。图用 webp，透明边扣干净。

<details>
<summary>路径一览</summary>

根目录：`head.webp`，`sakiicon.webp`，`sakiicon2.webp`，`saki_click.webp`，`tiebian.webp`，`hang.webp`，`lie.webp`，`saki_files.webp`，`shuru.webp`，`shuru_sit.webp`。

`expression/` 静图：`normal`，`think`，`worry`，`happy`，`shy`，`wink`，`cry`，`surprised`，`pout`，`OK`，`sorry`，`sleepy`，`eating`，`working`，`reading`，`checkfiles`，`gaming`，`listen`，`waiting`，`writing`，`terminal`，`search`，`diagnose`，`rollback`，`blocked`，`singing`，`upset`，`middlefinger`，`pickup1`，`pickup2`，`speaking1`，`speaking2`，`empty_healthy`，`empty_instances`，`empty_tasks`，`empty_logs`，`daemon_offline`，`page_404`。

六帧在 `expression/anim/<名>_f1.webp` 到 `_f6.webp`。名字要和静图一致，`think` 对应 `think_f1`。循环包括：`happy`，`shy`，`wink`，`surprised`，`pout`，`sorry`，`cry`，`eating`，`sleepy`，`OK`，`think`，`worry`，`working`，`reading`，`checkfiles`，`upset`，`gaming`，`listen`，`waiting`，`writing`，`terminal`，`search`，`diagnose`，`rollback`，`blocked`，`middlefinger`，`singing`。

`pet/`：`idle`，`hover`，`sit`，`sleep`，`lie`，`hang`，`look`，`run`，`roll`，`walk`，`walk2`，`walk3`，`climb`，`fall`，`pickup`，`happy`，`shy`，`poke`，`yawn`，`pout`，`blink`，`drink`，`doctor`，`eat`，`bath`。其中 `walk`、`climb`、`bath`、`doctor`、`eat` 另有 `_f1` 到 `_f6`。

</details>

## 游戏与组件

切水果在 [`plugins/saki-game-fruit-slice`](./plugins/saki-game-fruit-slice)。入口是一份能单独打开的 HTML，塞在沙箱 iframe 里。

```json
{
  "game": {
    "entry": "index.html",
    "title": "切水果",
    "width": 720,
    "height": 540
  }
}
```

宽高只决定弹窗一开始有多大，全屏是面板的按钮。沙箱是 `allow-scripts allow-pointer-lock`，没有 `allow-same-origin`，所以碰不到父页面、Cookie 和本机存储。图片、音效用相对 `index.html` 的路径：

```html
<img src="assets/apple.webp" alt="" />
```

工坊上的刷新等于重开一局。不要去调跨域登录。

组件是同一种 iframe，清单里写 `widget`。`mountPoint` 可以是 `dashboard`、`sidebar`、`settings`；`height` 是卡片内容高度。工坊里只有启用，没有「应用」。这个仓库暂时没有组件样例。

## 语言

日语包在 [`plugins/saki-locale-ja`](./plugins/saki-locale-ja)。面板自己带简体、繁体和 English。语言包启用后，会在 **系统设置 → 面板语言**（登录页同一处）多出一个选项。

```json
{
  "locale": {
    "language": "ja-JP",
    "translations": "ja.json",
    "label": "日本語"
  }
}
```

`language` 用 BCP 47。`translations` 是插件里的 JSON。词典是扁平的键值表，键有两种，两种都写更稳：给 `t("nav.dashboard")` 用的 i18n 键，以及页面上还按原文替换的中文句子。

```json
{
  "nav.dashboard": "概要",
  "auth.loginSubmit": "ログイン",
  "settings.language": "パネル言語",
  "扩展工坊": "拡張ストア"
}
```

键必须和源文一模一样，包括标点。缺的条目会退回简体。不必一次译完，先覆盖导航、登录和常用按钮。完整键名在面板源码 `apps/web/src/i18n/translations.ts` 的 `zh-CN` 段；日语示范见 `ja.json`。

同一个 `language` 不要装两份。停用插件后，下拉框里的选项会消失——若正在用这种语言，先改回简体或英文。

## 封面

`icon` 那张图会被工坊卡片 `cover` 铺满。请用 **4:3 横版** webp，1600×1200 合适。人放在中间，别贴边。不要拿纯黑底立绘直接当封面，卡片里会裁得很难看。字不要写在图上，名字走清单。

## 送来

Fork，从 `master` 拉分支，在 `plugins/<name>/` 放下能独立安装的一份，根上要有清单。然后在 `registry.json` 里加一项，`name` 与清单相同，`repo` 在合进本仓之后保持 `EthanChan050430/saki-plugins`，`ref` 写 `master`。

Pull Request 里写清：什么类型、改了哪几页、你怎么点过。图不要塞进未压缩的原片，主题不要切到脸上，形象不要拿错姿势顶空缺。

也可以不进本仓。把你自己的 GitHub 地址填进工坊，只要仓里找得到清单。一个仓里多份插件也行。

面板用提交哈希或清单版本判断有没有更新。改完记得把 `version` 加上去。
