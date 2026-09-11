# Saki Panel 示例插件

官方示范仓库，目前收录三个插件：

| 插件 | 类型 | 说明 |
| :--- | :--- | :--- |
| [女仆装](./plugins/saki-skin-maid) | 形象 | 全站 Saki 立绘 / 桌宠换上法式女仆装 |
| [简约几何](./plugins/saki-theme-geo) | 主题 | 切角几何按钮与模态框 |
| [切水果](./plugins/saki-game-fruit-slice) | 游戏 | 滑动切开水果的街机小游戏 |

## 安装

1. 打开 Saki Panel → **扩展工坊**
2. 需要加速时先测速并选择 GitHub 镜像
3. 在 **社区精选** 里一键安装，或填写 `EthanChan050430/saki-plugins`

仓库为 monorepo：每个子目录一份 `saki-plugin.json`。安装器会扫描并安装全部插件，也可以用 `owner/repo:插件名` 只装其中一个。

## 开发要点

每个插件根目录需要 `saki-plugin.json`。

**主题** `type: "theme"`：提供 `theme.css`，可设 `theme.htmlClass`（必须以 `saki-plugin-theme-` 开头），用 `--radius-control`、`--radius-modal` 改按钮和模态框外形。

**形象** `type: "skin"`：用 `skin.assetRoot` 按面板 `/assets/` 同样的目录结构投放 webp。面板会读取插件文件清单，自动覆盖表情、动画帧和桌宠。

**游戏** `type: "game"`：一个 `index.html`，在沙箱 iframe 中运行，也会出现在 Saki 手机里。
