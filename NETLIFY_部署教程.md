# Netlify 部署教程 - 基金净值查询

## 方法一：拖拽部署（最简单，3分钟完成）

### 步骤：

1. **访问 Netlify**
   - 打开 https://app.netlify.com/drop
   - 无需注册即可使用（但建议注册以便管理）

2. **准备文件**
   - 将以下文件放在同一个文件夹中：
     - `wy1714470801.html`（主文件）
     - `manifest.json`（PWA配置）
     - `netlify.toml`（Netlify配置）

3. **拖拽上传**
   - 直接将整个文件夹拖到 Netlify Drop 页面
   - 或者选择文件夹上传

4. **获取链接**
   - 上传完成后，Netlify会自动生成一个链接
   - 格式：`https://随机名称.netlify.app`
   - 立即可以在手机访问！

5. **自定义域名（可选）**
   - 注册/登录 Netlify
   - 在 Site settings → Domain management
   - 可以修改为：`你的名字-fund.netlify.app`

---

## 方法二：通过 Git 部署（推荐，支持自动更新）

### 步骤：

1. **创建 GitHub 仓库**
   ```bash
   # 在项目文件夹中执行
   git init
   git add .
   git commit -m "初始化基金查询工具"
   
   # 在 GitHub 创建新仓库后
   git remote add origin https://github.com/你的用户名/仓库名.git
   git push -u origin main
   ```

2. **连接 Netlify**
   - 访问 https://app.netlify.com
   - 点击 "Add new site" → "Import an existing project"
   - 选择 "GitHub"，授权并选择你的仓库

3. **配置构建**
   - Build command: 留空
   - Publish directory: `.`（当前目录）
   - 点击 "Deploy site"

4. **自动部署**
   - 以后每次 `git push`，Netlify 会自动更新网站

---

## 方法三：使用 Netlify CLI（开发者推荐）

### 安装 CLI：
```bash
npm install -g netlify-cli
```

### 部署命令：
```bash
# 登录
netlify login

# 初始化项目
netlify init

# 部署
netlify deploy --prod
```

---

## 部署后的操作

### 1. 在手机上使用

**iOS (Safari):**
1. 打开 Netlify 提供的链接
2. 点击底部"分享"按钮
3. 选择"添加到主屏幕"
4. 设置名称为"基金查询"
5. 完成！图标会出现在主屏幕

**Android (Chrome):**
1. 打开 Netlify 提供的链接
2. 点击右上角菜单（三个点）
3. 选择"添加到主屏幕"
4. 确认添加
5. 完成！

### 2. 自定义域名

在 Netlify 控制台：
- Site settings → Domain management
- Add custom domain
- 可以使用自己的域名或修改 Netlify 子域名

### 3. 启用 HTTPS

Netlify 自动提供免费 HTTPS 证书，无需配置！

---

## 常见问题

### Q: 部署后无法访问基金数据？
A: 这是正常的，因为基金接口可能有跨域限制。解决方案：
- 数据接口使用的是 JSONP，通常不受跨域限制
- 如果遇到问题，可以使用 Netlify Functions 作为代理

### Q: 如何更新网站？
A: 
- 方法一（拖拽）：重新拖拽文件夹到 Netlify
- 方法二（Git）：修改代码后 `git push`
- 方法三（CLI）：运行 `netlify deploy --prod`

### Q: 可以绑定自己的域名吗？
A: 可以！在 Netlify 控制台添加自定义域名，然后修改域名的 DNS 记录指向 Netlify。

### Q: 费用如何？
A: 
- 个人使用完全免费
- 每月 100GB 流量
- 300 分钟构建时间
- 对于这个项目完全够用

---

## 优化建议

### 添加 Service Worker（离线支持）

创建 `sw.js` 文件：
```javascript
self.addEventListener('install', (e) => {
  e.waitUntil(
    caches.open('fund-v1').then((cache) => {
      return cache.addAll([
        './wy1714470801.html',
        './manifest.json'
      ]);
    })
  );
});

self.addEventListener('fetch', (e) => {
  e.respondWith(
    caches.match(e.request).then((response) => {
      return response || fetch(e.request);
    })
  );
});
```

在 HTML 中注册：
```javascript
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('./sw.js');
}
```

---

## 快速开始命令

```bash
# 1. 确保文件都在当前目录
ls

# 2. 如果使用 Git 方式
git init
git add .
git commit -m "部署基金查询工具"

# 3. 推送到 GitHub（需要先在 GitHub 创建仓库）
git remote add origin https://github.com/你的用户名/fund-query.git
git push -u origin main

# 4. 然后在 Netlify 网站导入这个仓库
```

---

## 示例链接

部署成功后，你会得到类似这样的链接：
- `https://fund-query-abc123.netlify.app`
- 可以改成：`https://my-fund-query.netlify.app`

立即在手机浏览器打开，添加到主屏幕，就像原生 App 一样使用！

---

## 需要帮助？

如果遇到问题：
1. 检查 Netlify 的部署日志
2. 确保所有文件都已上传
3. 查看浏览器控制台的错误信息
