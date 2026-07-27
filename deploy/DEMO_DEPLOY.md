# AdPilot Demo — 独立域名部署说明

> 纯前端 Demo：所有接口由前端 mock，**不需要后端、不需要数据库、不需要反向代理**。
> 这台服务器上的其他项目完全不受影响（各自独立目录 + 独立 server block）。

---

## 你会拿到的东西

- `adpilot-demo-dist.tar.gz` —— 打好的静态文件包（解压即用）
- `demo-nginx.conf` —— 独立域名的 Nginx 配置模板

---

## 部署步骤（Linux + Nginx，最常见）

### 1. DNS 解析
去域名商后台，给独立域名加一条 **A 记录**指向这台服务器的公网 IP：

```
类型   主机记录   记录值
A      demo       <这台服务器的公网IP>      # 得到 demo.teensing.com
```

等解析生效（`ping demo.teensing.com` 能通到你的 IP 即可）。

### 2. 上传并解压静态文件
```bash
# 本地上传（把 tar.gz 传到服务器）
scp adpilot-demo-dist.tar.gz user@<服务器IP>:/tmp/

# 服务器上解压到独立目录
sudo mkdir -p /var/www/adpilot-demo
sudo tar -xzf /tmp/adpilot-demo-dist.tar.gz -C /var/www/adpilot-demo --strip-components=1
# 解压后 /var/www/adpilot-demo/index.html 应该存在
ls /var/www/adpilot-demo
```

### 3. 配置 Nginx
```bash
# 把 demo-nginx.conf 放进 Nginx 配置目录
sudo cp demo-nginx.conf /etc/nginx/conf.d/adpilot-demo.conf

# 编辑，改两处：server_name(你的域名) 和 root(你的路径)
sudo nano /etc/nginx/conf.d/adpilot-demo.conf

# 测试配置无误后 reload（不影响其他站点）
sudo nginx -t && sudo nginx -s reload
```

### 4. 访问
浏览器打开 `http://demo.teensing.com` —— 会**自动登录**直接进后台，所有页面都是假数据，随便点。

---

## 如果是 Windows Server + Nginx（和你主站同款环境）

```powershell
# 1. 解压到独立目录，例如 C:\adpilot_demo\
#    （把 tar.gz 里的内容放进去，确保 C:\adpilot_demo\index.html 存在）

# 2. 把 demo-nginx.conf 的 server{} 段加进 nginx.conf 的 http{} 里，改：
#      server_name  demo.teensing.com;
#      root         C:/adpilot_demo;        # 注意用正斜杠

# 3. reload
cd C:\nginx
nginx -t
nginx -s reload
```

---

## 常见问题

| 现象 | 原因 / 解决 |
|------|------------|
| 打开是白屏 | 检查 `root` 路径对不对，`index.html` 是否在该目录 |
| 刷新子页面 404 | Nginx 少了 `try_files $uri /index.html`（SPA 回退） |
| 页面能开但没数据 | 正常情况下不会——demo 数据全在前端。若真没数据，强刷清缓存 |
| JS 加载很慢 | 确认 Nginx 开了 `gzip on`（配置模板里已带） |
| 想验证是否 demo 模式 | 页面直接进后台不需登录 = demo 已生效 |

---

## 这份包的特性（构建时已写死）

- **demo 模式常开**：不依赖域名，任何域名/IP 打开都是 demo（用 `VITE_DEMO=1` 构建）
- **自动登录**：无需账号密码，打开即进
- **品牌**：AdPilot（标题 / Logo 已换，改品牌见 `src/config/brand.js` 后重新构建）
- **全接口 mock**：Overview / Campaigns / Finance / Partner Hub / Reports / Resources / Profile 等均有假数据

---

## 以后要更新 demo 内容

在 `demo` 分支改完代码后，重新打包上传即可：
```bash
git checkout demo
git merge master        # 需要同步主站新功能时
VITE_DEMO=1 npm run build
tar -czf adpilot-demo-dist.tar.gz -C dist .
# 上传覆盖 /var/www/adpilot-demo，reload 可选
```
