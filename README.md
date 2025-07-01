# Telegram 关键词监听机器人

一个基于 Go 语言开发的 Telegram 关键词监听系统，支持多账号管理、关键词监听、自动通知/私信、账号健康检测等功能。
本项目完全免费，一分钱不收取，收费的全是骗子
---

## 功能特性
- 多账号支持（监听账号、私信账号，支持批量导入 session）
- 关键词监听，自动通知群组/用户
- 关键词触发后自动私信用户，失败自动切换账号
- 账号健康检测与自动告警
- SQLite 数据持久化
- YAML 配置，模板自定义

---


## 其他开源项目
- 双向机器人 https://github.com/gooso1/GoSearch_two_way
- 群组管理机器人 https://github.com/gooso1/GoSearch_group_mang

## 收费项目
- 索引机器人 https://github.com/gooso1/GoSearch_Keyword_listen

## 联系我们
- telegram @GoSearch

## 赞助我们
- TRC20 TAwZhEJkQqoaHqz6e2rPsPd2SCMNKx2C8s

## 目录结构
```
telegram-keyword-watcher/
├── data/                    # 数据库存放
├── sessions/                # session 文件
├── logs/                    # 日志
```

---

## 依赖环境
- Go 1.21+
- SQLite3
- [gotd/td](https://github.com/gotd/td) (Telegram MTProto 客户端)
- [go-telegram-bot-api/telegram-bot-api](https://github.com/go-telegram-bot-api/telegram-bot-api) (Bot API)
- [mattn/go-sqlite3](https://github.com/mattn/go-sqlite3)
- [gopkg.in/yaml.v3](https://pkg.go.dev/gopkg.in/yaml.v3)

---

## 快速开始

### 1. 克隆项目
```bash
git clone https://github.com/gooso1/GoSearch_Keyword_listen.git
cd GoSearch_Keyword_listen
```

### 2. 配置 config.yaml
- 填写管理员 Telegram ID、Bot Token、Client API ID/Hash、通知目标等。
- 示例：
```yaml
admin:
  telegram_ids: [123456789]
bot_token: "YOUR_BOT_TOKEN"
clients:
  - api_id: 123456
    api_hash: "your_api_hash"
notify_targets:
  groups: ["-1001234567890", "@groupusername"]
  users: [123456789]
pm_template: "🔔 检测到关键词：{keyword}\n👤 用户名：@{username}\n🧑 昵称：{nickname}\n📝 内容：{message}"
database:
  path: "./data/bot.db"
logging:
  level: "info"
  file: "./logs/bot.log"
```


### 3. 启动项目
```bash
./GoSearch_Keyword_listen
```

---

## 使用教程

### 管理机器人命令（在 Telegram 聊天窗口对你的 Bot 发送）

| 命令 | 说明 | 格式 |
|------|------|------|
| `/start` | 显示帮助信息 | - |
| `/addaccount` | 上传 session 文件（支持单个 .session 或 zip 批量导入） | 上传文件 |
| `/addgroup` | 添加监听群组 | `/addgroup <群组ID/用户名/邀请链接> <群组名称>` |
| `/addkeyword` | 添加关键词 | `/addkeyword <关键词> <群组ID>` |
| `/listaccounts` | 查看账号列表 | - |
| `/listgroups` | 查看群组列表 | - |
| `/listkeywords` | 查看关键词列表 | - |
| `/checkaccounts` | 检查账号存活状态 | - |

#### 示例
- 添加群组：
  ```
  /addgroup -1001234567890 测试群组
  ```
- 添加关键词：
  ```
  /addkeyword 测试  -1001234567890
  ```
- 上传 session 文件：
  发送 `/addaccount`，然后上传 .session 或 .zip 文件

### 监听与通知
- 监听账号会自动监听你添加的群组消息。
- 检测到关键词后，会按 `pm_template` 模板自动通知 `notify_targets` 配置的群组和用户。
- 如果配置了私信账号，检测到关键词后会自动私信用户。

### 账号健康与告警
- 账号存活数低于 3 个时，系统会自动告警管理员。
- 私信账号被限制时会自动切换并告警。

### 数据与日志
- 所有数据保存在 `./data/bot.db`（SQLite）
- 日志保存在 `./logs/bot.log`

---

## 配置说明
- `admin.telegram_ids`：管理员 Telegram ID 列表
- `bot_token`：管理机器人 Bot Token
- `clients`：Telegram Client 的 api_id 和 api_hash 列表
- `notify_targets.groups`：通知群组 ID 或用户名
- `notify_targets.users`：通知用户 ID
- `pm_template`：通知模板，支持 `{keyword}`、`{username}`、`{nickname}`、`{message}` 占位符
- `database.path`：数据库文件路径
- `logging.level`：日志级别
- `logging.file`：日志文件路径

---

## 常见问题

### 1. session 文件如何获取？
用 [gotd/td](https://github.com/gotd/td) 或 Telethon 等工具登录 Telegram 账号生成 .session 文件。

### 2. Bot Token 如何获取？
通过 Telegram 的 @BotFather 创建机器人获得。

### 3. API ID/Hash 如何获取？
在 [my.telegram.org](https://my.telegram.org) 注册开发者应用获得。

### 4. 监听账号/私信账号如何区分？
上传 session 时可在数据库中手动修改账号类型，或扩展管理命令。

### 5. 支持哪些关键词匹配？
关键词匹配为不区分大小写的包含关系。

---

## 贡献方式

欢迎提交 Issue 或 Pull Request 参与项目改进！

---

## License

MIT License 