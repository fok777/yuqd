# yuqd

雨云自动签到工具，基于 Selenium + ICR（旋转模板匹配）验证码识别。

## 功能

- 自动登录雨云账户
- 自动识别腾讯 TCaptcha 点选验证码
- 每日自动签到获取积分
- 支持多账户
- 支持代理

## 使用

```bash
pip install -r requirements.txt
cp .env.example .env
# 编辑 .env 填入账号密码
python rainyun_signin.py
```

## GitHub Actions

Fork 后配置 Secrets：
- `RAINYUN_USER` - 雨云邮箱
- `RAINYUN_PASS` - 雨云密码
- `PROXY_SERVER` - 代理地址（可选）

## License

MIT
