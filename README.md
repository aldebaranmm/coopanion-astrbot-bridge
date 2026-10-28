# Cortico × AstrBot 桥接

把 **Coopanion 桌宠**和 **AstrBot** 接起来。装好之后：

- 桌宠能查 AstrBot 记住的喜好习惯
- AstrBot 的主动消息会变成桌宠的气泡说给你听
- 桌宠能自己刷 B站/抖音，看完把内容写进记忆
- 可以在 QQ 上远程让桌宠去刷，也能查它记了什么

---

## 三个组件，各装各的

整套由三部分组成，**服务端装一个、客户端装两个**，缺一不可。

| 组件 | 装到哪 | 作用 | 下载 |
|---|---|---|---|
| **桥接插件**（服务端） | AstrBot | 身份绑定、机器配对、主动消息双发、记忆检索、对话回传、远程推送 | [astrbot_plugin_coop_bridge-v0.9.2.zip](https://github.com/aldebaranmm/astrbot_plugin_coop_bridge/releases/download/v0.9.2/astrbot_plugin_coop_bridge-v0.9.2.zip) |
| **cortico-world-astrbot** | Coopanion 桌宠 | 刷视频主体：开浏览器、点开看、按你的收藏挑片、读收藏 | [cortico-world-astrbot-0.9.5.zip](https://github.com/aldebaranmm/cortico-world-astrbot/releases/download/v0.9.5/cortico-world-astrbot-0.9.5.zip) |
| **cortico-world-web** | Coopanion 桌宠 | 网页/接口取数（`web_fetch`、`web_bilibili`） | [cortico-world-web-0.1.0.zip](https://github.com/aldebaranmm/cortico-world-web/releases/download/v0.1.0/cortico-world-web-0.1.0.zip) |

> 版本号会更新，上面是写这份文档时的最新版。
> 要找更新的版本，去各自仓库的 Releases 页。

---

## 还需要：AstrBot 侧再装两个插件

上面三个只是"桥"，**桥要有东西可查、有话可推**才有用。服务端还需要：

| 插件 | 必需？ | 干什么 | 不装会怎样 |
|---|---|---|---|
| [astrbot_plugin_proactive_chat](https://github.com/Pancakes-Labs/astrbot_plugin_proactive_chat) | **强烈建议** | 让机器人主动找你说话 | 主动消息只发到 QQ，**桌宠不会冒泡** |
| [astrbot_plugin_livingmemory](https://github.com/lxfight-s-Astrbot-Plugins/astrbot_plugin_livingmemory) | **至少装一个** | 事实/主题记忆（主力） | 桌宠查记忆只会返回空 |
| [astrbot_plugin_angel_memory](https://github.com/kawayiYokami/astrbot_plugin_angel_memory) | 可选 | 判断类记忆 | — |
| [astrbot_plugin_self_learning](https://github.com/NickCharlie/astrbot_plugin_self_learning) | 可选 | 个人记忆 + 黑话词典 | — |

**记忆源一个都不装的话，插件不报错，但 `recall` 永远查不到东西** —— 这是最容易让人以为"装坏了"的坑。

装完 proactive_chat 还要配两处，否则桌宠仍然不冒泡：
把它加进私聊白名单 `friend_settings.session_list`，并把客户端的 `wakeMode` 设成 `flush`。

作者实机版本：proactive_chat v1.2.6 / livingmemory 2.8.2 / angel_memory 2.0.4 / self_learning 4.4.3

---

## 快速开始（三步）

**1. 服务端** —— 把桥接插件解压到 AstrBot 的插件目录，**重启 AstrBot 实例**才生效。

**2. 客户端** —— 把两个扩展包解压到 Coopanion 的扩展目录，**重启 Coopanion**。

**3. 配对** —— 在 QQ 私聊机器人发「**配对**」，拿到 8 位配对码，填进 Coopanion 的「AstrBot → 桥接设置」里。

配好之后，先在气泡里跟桌宠说句话验证一次（最稳），确认它能查到记忆、能开口。

**完整步骤、路径说明、排错清单见 → [安装使用说明](docs/安装使用说明.md)**

---

## 源码仓库

三个组件各自独立（服务端插件必须单独成库，AstrBot 的更新机制要求仓库根目录就是插件本身）：

- https://github.com/aldebaranmm/astrbot_plugin_coop_bridge
- https://github.com/aldebaranmm/cortico-world-astrbot
- https://github.com/aldebaranmm/cortico-world-web

这是个**索引仓库**，只放文档，不含代码。

---

## 几个容易踩的点

- **AstrBot 侧改代码/装插件后必须重启实例**，仪表盘的重载对新代码无效
- **主动消息不开口**：检查 `wakeMode` 是不是 `flush`。老版本默认 `piggyback`，那种模式下要等你先跟桌宠说话才会捎带出来
- **跨机器连接**：默认只监听 `127.0.0.1:6190`，别的机器要连就用 frp 穿透，别直接暴露端口
- **更新地址**：`worlds.astrbot.updateUrl` 默认留空 = 这台机器不检查更新。想开自动更新就填一个目录地址（`http://`、`https://` 都行），里面放 `version.json` + 安装包

---

## 许可与商用限制

作者：**Kaiser**

### ✅ 可以自由使用

- 个人自用、学习研究
- 非营利的修改与二次分发（须保留署名和本声明）

### ❌ 未经书面授权，禁止任何形式的商业使用

包括但不限于：

- 打包进收费产品或服务出售
- 用于经营性业务（付费机器人、商业客服、企业内部系统等）
- 二次分发收费，或去除/篡改署名后转售
- 作为付费部署、代搭建服务的一部分提供

### 商业授权

确有商业使用需求的，请联系作者取得 **书面授权**。

### 📮 联系方式

有问题、建议、bug 反馈，或需要商业授权，都欢迎联系作者：

- **QQ 邮箱：2760546027@qq.com**

（作者即本项目的 QQ 机器人使用者，反馈时说明一下你用的是哪个组件、版本号，会更快定位。）

### 法律声明

对未经授权的商业使用，作者 **保留一切追诉权利**，
包括但不限于要求立即停止侵害、消除影响、赔偿损失，以及提起诉讼。

---

代码许可以仓库根目录的 [LICENSE](LICENSE) 文件为准：**PolyForm Noncommercial License 1.0.0**
（源码公开可读、可改、可分发，但**仅限非商业用途**）。
商业使用须另行取得作者的书面许可，联系方式见上。
