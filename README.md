# yuqd

基于 Selenium 和 ICR（Image Captcha Recognition）的雨云自动签到工具，通过模拟浏览器操作和高级验证码识别，实现雨云账户的自动每日签到以赚取积分。

## 功能特性

- ✅ 自动完成雨云账户登录
- ✅ 使用 ICR 模块进行验证码自动识别（旋转分析+模板匹配）
- ✅ 支持自定义随机延时（5-20秒），避免被系统识别为自动化脚本
- ✅ 支持在本地环境和 GitHub Actions 中运行
- ✅ 集成 webdriver-manager 自动匹配 ChromeDriver
- ✅ 详细的日志记录，便于排查问题
- ✅ 支持多账户签到，并发处理
- ✅ 随机浏览器指纹（User-Agent、分辨率、语言、时区）
- ✅ Cookie 缓存功能，支持免密登录
- ✅ 支持 7 种通知推送方式（Push+、SMTP、Bark、钉钉、飞书、Telegram、Server酱）

## 技术栈

- Python 3.9+
- Selenium WebDriver 4.27+
- ICR 验证码识别模块（旋转分析+模板匹配）
- OpenCV 图像处理
- Google Chrome 浏览器

## 安装步骤

### 1. 环境要求

- Python 3.9 或更高版本
- Google Chrome 浏览器

### 2. 克隆项目

```bash
git clone https://github.com/fok777/yuqd.git
cd yuqd
```

### 3. 安装依赖

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

## 使用方法

### 本地运行

#### 通过环境变量配置

```bash
# Linux/macOS
export RAINYUN_USER="your_username"
export RAINYUN_PASS="your_password"

# 运行脚本
python rainyun.py
```

#### 多账户配置

支持多行格式，每行一个用户名/密码，数量需匹配：

```bash
export RAINYUN_USER="user1\nuser2\nuser3"
export RAINYUN_PASS="pass1\npass2\npass3"
```

#### 通过 .env 文件配置

创建 `.env` 文件：

```env
RAINYUN_USER=your_username
RAINYUN_PASS=your_password
DEBUG=false
HEADLESS=false
AUTO_UPDATE=false
MAX_WORKERS=2
MAX_RETRIES=1
```

### 使用 GitHub Actions 自动签到

1. Fork 本仓库
2. 进入仓库的 `Settings` > `Secrets and variables` > `Actions`
3. 添加以下密钥：

| Secret 名称 | 说明 | 必需 |
|-------------|------|------|
| RAINYUN_USER | 雨云用户名（支持多行） | ✅ |
| RAINYUN_PASS | 雨云密码（支持多行） | ✅ |

4. 可选的通知推送配置：

| Secret 名称 | 说明 |
|-------------|------|
| PUSH_PLUS_TOKEN | Push+ 用户令牌 |
| SMTP_SERVER | SMTP 服务器地址 |
| SMTP_EMAIL | 邮箱地址 |
| SMTP_PASSWORD | 邮箱密码 |
| BARK_PUSH | Bark 设备码或完整 URL |
| DD_BOT_SECRET | 钉钉机器人密钥 |
| DD_BOT_TOKEN | 钉钉机器人令牌 |
| FSKEY | 飞书 Webhook 密钥 |
| TG_BOT_TOKEN | Telegram 机器人令牌 |
| TG_USER_ID | Telegram 用户 ID |
| PUSH_KEY | Server 酱密钥 |

5. 工作流将每天 UTC 2 点（UTC+8 10点）自动运行，也可以手动触发

## 配置说明

| 变量名 | 说明 | 默认值 |
|--------|------|--------|
| RAINYUN_USER | 雨云用户名（支持多行） | - |
| RAINYUN_PASS | 雨云密码（支持多行） | - |
| HEADLESS | 是否以无头模式运行 | false |
| DEBUG | 是否启用调试模式 | false |
| AUTO_UPDATE | 是否启用自动更新 | false |
| MAX_WORKERS | 最大并发线程数 | 2 |
| MAX_RETRIES | 最大重试次数 | 1 |

## Cookie 缓存

- Cookie 保存在 `cookies/` 目录
- 每个账户使用独立的 Cookie 文件（基于用户名哈希）
- 支持 7 天免登录
- GitHub Actions 中通过 `actions/cache` 持久化

## 常见问题

### 验证码识别失败

ICR 模块使用旋转分析和模板匹配算法，识别率较高。脚本会自动重试，多次尝试后通常能成功通过验证。

### Chrome 初始化失败

- 确保 Chrome 和 ChromeDriver 版本匹配
- 检查 GitHub Actions 日志中的错误信息
- 项目已优化 Chrome 选项配置，支持 headless 模式

### 多账户并发端口冲突

项目已实现线程锁机制，确保 Chrome 实例按顺序初始化，避免端口冲突。

## 许可证

本项目采用 GNU General Public License v3.0 许可证。

## 免责声明

- 本工具仅用于学习和个人使用
- 使用本工具应遵守雨云官方的用户协议和相关规定
- 不对因使用本工具可能产生的任何后果负责
