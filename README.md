# 雨云自动签到

基于 Selenium + ICR 验证码识别的雨云每日自动签到工具，支持多账号、Cookie 缓存、代理、并发执行，可在 GitHub Actions 中定时运行。

## 功能特性

- ✅ 自动完成雨云账户登录（Selenium 模拟浏览器）
- ✅ ICR 验证码识别（旋转分析 + 模板匹配）
- ✅ 浏览器指纹伪装（WebGL / Canvas / Audio / UA 随机化）
- ✅ Cookie 缓存，支持 7 天免密登录
- ✅ 多账号并发签到，随机延时避免风控
- ✅ 失败自动重试，僵尸进程自动清理
- ✅ GitHub Actions 定时触发 + 手动触发
- ✅ 支持 HTTP/SOCKS5 代理
- ✅ 7 种通知推送（Push+ / SMTP / Bark / 钉钉 / 飞书 / Telegram / Server 酱）

## 环境要求

- Python 3.9+
- Google Chrome 浏览器（CI 环境自动安装）

## 安装

```bash
git clone https://github.com/fok777/yuqd.git
cd yuqd
pip install -r requirements.txt
```

## 配置

### 方式一：环境变量

```bash
export RAINYUN_USER="your_username"
export RAINYUN_PASS="your_password"
python rainyun.py
```

多账号（每行一个，用户名和密码数量需匹配）：

```bash
export RAINYUN_USER="user1\nuser2\nuser3"
export RAINYUN_PASS="pass1\npass2\npass3"
```

### 方式二：.env 文件（推荐）

```env
RAINYUN_USER=your_username
RAINYUN_PASS=your_password
DEBUG=false
HEADLESS=true
MAX_WORKERS=2
MAX_RETRIES=1
```

### 可选：代理配置

```env
PROXY_SERVER=p.webshare.io:80
PROXY_USER=your_proxy_user
PROXY_PASS=your_proxy_password
```

## GitHub Actions 自动签到

1. Fork 或克隆本仓库
2. 进入 `Settings` → `Secrets and variables` → `Actions`
3. 添加必填密钥：

| Secret | 说明 |
|--------|------|
| `RAINYUN_USER` | 雨云用户名（多行支持多账号） |
| `RAINYUN_PASS` | 雨云密码（多行需与用户名数量匹配） |

4. 可选通知推送密钥：

| Secret | 推送方式 |
|--------|----------|
| `PUSH_PLUS_TOKEN` | Push+ 微信推送 |
| `SMTP_SERVER` / `SMTP_EMAIL` / `SMTP_PASSWORD` | 邮件推送 |
| `BARK_PUSH` | Bark iOS 推送 |
| `DD_BOT_TOKEN` / `DD_BOT_SECRET` | 钉钉机器人 |
| `FSKEY` | 飞书 Webhook |
| `TG_BOT_TOKEN` / `TG_USER_ID` | Telegram |
| `PUSH_KEY` | Server 酱 |

5. 工作流每天 UTC 2:00（北京时间 10:00）自动运行，也可手动触发。

## 配置项说明

| 变量 | 默认值 | 说明 |
|------|--------|------|
| `RAINYUN_USER` | - | 用户名（必填） |
| `RAINYUN_PASS` | - | 密码（必填） |
| `HEADLESS` | `true` | 无头模式 |
| `DEBUG` | `false` | 调试模式 |
| `MAX_WORKERS` | `2` | 最大并发数 |
| `MAX_RETRIES` | `1` | 最大重试次数 |
| `MAX_DELAY` | `15` | 账号间最大延时（秒） |
| `TIMEOUT` | `15000` | 元素等待超时（毫秒） |

## 常见问题

**验证码识别失败？**
ICR 模块使用旋转分析和模板匹配，识别率较高。脚本会自动重试，多次尝试后通常能通过。

**ChromeDriver 版本不匹配？**
CI 环境会自动从 Chrome for Testing 下载匹配的 ChromeDriver。本地开发可使用 `webdriver-manager` 自动管理。

**多账号端口冲突？**
项目已实现线程锁机制，Chrome 实例按顺序初始化，避免端口冲突。

## 许可证

GPL v3.0 — 查看 [LICENSE](LICENSE) 文件了解详情。

## 免责声明

- 本工具仅用于学习和个人使用
- 使用本工具应遵守雨云官方的用户协议和相关规定
- 作者不对因使用本工具可能产生的任何后果负责
