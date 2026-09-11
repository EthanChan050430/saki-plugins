# 🌸 Saki Panel 插件工坊 (Community Plugins) 🧩

> 欢迎光临！这里是 **Saki Panel** 的官方插件示范库与社区集市。
> 觉得默认的控制台太严肃了？想给趴在窗口角落的 Saki 换一套新衣服？或者在服务器编译代码的间隙摸个鱼？
> 没问题，把你的创意变成现实吧！✨

---

## 🌟 什么是 Saki Panel 插件？

在 3.5 版本中，Saki Panel 迎来了一次彻底的瘦身与模块化重构。我们不希望面板本身变得臃肿不堪，因此将 **主题风格**、**Saki 皮肤**、**小游戏**、**实用小工具** 等全部解耦成了独立插件！

每个插件就是一个独立的小世界，具备以下几个最酷的特性：

1. **人人都能写**：零脚手架、不用配 Webpack/Vite，只要会写基本的 HTML/CSS 或画几张画，就能做出属于你的插件！
2. **极速热加载**：在面板里一键启用/切换，主题立刻变幻、皮肤瞬间换好，甚至不需要按 F5 刷新！
3. **🇨🇳 国内网络超速直达**：针对国内很多服务器连接 GitHub 偶尔抽风的问题，Saki Panel 的扩展工坊**内置了 GitHub 加速镜像选择器**（GHFast、KKGitHub、GHProxy 等），支持测速与一键加速拉取，再也不用担心因为网络超时安装失败了！

---

## 🎁 本仓库精选示范插件

| 插件名称 | 类型 | 描述 | 入口 |
| :--- | :--- | :--- | :--- |
| **[🌸 樱花梦境 (Sakura Dream)](./plugins/saki-theme-sakura-dream)** | 🎨 主题 | 柔和唯美的樱花渐变色，带毛玻璃与呼吸发光，深夜运维也不刺眼 | `theme.css` |
| **[👗 赛博女仆 (Cyber Maid)](./plugins/saki-skin-cyber-maid)** | 👗 皮肤 | 穿上蓝白科技风女仆装的 Saki，全套专属表情立绘与动态桌宠 | `expressions/`, `pet/` |
| **[🧰 极客运维工具箱 (Toolbox)](./plugins/saki-widget-server-toolbox)** | 🧰 工具 | 实用微应用：实时网络 Ping 测速、随手记事便签、Linux 命令速查速拷 | `index.html` |
| **[🍰 甜点大作战 (Dessert Catcher)](./plugins/saki-game-dessert-catcher)** | 🎮 游戏 | 操控 Saki 接住掉落的马卡龙与草莓蛋糕，8-bit 街机音效与连击手感拉满！ | `index.html` |

---

## 🚀 怎么在 Saki Panel 中安装插件？

### 方法一：扩展工坊一键安装（最推荐）
1. 登录你的 Saki Panel 管理后台。
2. 点击左侧导航栏的 **「扩展工坊 (Plugins)」**。
3. 如果你在国内，可以点击顶部的 **「⚡ 测速」** 按钮，选择延迟最低的 GitHub 加速线路（推荐 `GHFast 镜像`）。
4. 切换到 **「社区精选工坊」** 标签，找到喜欢的插件，直接点击 **「一键安装」**！

### 方法二：从任意 GitHub 仓库安装
1. 在扩展工坊右上角点击 **「+ 从 GitHub 安装」**。
2. 输入插件所在的 GitHub 仓库地址，例如：
   ```text
   EthanChan050430/saki-plugins
   ```
3. 确认加速镜像，点击 **「确认安装」** 即可自动下载并解包！

---

## 🛠️ 5 分钟上手：写一个属于你的插件！

插件开发遵循 **「约定优于配置」** 原则。无论哪种类型的插件，核心都只需要根目录下放一个 `saki-plugin.json` 清单文件。

### 1. 清单文件范例 (`saki-plugin.json`)

```json
{
  "name": "my-cute-theme",
  "version": "1.0.0",
  "displayName": "✨ 我的魔法主题",
  "description": "这是我给 Saki Panel 做的第一个主题！",
  "author": "你的名字",
  "type": "theme",
  "theme": {
    "css": "theme.css"
  }
}
```

### 2. 编写主题插件 (`type: "theme"`)
只需写一个 `theme.css` 覆盖 Saki Panel 的 CSS 变量即可：

```css
/* 修改全局主题色与背景卡片微光 */
:root {
  --accent: #ff69b4;
  --surface-panel: rgba(255, 230, 240, 0.08);
  --border-subtle: rgba(255, 105, 180, 0.2);
}
```

### 3. 制作皮肤插件 (`type: "skin"`)
把你的画作放在插件文件夹里：
- `expressions/`: 放置常用表情，如 `normal.webp`, `happy.webp`, `thinking.webp`, `shy.webp`, `pout.webp`, `gaming.webp` 等。
- `pet/`: 放置桌面小宠物的状态图，如 `sit.webp`, `walk.webp`, `lie.webp`, `sleep.webp`。

### 4. 制作小游戏或小工具 (`type: "game"` 或 `"widget"`)
- 只需要一个标准的前端网页 `index.html`（可引入本地的 CSS / JS）。
- 游戏会运行在沙箱容器中，小巧安全，也可以被 Saki 的虚拟手机直接调用！

---

## 🤝 欢迎贡献！

你有写好的有趣插件想分享给大家吗？
1. Fork 本仓库。
2. 在 `plugins/` 目录下创建你的插件文件夹。
3. 在根目录的 `registry.json` 中添加你的插件信息。
4. 提交 Pull Request！审核通过后，所有 Saki Panel 用户都能在商店里看到你的大作啦！🎉

---

## 📜 开源协议

本项目采用 MIT 协议开源。尽情发挥你的创造力吧！💖
