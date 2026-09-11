<p align="center">
  <img src="./docs/header-en.svg" width="1100" alt="Saki Panel plugin guide">
</p>

<p align="center">
  <a href="./README.md"><img src="./docs/pill-zh-off.svg" height="32" alt="中文"></a>
  &nbsp;
  <a href="./README.en.md"><img src="./docs/pill-en-on.svg" height="32" alt="English"></a>
</p>

<p align="center">
  <img src="./docs/rule.svg" width="1100" alt="">
</p>

Every folder in this repo is a finished plugin. A plugin is really just three things: **a folder, a manifest file, and the files that match its type**. This guide walks you from an empty folder to something you can click in the Plugin Workshop. When you finish a section, check the sample plugin in this repo to see what you might have missed.

<p align="center">
  <a href="#0-something-the-panel-can-see-in-ten-minutes">Ten minutes</a>
  ·
  <a href="#1-how-the-panel-loads-a-plugin">How it loads</a>
  ·
  <a href="#2-the-manifest">Manifest</a>
  ·
  <a href="#3-writing-a-theme">Theme</a>
  ·
  <a href="#4-writing-a-skin">Skin</a>
  ·
  <a href="#5-writing-a-game">Game</a>
  ·
  <a href="#6-writing-a-widget">Widget</a>
  ·
  <a href="#7-writing-a-locale">Locale</a>
  ·
  <a href="#8-cover-art">Cover</a>
  ·
  <a href="#9-shipping">Ship</a>
</p>

---

## 0. Something the panel can see in ten minutes

> The panel does **not** read the files in this GitHub repo. It only reads `data/plugins/<plugin-id>/` on your machine. So the workflow is simple: write files in that folder, then refresh the Plugin Workshop.

Start with a minimal theme plugin folder:

```
data/plugins/saki-theme-demo/
  saki-plugin.json
  theme.css
  preview.webp
```

`saki-plugin.json` (the manifest):

```json
{
  "name": "saki-theme-demo",
  "version": "0.1.0",
  "displayName": "My first theme",
  "description": "Accent colour only, to check the load path works.",
  "author": "Your name",
  "type": "theme",
  "icon": "preview.webp",
  "minPanelVersion": "3.5.0",
  "theme": {
    "css": "theme.css",
    "htmlClass": "saki-plugin-theme-demo"
  }
}
```

`theme.css` (the stylesheet):

```css
html.saki-plugin-theme-demo {
  --primary: #1f4bff !important;
  --primary-hover: #1636c9 !important;
  --primary-light: rgba(31, 75, 255, 0.14) !important;
}
```

For `preview.webp`, drop in a **4:3 landscape** image, 1600×1200 is fine. Without it the Workshop card looks bad, but it won't stop the plugin from loading.

Open the Plugin Workshop. You should see "My first theme". Click **Apply theme**. If the accent changes from pink to blue, the manifest, the stylesheet and `htmlClass` all connected. Sidebar, login page and rounded corners are just more rules in the same CSS file.

<div style="background:#fff8e6;border:1px solid #f0d98c;border-left:4px solid #f0b429;border-radius:6px;padding:12px 16px;margin:16px 0;">
  <strong style="color:#9a6700;">About the <code>name</code> field</strong><br>
  <code>name</code> becomes the plugin id and the folder name. Only letters, digits, underscores and hyphens are allowed, and it must start with a letter or digit. <strong>Don't rename it after install</strong> — that creates a brand-new plugin and leaves the old one behind.
</div>

> [!IMPORTANT]
> After changing CSS, images or JSON, the browser may still serve the old cached file. Bump `version` from `0.1.0` to `0.1.1` and refresh. Theme, skin and cover URLs all carry `?v=version`, so bumping the version forces a reload.

## 1. How the panel loads a plugin

The installer walks a GitHub repo looking for `saki-plugin.json`, up to five directories deep. When it finds one, that folder is the plugin root — it will **not** search inside that folder anymore. So don't hide a second manifest inside another plugin's `assets/` folder.

One repo can hold many plugins. This one does:

```
saki-plugins/
  registry.json
  plugins/
    saki-skin-maid/        ← has a manifest here
    saki-theme-geo/
    saki-game-fruit-slice/
    saki-locale-ja/
```

Enter `owner/repo` in the Workshop to install every plugin it finds. To install just one: `owner/repo:plugin-name`, for example `EthanChan050430/saki-plugins:saki-theme-geo`.

After install, each type behaves differently:

<table>
  <tr>
    <th width="120">type you set</th>
    <th>What the panel does</th>
    <th>What you still click</th>
  </tr>
  <tr>
    <td><code>theme</code></td>
    <td>Injects a <code>&lt;link&gt;</code> to your CSS and adds <code>htmlClass</code> to <code>&lt;html&gt;</code></td>
    <td><strong>Apply theme</strong>. Only one at a time. The login page loads it before sign-in.</td>
  </tr>
  <tr>
    <td><code>skin</code></td>
    <td>Replaces images by <code>/assets/</code>-relative path</td>
    <td><strong>Change appearance</strong>. Only one at a time. Missing paths keep the original art.</td>
  </tr>
  <tr>
    <td><code>game</code></td>
    <td>Opens <code>game.entry</code> in a sandboxed iframe</td>
    <td><strong>Play</strong>, or open it from Saki's phone.</td>
  </tr>
  <tr>
    <td><code>widget</code></td>
    <td>Same sandbox iframe</td>
    <td><strong>Enable</strong>. There is no "Apply" button.</td>
  </tr>
  <tr>
    <td><code>locale</code></td>
    <td>Adds your JSON dictionary to the language list</td>
    <td><strong>Enable</strong>, then go to <strong>System settings → Panel language</strong>.</td>
  </tr>
</table>

In short: **themes and skins** are "the one currently in use", while **games, locales and widgets** just stay running while enabled.

Static files from your plugin are served at:

```
/api/plugins/<id>/assets/<path-from-plugin-root>?v=<version>
```

Games, themes and cover art all use this path. A wrong relative path means a 404 and a blank iframe.

## 2. The manifest

The file must be named `saki-plugin.json`, sit at the plugin root, be UTF-8, and contain no comments. If `name`, `type` or `displayName` is missing, the installer skips that folder.

```json
{
  "name": "saki-theme-demo",
  "version": "0.1.0",
  "displayName": "Name on the card",
  "description": "One sentence.",
  "author": "Your name",
  "type": "theme",
  "icon": "preview.webp",
  "homepage": "https://github.com/you/repo",
  "minPanelVersion": "3.5.0"
}
```

`type` must be one of the five below, then you add the matching config block:

<div style="display:flex;flex-wrap:wrap;gap:8px;margin:12px 0;">
  <span style="background:#eef2ff;border:1px solid #c7d2fe;color:#3730a3;padding:4px 12px;border-radius:999px;font-size:13px;font-weight:600;">theme</span>
  <span style="background:#ecfdf5;border:1px solid #a7f3d0;color:#065f46;padding:4px 12px;border-radius:999px;font-size:13px;font-weight:600;">skin</span>
  <span style="background:#fff7ed;border:1px solid #fed7aa;color:#9a3412;padding:4px 12px;border-radius:999px;font-size:13px;font-weight:600;">game</span>
  <span style="background:#f0f9ff;border:1px solid #bae6fd;color:#075985;padding:4px 12px;border-radius:999px;font-size:13px;font-weight:600;">widget</span>
  <span style="background:#fdf4ff;border:1px solid #f5d0fe;color:#86198f;padding:4px 12px;border-radius:999px;font-size:13px;font-weight:600;">locale</span>
</div>

Sample plugins set `minPanelVersion` to `3.5.0`.

The next five sections cover: which files to create, what to put in them, what the panel does after reading them, and how to confirm it works.

## 3. Writing a theme

A theme is not just swapping the pink. You hand over a stylesheet that redraws the sidebar, top bar, buttons, dialogs and login page.

Minimum folder:

```
saki-theme-demo/
  saki-plugin.json
  preview.webp
  theme.css
```

Theme block in the manifest:

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

- `css` is a path relative to the plugin root.
- `htmlClass` must start with `saki-plugin-theme-`, followed by letters, digits, underscores or hyphens. If it's wrong, the class never gets added to `<html>` and none of your selectors match.
- `backgrounds` is optional. If you set it, the panel defines two variables on `<html>`. Wire them into `--app-background-image` in your CSS. If you skip it, the panel's own wallpaper stays.

```css
--plugin-theme-bg-light: url("/api/plugins/.../bg-light.webp");
--plugin-theme-bg-dark: url("/api/plugins/.../bg-dark.webp");
```

### Step 1: Change CSS variables

The panel's built-in liquid-glass styles use a lot of `!important`. Your stylesheet is injected after it, but selectors still need to be specific enough — and your variables should carry `!important` too:

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

At this point the accent colour and rounded corners change, but the sidebar probably still looks like glass. That's expected — many rules target selectors directly and never read a variable.

### Step 2: Override specific UI elements

Override the blocks you can see. Common classes: `.sidebar`, `.topbar-inner`, `.primary-button`, `.modal-panel`, `.login-container`, `.glass-panel`, `.instance-card`.

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

For dark mode use `html.saki-plugin-theme-demo[data-theme="dark"]`. Don't write just `[data-theme="dark"]` — it will fight the panel's own dark rules.

Also, don't clip the character art:

```css
html.saki-plugin-theme-demo .saki-character-art,
html.saki-plugin-theme-demo .saki-character-art img {
  clip-path: none !important;
}
```

### How to know it worked

After applying the theme, check these four spots: **sidebar background, a primary button, any dialog, and the login page after signing out**. If all four changed, you have a real theme, not just a recolour. Disable it and the pink glass should come back intact.

## 4. Writing a skin

A skin isn't just "a folder of drawings". The panel replaces paths like `/assets/expression/happy.webp` with your file — but only if that relative path is listed in the manifest. Unlisted paths keep the original art.

Start with the smallest possible skin: just replace the hover image. The launcher uses `saki_click.webp` when you hover over it.

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

- `assetRoot` is a prefix inside the plugin. An empty string means `saki_click.webp` sits at the plugin root and maps to `/assets/saki_click.webp`. If you put everything under `art/`, set `"assetRoot": "art"` and store `art/saki_click.webp`.

Copy the plugin into `data/plugins/saki-skin-demo/`, refresh the Workshop, click **Change appearance**. Hover over the Saki launcher and you should see your image. If not, check three things: did you list the path in `files`, does the path match `/assets/`, and did you bump `version`?

> [!WARNING]
> Don't use `skin.mapping` to point a missing pose at a different drawing. Leave gaps as the original. Also, the hover image is `saki_click.webp`, not `pet/hover.webp` — easy to mix up.

### Adding more expressions

Stills go in `expression/<name>.webp`. A six-frame cycle must be:

```
expression/anim/<name>_f1.webp
expression/anim/<name>_f2.webp
...
expression/anim/<name>_f6.webp
```

Note that the frames for `think` are `think_f1`, not `thinking_f1`. Every file you add must be listed in `skin.files`. The panel only covers paths from that list — an unlisted path is simply not shipped.

<details>
<summary><strong>All paths the panel reads (click to expand)</strong></summary>

**Root**: `head.webp`, `sakiicon.webp`, `sakiicon2.webp`, `saki_click.webp`, `tiebian.webp`, `hang.webp`, `lie.webp`, `saki_files.webp`, `shuru.webp`, `shuru_sit.webp`.

**`expression/` stills**: `normal`, `think`, `worry`, `happy`, `shy`, `wink`, `cry`, `surprised`, `pout`, `OK`, `sorry`, `sleepy`, `eating`, `working`, `reading`, `checkfiles`, `gaming`, `listen`, `waiting`, `writing`, `terminal`, `search`, `diagnose`, `rollback`, `blocked`, `singing`, `upset`, `middlefinger`, `pickup1`, `pickup2`, `speaking1`, `speaking2`, `empty_healthy`, `empty_instances`, `empty_tasks`, `empty_logs`, `daemon_offline`, `page_404`.

**Six-frame cycles**: `happy`, `shy`, `wink`, `surprised`, `pout`, `sorry`, `cry`, `eating`, `sleepy`, `OK`, `think`, `worry`, `working`, `reading`, `checkfiles`, `upset`, `gaming`, `listen`, `waiting`, `writing`, `terminal`, `search`, `diagnose`, `rollback`, `blocked`, `middlefinger`, `singing`.

**`pet/`**: `idle`, `hover`, `sit`, `sleep`, `lie`, `hang`, `look`, `run`, `roll`, `walk`, `walk2`, `walk3`, `climb`, `fall`, `pickup`, `happy`, `shy`, `poke`, `yawn`, `pout`, `blink`, `drink`, `doctor`, `eat`, `bath`. `walk`, `climb`, `bath`, `doctor` and `eat` also have `_f1`–`_f6` frames.

</details>

Use webp format and clean up transparent edges. Only one skin can be active at a time.

## 5. Writing a game

A game is just an HTML page that opens on its own. The panel puts it in an iframe with `allow-scripts allow-pointer-lock` and **without** `allow-same-origin`. That means you can't touch the parent page, cookies, or the parent's `localStorage`. If you need to store a score, keep it in memory — it disappears on refresh.

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
    "title": "My game",
    "width": 720,
    "height": 540
  }
}
```

- `entry` is relative to the plugin root.
- `width` / `height` set the initial window size, not the canvas limit. Refreshing in the Workshop reloads the iframe.

Asset paths in `index.html` must be relative to that HTML file itself:

```html
<!DOCTYPE html>
<html lang="en">
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

<div style="background:#fef2f2;border:1px solid #fecaca;border-left:4px solid #ef4444;border-radius:6px;padding:12px 16px;margin:16px 0;">
  <strong style="color:#991b1b;">Don't get the path wrong</strong><br>
  Don't write <code>/assets/player.webp</code> — that's the panel's own directory, not your plugin. If you get a white screen, open these two URLs directly in the browser dev tools:
  <pre style="background:#1e1e1e;color:#e0e0e0;padding:10px;border-radius:4px;margin:8px 0;overflow-x:auto;font-size:13px;">/api/plugins/saki-game-demo/assets/index.html
/api/plugins/saki-game-demo/assets/assets/player.webp</pre>
  A 404 means the path is wrong.
</div>

Enabled games also show up in Saki's phone list, using the `icon` from the manifest.

Don't call login APIs on other origins. Request pointer lock only after the user clicks the canvas.

## 6. Writing a widget

Widgets use the same iframe sandbox as games. Just change the type to `widget` in the manifest:

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

- `mountPoint` can be `dashboard`, `sidebar` or `settings`.
- `height` is the card body height; when expanded the panel estimates about 1.5×.
- The Workshop only has **Enable**, no "Apply" button.
- Asset path rules are the same as games.

There's no widget sample in this repo yet — just follow these rules.

## 7. Writing a locale

A locale is not "translate the README". It adds an option to the panel's language list. Three languages ship by default: Simplified Chinese, Traditional Chinese, English. After you enable your pack, **System settings → Panel language** (same spot on the login page) will show your `label`.

Ship a tiny pack first — just a handful of keys — to confirm the dropdown and `t()` work, then fill in the rest.

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
  "description": "French UI.",
  "author": "Your name",
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

- `language` uses BCP 47 format and becomes the dropdown value.
- `translations` is the path to a JSON file inside the plugin.

`fr.json` is a flat object — no nesting:

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

There are two kinds of keys, and it's safest to write both:

1. i18n keys used by `t("nav.dashboard")`. The full list is the `zh-CN` object in `apps/web/src/i18n/translations.ts`. Frequent prefixes: `common`, `nav`, `auth`, `account`, `users`, `roles`, `settings`, `view`, `context`.
2. Chinese phrases still replaced directly in the DOM, like `扩展工坊`. The key must match the source exactly, punctuation included.

Missing keys fall back to Simplified Chinese — no need to translate everything at once.

To switch languages, follow all four steps or the UI won't change:

1. **Enable** the pack in the Workshop.
2. Open **System settings**, find **Panel language**.
3. Choose `fr-FR` (or whatever you put in `language`).
4. Before disabling the pack, switch away if that language is selected. Otherwise the option vanishes while the UI is still using a dictionary that no longer exists.

Don't install two packs for the same `language` — the later one overwrites the earlier. The Japanese sample at `plugins/saki-locale-ja/ja.json` shows how dense a dictionary can get; you don't have to copy its structure.

## 8. Cover art

The `icon` image is drawn with `object-fit: cover` on the Workshop card. Make it a **4:3 landscape** webp, 1600×1200 if you can. Keep the subject centred. A cut-out character on a black background becomes an ugly slice in the card.

<div style="background:#eef2ff;border:1px solid #c7d2fe;border-left:4px solid #6366f1;border-radius:6px;padding:12px 16px;margin:16px 0;">
  <strong style="color:#3730a3;">Tip</strong><br>
  Don't paint the plugin name onto the image — it's already shown via <code>displayName</code>.
</div>

## 9. Shipping

Once it works locally, two routes are available.

**Your own GitHub repository.** Paste the repo URL into the Workshop. A valid manifest is all it takes. Several plugins in one repo are fine.

**Featured in this repository.** Fork this repo, branch from `master`, drop a self-contained plugin in `plugins/<name>/`. Then add a row to `registry.json` whose `name` matches the manifest:

```json
{
  "name": "saki-theme-demo",
  "displayName": "My first theme",
  "description": "One sentence.",
  "author": "Your name",
  "type": "theme",
  "version": "0.1.0",
  "repo": "EthanChan050430/saki-plugins",
  "ref": "master",
  "icon": "preview.webp",
  "featured": true
}
```

After merge, keep `repo` pointing here. In the pull request, state the type, which pages you opened, and what you expected to see. Don't include editor junk or uncompressed originals. Themes shouldn't clip the face. Skins shouldn't substitute one pose for another. Bump `version` when files change — the panel watches the commit or that field.

When you're stuck, open the finished sample of the matching type under `plugins/` and check how its manifest and files line up with the rules above.
