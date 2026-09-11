[中文](./README.md) · English

# Saki Panel plugins

A plugin is a folder. Put `saki-plugin.json` at its root and the panel will load it.

This repository holds the samples we ship, and the `registry.json` file that the Plugin Workshop reads for featured entries. The fastest way to start is to copy the sample closest to what you want.

Here you will find the maid skin, the geometric theme, the fruit-slicing game, and a Japanese locale. Only one theme and one skin can be active at a time. Games and locales can run together.

<p>
  <a href="#layout">Layout</a>
  · <a href="#installing">Install</a>
  · <a href="#the-manifest">Manifest</a>
  · <a href="#themes">Theme</a>
  · <a href="#skins">Skin</a>
  · <a href="#games-and-widgets">Game</a>
  · <a href="#locales">Locale</a>
  · <a href="#cover-art">Cover</a>
  · <a href="#sending-a-change">Contribute</a>
</p>

## Layout

The installer walks the repository looking for `saki-plugin.json`, at most five directories deep. The folder that contains the file is the plugin root; it does not search inside that folder afterwards. Do not nest a manifest under another plugin’s `assets/`.

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

Name the folder after `name` in the manifest. `name` may contain letters, digits, underscores and hyphens, and must start with a letter or a digit.

## Installing

Open the Plugin Workshop. Featured entries come from `registry.json` in this repository. Pasting `EthanChan050430/saki-plugins` installs every plugin the installer can see; `EthanChan050430/saki-plugins:saki-theme-geo` installs only that one.

If GitHub is slow, test a mirror in the Workshop first. This repository’s default branch is `master`.

Installing is not the same as turning something on. Apply a theme with “Apply theme”, a skin with “Change appearance”. Enable a locale, then pick it under **System settings → Panel language** — enabling alone does not switch the UI. Games launch from the Workshop and also appear on Saki’s phone.

When you iterate, copy into the running panel’s `data/plugins/<name>/`, not into this git tree, then refresh the Workshop. If the browser keeps an old file, bump `version` in the manifest.

## The manifest

Every plugin root needs `saki-plugin.json` in UTF-8, with no comments. Missing `name`, `type` or `displayName` means the installer skips the folder.

```json
{
  "name": "saki-theme-geo",
  "version": "2.0.3",
  "displayName": "Geometry",
  "description": "A constructivist shell for the panel.",
  "author": "DreamStarry",
  "type": "theme",
  "icon": "preview.webp",
  "minPanelVersion": "3.5.0"
}
```

`type` is `theme`, `skin`, `game`, `widget` or `locale`. Add the matching block: `theme`, `skin`, `game`, `widget` or `locale`. `version` is appended to asset URLs so caches drop. Sample plugins set `minPanelVersion` to `3.5.0`.

## Themes

See [`plugins/saki-theme-geo`](./plugins/saki-theme-geo). Changing `--primary` only tints the accent; the sidebar and login page stay glass. Hang a class on `<html>` and restyle the shell. The login page fetches the active theme before anyone signs in.

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

`htmlClass` must start with `saki-plugin-theme-`, then letters, digits, underscores or hyphens. Anything else is ignored.

The stylesheet is injected after the panel’s own CSS. Prefix every rule with your class, and check both light and dark:

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

Radii, glass, surfaces, type colour and page backgrounds live as tokens in the panel’s `theme-liquid.css`. `--app-background-image` is the desktop field; `--mobile-background-image` is the narrow one.

Do not clip Saki. Leave `.saki-character-art`, ordinary `img` and `video` alone. Only one theme is active; switching or disabling removes the class and the `<link>`.

## Skins

See [`plugins/saki-skin-maid`](./plugins/saki-skin-maid). Files overlay the panel’s `/assets/` tree by relative path: `expression/happy.webp` replaces the happy face; a path you omit keeps the original. Do not map a missing pose onto a different drawing.

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

`assetRoot` is a prefix inside the plugin; leave it empty if files sit at the plugin root. Always list `skin.files`. The panel unions that list with the files API; a path left out keeps showing the stock art. `skin.mapping` is for renaming a path, not for faking coverage.

The launcher pair is `sakiicon.webp` and `sakiicon2.webp`. Hover is `saki_click.webp`, not `pet/hover.webp`. Use webp, and key the edges clean.

<details>
<summary>Paths the panel actually reads</summary>

Root: `head.webp`, `sakiicon.webp`, `sakiicon2.webp`, `saki_click.webp`, `tiebian.webp`, `hang.webp`, `lie.webp`, `saki_files.webp`, `shuru.webp`, `shuru_sit.webp`.

Stills in `expression/`: `normal`, `think`, `worry`, `happy`, `shy`, `wink`, `cry`, `surprised`, `pout`, `OK`, `sorry`, `sleepy`, `eating`, `working`, `reading`, `checkfiles`, `gaming`, `listen`, `waiting`, `writing`, `terminal`, `search`, `diagnose`, `rollback`, `blocked`, `singing`, `upset`, `middlefinger`, `pickup1`, `pickup2`, `speaking1`, `speaking2`, `empty_healthy`, `empty_instances`, `empty_tasks`, `empty_logs`, `daemon_offline`, `page_404`.

Six-frame cycles live at `expression/anim/<name>_f1.webp` … `_f6.webp`. The name must match the still (`think` → `think_f1`). Cycles: `happy`, `shy`, `wink`, `surprised`, `pout`, `sorry`, `cry`, `eating`, `sleepy`, `OK`, `think`, `worry`, `working`, `reading`, `checkfiles`, `upset`, `gaming`, `listen`, `waiting`, `writing`, `terminal`, `search`, `diagnose`, `rollback`, `blocked`, `middlefinger`, `singing`.

Under `pet/`: `idle`, `hover`, `sit`, `sleep`, `lie`, `hang`, `look`, `run`, `roll`, `walk`, `walk2`, `walk3`, `climb`, `fall`, `pickup`, `happy`, `shy`, `poke`, `yawn`, `pout`, `blink`, `drink`, `doctor`, `eat`, `bath`. `walk`, `climb`, `bath`, `doctor` and `eat` also have `_f1`–`_f6`.

</details>

## Games and widgets

Fruit slice lives in [`plugins/saki-game-fruit-slice`](./plugins/saki-game-fruit-slice). The entry is a self-contained HTML page in a sandboxed iframe.

```json
{
  "game": {
    "entry": "index.html",
    "title": "Fruit Slice",
    "width": 720,
    "height": 540
  }
}
```

Width and height size the first window; fullscreen is a panel control. The sandbox is `allow-scripts allow-pointer-lock` without `allow-same-origin`, so the game cannot see the parent page, cookies or local storage. Load images from paths relative to `index.html`:

```html
<img src="assets/apple.webp" alt="" />
```

Refresh in the Workshop reloads the iframe. Do not call cross-origin login APIs.

A widget is the same kind of iframe, with `type` set to `widget`. `mountPoint` may be `dashboard`, `sidebar` or `settings`. `height` is the card body. The Workshop only enables or disables it. There is no widget sample in this repository yet.

## Locales

The Japanese pack is [`plugins/saki-locale-ja`](./plugins/saki-locale-ja). The panel ships Simplified Chinese, Traditional Chinese and English. Once enabled, a locale appears under **System settings → Panel language**, and on the login screen.

```json
{
  "locale": {
    "language": "ja-JP",
    "translations": "ja.json",
    "label": "日本語"
  }
}
```

`language` is BCP 47. `translations` is a JSON file in the plugin. The file is a flat map. Write both kinds of key: i18n keys used by `t("nav.dashboard")`, and the original Chinese phrases still rewritten from the DOM.

```json
{
  "nav.dashboard": "Overview",
  "auth.loginSubmit": "Sign in",
  "settings.language": "Panel language",
  "扩展工坊": "Plugin Workshop"
}
```

Keys must match the source, punctuation included. Missing entries fall back to Simplified Chinese. Translate navigation, sign-in and common buttons first. The full key list is the `zh-CN` object in `apps/web/src/i18n/translations.ts`; the Japanese sample is `ja.json`.

Do not install two packs for the same `language`. Disabling the plugin removes it from the dropdown — switch away first if you are currently using it.

## Cover art

`icon` is drawn with `object-fit: cover` on the Workshop card. Use a **4:3 landscape** webp, 1600×1200 if you can. Keep the subject off the edges. A cut-out on black crops badly. Do not paint the plugin name onto the image; the manifest already has it.

## Sending a change

Fork, branch from `master`, and put a self-contained plugin in `plugins/<name>/` with a manifest at the root. Add a row to `registry.json` whose `name` matches the manifest. After merge, `repo` stays `EthanChan050430/saki-plugins` and `ref` is `master`.

Say in the pull request what kind it is, which screens you opened, and how you clicked through them. Skip uncompressed originals. Themes should not clip the face. Skins should not stand in one pose for another.

You can also skip this repository: paste your own GitHub URL into the Workshop if it contains a valid manifest. A monorepo of several plugins is fine.

The panel notices updates by git commit or by `version`. Bump `version` when you change files.
