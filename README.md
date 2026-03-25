# 🎮 FreezeHost 自动续期助手

基于 Playwright + GitHub Actions 优化的 FreezeHost 免费服务器自动续期脚本。本项目在原版的基础上，大量完善了并发逻辑与防风控机制。

## ✨ 核心优化项

- 🛡️ **Token 注入登录**：抛弃传统账号密码，直接通过 Token 完美绕过 Discord 多端登录拦截与人机验证 (hCaptcha)。
- 🔄 **多账号隔离支持**：支持配置多个 Discord Token，全自动为每个账号分配独立的沙盒环境以并发执行，互不干扰。
- 🤖 **智能账号冠名**：全新加入无感抓取器！无需您手动配置冗长的备注，系统自然访问官方接口帮您自动捕捉 Discord 真实昵称或注册邮箱名。
- 🖥️ **全量服务器遍历**：自动彻底巡视当前账号下的所有服务器节点状态，而不是只读第一台。
- 💰 **可用余额抓取**：智能精准提取当前网页金币储备（AVAILABLE BALANCE），直接附在续期通知中下发展示。
- ⚡ **环境缓存提速**：引入 Actions 依赖缓存机制与失败自动重试 (Retry) 配置，兼顾极速运行与超高稳定性。

## ⚙️ 配置步骤

### 1️⃣ Fork 此仓库到您的 GitHub

### 2️⃣ 添加 Secrets
进入仓库 → **Settings** → **Secrets and variables** → **Actions** → **New repository secret**：

| 🔑 Secret 名称 | 📝 格式 | ✅ 必填 |
|---|---|---|
| `DISCORD_TOKEN` | `你的Token (见下方多账号命名格式配置)` | ✅ |
| `TG_BOT` | `chat_id,bot_token` | ✅ |
| `GOST_PROXY` | `socks5://host:port` 或 `http://host:port` | 可选 |

> **💡 如何配置多个账号并显示名称？**
> 脚本目前默认会**智能全自动抓取您原本的 Discord 真实昵称**。但如果您偏想自己起个个性化名字覆盖它，可以在每个 Token 前面加上冒号 `:` 或井号 `#`，多个账号依然用英文逗号 `,` 隔开。
> **示范格式**：`lyaurora:TokenA, 朋友号#TokenB, TokenC` （其中的 `TokenC` 没写死备注名，系统就会顺滑地自动帮它读出真实名字拉起战报）

> **💡 如何获取 Discord Token？**
> 1. 在电脑浏览器中登录 [Discord 网页版](https://discord.com/app)。
> 2. 按 `F12` 打开开发者工具，切换到 **Network (网络)** 面板。
> 3. **关键步骤**：在 Network 面板的过滤选项卡中，选中 **"Fetch/XHR"**。
> 4. 保持面板打开，回到 Discord 页面并**随便点击左侧的其他服务器或频道**。
> 5. 这时 Network 列表会出现一些名为 `science`、`messages` 或 `typing` 的请求，点击其中一个。
> 6. 在右侧弹出的 **Headers (标头)** 标签页里向下滑，找到 **Request Headers (请求标头)** 区域。
> 7. 在里面寻找名叫 `Authorization` 的字段，它后面的那一串复杂字符就是您的 Token！直接复制下来即可。

### 3️⃣ 启用 Actions
进入 **Actions** 标签页，点击绿色按钮 **I understand my workflows, go ahead and enable them**。

### 4️⃣ 手动触发测试
进入 **Actions** → 左侧点击 **🎮 FreezeHost 自动续期** → 右侧点击 **Run workflow**。

## 🕐 运行机制与频次

- **智能续期阈值**：FreezeHost 官方规则为少于 7 天可续期并重置至 14 天。脚本会在识别到剩余时间 `<=` 7 天时才执行续期动作，避免徒劳无功。
- **默认执行频率**：配置为 **每 3 天**（即每月 1, 4, 7... 号）的北京时间 16:00 自动运行一次，完美契合机制并防止频繁刷新触发官方风控。
- *(如需修改频率，请前往 `.github/workflows/freeze.yml` 的 `cron` 表达式中调整)*

## 📊 Telegram 效果展示

```text
📝 所有账号处理完毕:

👤 lyaurora | 💰 2,078
  📦 Node 1: ⏸️ 无需 (剩 12.5 天)
  📦 Node 2: ✅ 续期成功

👤 朋友的名字 | 💰 345
  📦 Node 1: ⚠️ 余额不足
```
