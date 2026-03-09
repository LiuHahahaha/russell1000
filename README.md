# 📈 Russell 1000 美股异动日报

> 每个交易日盘后自动扫描罗素1000全部成分股，找出**涨幅前30 / 跌幅前30**，
> 抓取最新新闻分析原因，生成精美网页 + 发送邮件。
> **完全免费，无需服务器。**

---

## 🌐 工作原理

```
每天美东 17:30 收盘
        ↓
GitHub Actions 自动启动（UTC 22:00）
        ↓
Python 脚本抓取真实行情 + 新闻
        ↓
生成 docs/index.html（注入真实数据）
        ↓
自动 commit → GitHub Pages 发布
        ↓
固定网址更新 ✅  +  发送邮件通知 ✅
```

---

## 🚀 五步部署

### 第一步：Fork 本仓库

右上角点击 **Fork** → 复制到你自己的 GitHub 账号。

---

### 第二步：开启 GitHub Pages

进入你 Fork 后的仓库：

**Settings → Pages → Source**

选择：
- Branch: `main`
- Folder: `/docs`

点击 **Save**。

你的**固定网址**就是：
```
https://【你的GitHub用户名】.github.io/【仓库名】
```
例如：`https://johndoe.github.io/russell1000`

---

### 第三步：配置 Secrets

进入仓库 → **Settings → Secrets and variables → Actions → New repository secret**

| Secret 名称 | 内容 | 是否必填 |
|------------|------|---------|
| `EMAIL_FROM` | 你的 Gmail 地址 | ✅ |
| `EMAIL_PASSWORD` | Gmail **App 密码**（见下方） | ✅ |
| `EMAIL_TO` | 收件人邮箱（可与 FROM 相同） | ✅ |
| `PAGES_URL` | 你的 GitHub Pages 网址 | 推荐填 |
| `SMTP_HOST` | `smtp.gmail.com` | 可不填 |
| `SMTP_PORT` | `465` | 可不填 |
| `NEWS_API_KEY` | NewsAPI.org 免费 Key | 可不填（有更多新闻） |

> **📌 Gmail App 密码获取：**
> 1. [myaccount.google.com/security](https://myaccount.google.com/security)
> 2. 开启**两步验证**
> 3. 搜索"应用专用密码" → 生成 → 复制16位

> **📌 NewsAPI 免费 Key（可选）：**
> 1. 注册 [newsapi.org](https://newsapi.org)（免费）
> 2. 每天100次请求，覆盖60只股票够用
> 3. 不填时自动使用 Yahoo Finance RSS

---

### 第四步：启用 GitHub Actions

进入仓库 → **Actions** 标签 → 点击确认按钮启用。

---

### 第五步：手动测试

**Actions → 美股异动日报 → Run workflow → Run workflow**

等待约 **20-40 分钟**（需下载约1000只股票数据 + 抓60条新闻）。

完成后：
- ✅ GitHub Pages 网址已更新为真实数据
- ✅ 收件箱收到邮件通知

---

## ⏰ 更新时间

| 时区 | 时间 |
|------|------|
| 美东（夏令时） | 每个交易日 18:00 |
| 美东（冬令时） | 每个交易日 17:00 |
| 北京时间（夏令时） | 次日 06:00 |
| 北京时间（冬令时） | 次日 07:00 |

---

## 📁 文件结构

```
├── .github/
│   └── workflows/
│       └── daily.yml          ← GitHub Actions 定时任务
├── scripts/
│   └── monitor.py             ← 核心抓取 + 生成脚本
├── docs/
│   └── index.html             ← 每日自动更新的网页（GitHub Pages 发布）
├── requirements.txt
└── README.md
```

---

## ❓ FAQ

**Q: 每月费用？**
公开仓库 GitHub Actions 完全免费，无限分钟。GitHub Pages 也免费。

**Q: 数据准确性？**
行情数据来自 Yahoo Finance（与 Yahoo Finance 官网一致）。新闻来自 NewsAPI / Yahoo Finance RSS，为真实标题。

**Q: 可以改成其他指数吗？**
修改 `scripts/monitor.py` 中的 `FALLBACK` 列表或 `get_tickers()` 函数即可。

**Q: 网页能分享给别人吗？**
可以。直接把 GitHub Pages 网址发给任何人，无需登录，手机电脑都能看。
