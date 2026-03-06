# 基金净值查询 - 手机端部署指南

## 方案一：直接使用（推荐）

### 1. 本地测试
```bash
# 使用Python启动本地服务器
python -m http.server 8000

# 或使用Node.js
npx serve .
```

然后在手机浏览器访问：`http://你的电脑IP:8000/wy1714470801.html`

### 2. 添加到主屏幕
- iOS：Safari浏览器 → 分享 → 添加到主屏幕
- Android：Chrome浏览器 → 菜单 → 添加到主屏幕

## 方案二：部署到云端

### 使用GitHub Pages（免费）
1. 创建GitHub仓库
2. 上传所有文件
3. 在仓库设置中启用GitHub Pages
4. 访问：`https://你的用户名.github.io/仓库名/wy1714470801.html`

### 使用Vercel（免费）
1. 访问 https://vercel.com
2. 导入GitHub仓库或直接拖拽文件夹
3. 自动部署，获得HTTPS链接

### 使用Netlify（免费）
1. 访问 https://netlify.com
2. 拖拽文件夹到部署区域
3. 获得HTTPS链接

## 方案三：打包成原生App

### 使用Cordova/Capacitor
```bash
# 安装Capacitor
npm install @capacitor/core @capacitor/cli
npx cap init

# 添加平台
npx cap add android
npx cap add ios

# 构建
npx cap sync
npx cap open android  # 或 ios
```

### 使用React Native WebView
创建React Native项目，使用WebView组件加载HTML文件

## 注意事项

1. **跨域问题**：部署到服务器后，基金数据接口可能存在跨域限制
2. **HTTPS要求**：某些功能需要HTTPS环境
3. **缓存策略**：已实现localStorage缓存，离线也能查看历史数据
4. **自动刷新**：交易时间内每30秒自动刷新数据

## 优化建议

1. 添加Service Worker实现离线访问
2. 添加推送通知功能
3. 优化移动端触摸体验
4. 添加深色模式支持
