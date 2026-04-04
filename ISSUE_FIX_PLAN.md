# 网站问题修复计划

> 生成时间: 2026-04-04
> 项目: E:\coding\myweb

---

## 问题优先级分类

### P0 - 严重问题（必须修复，影响核心功能）

| 序号 | 问题 | 文件位置 | 详细描述 | 修复方案 |
|:---:|------|---------|---------|---------|
| P0-1 | 简历下载路径硬编码 | `index.html:1402` | `C:\\Users\\hp\\Desktop\\南京大学_许信园简历.pdf` 是本地路径，部署后无法使用 | 修改为相对路径 `./南京大学_许信园简历.pdf` 或上传到CDN |
| P0-2 | 联系表单无实际功能 | `index.html:2003-2007` | 表单提交只弹出alert演示，无真实后端 | 接入表单服务（如Formspree、EmailJS）或标记为"演示版" |
| P0-3 | 社交链接全部为占位符 | `index.html:1832-1837` | GitHub、微信链接都是 `#` | 添加真实的GitHub仓库地址和微信联系方式 |

### P1 - 高优先级（影响用户体验和SEO）

| 序号 | 问题 | 文件位置 | 详细描述 | 修复方案 |
|:---:|------|---------|---------|---------|
| P1-1 | profile.html 是未完成模板 | `profile.html` | 包含占位符 `your.email@example.com`、`linkedin.com/in/yourname` | 删除 profile.html 或用真实内容替换 |
| P1-2 | 缺少 Open Graph 标签 | `index.html:1-7` | 分享到社交媒体无预览图 | 添加 og:title, og:description, og:image 等 |
| P1-3 | 移动端导航菜单不自动关闭 | `index.html:1182-1202` | 移动菜单点击链接后不会自动收起 | 修复 JS 逻辑，点击导航链接后关闭移动菜单 |

### P2 - 中优先级（影响专业度）

| 序号 | 问题 | 文件位置 | 详细描述 | 修复方案 |
|:---:|------|---------|---------|---------|
| P2-1 | 缺少 SEO meta keywords | `index.html` | 没有 keywords 标签 | 添加相关关键词 |
| P2-2 | 微信二维码图片验证 | `index.html:1854` | 需确认 `wechat-qr.png` 文件存在且正确 | 检查文件，如无则添加 |
| P2-3 | 缺少真实头像 | `index.html:1414-1426` | 使用 SVG 占位符 | 替换为真实照片 |
| P2-4 | 缺少 sitemap.xml | 项目根目录 | 不利于搜索引擎收录 | 添加 sitemap.xml |
| P2-5 | 缺少 robots.txt | 项目根目录 | 无法控制爬虫访问 | 添加 robots.txt |

---

## 推荐的修复顺序

```
Step 1: P0-3  →  修复社交链接占位符
Step 2: P0-1  →  修复简历下载路径
Step 3: P0-2  →  联系表单功能处理
Step 4: P1-3  →  修复移动端导航
Step 5: P1-2  →  添加 Open Graph 标签
Step 6: P1-1  →  处理 profile.html 模板
Step 7: P2-1~5 →  SEO 和细节优化
```

---

## 详细修复指南

### Step 1: P0-3 修复社交链接

**文件**: `index.html`

```html
<!-- 修复前 -->
<a href="#" class="social-icon" title="GitHub">...</a>
<a href="#" class="social-icon" title="WeChat" id="wechatBtn">...</a>

<!-- 修复后 - 添加真实链接 -->
<a href="https://github.com/yourusername" class="social-icon" title="GitHub">...</a>
<!-- 微信链接保持 #，通过微信二维码图片展示 -->
```

### Step 2: P0-1 修复简历下载路径

**文件**: `index.html:1402`

```html
<!-- 修复前 -->
<a href="C:\\Users\\hp\\Desktop\\南京大学_许信园简历.pdf" download>

<!-- 修复后 - 使用相对路径 -->
<a href="./南京大学_许信园简历.pdf" download>
<!-- 或上传到CDN -->
<a href="https://cdn.yoursite.com/南京大学_许信园简历.pdf" download>
```

### Step 3: P0-2 联系表单处理

**方案A**: 使用第三方表单服务（推荐）

注册 [Formspree](https://formspree.io/) 或 [EmailJS](https://www.emailjs.com/)，获取 endpoint 后修改:

```javascript
// 修复前
contactForm.addEventListener('submit', (e) => {
    e.preventDefault();
    alert('感谢您的留言！...'); // 只是演示
});

// 修复后 - 使用 Formspree
contactForm.action = "https://formspree.io/f/your-form-id";
contactForm.method = "POST";
// 移除自定义 submit 监听器，或保留作成功提示
```

**方案B**: 标记为演示模式

```javascript
contactForm.addEventListener('submit', (e) => {
    e.preventDefault();
    const isZh = document.body.classList.contains('zh-mode');
    alert(isZh
        ? '感谢您的留言！此表单为演示版本，请通过邮箱联系我：xxyselion@163.com'
        : 'Thank you! This form is a demo. Please contact me via: xxyselion@163.com'
    );
});
```

### Step 4: P1-3 修复移动端导航

**文件**: `index.html:1914-1918`

```javascript
// 修复前 - 只关闭菜单，未阻止默认行为可能导致问题
mobileMenu.querySelectorAll('.nav-item').forEach(link => {
    link.addEventListener('click', () => {
        mobileMenu.classList.remove('active');
    });
});

// 修复后 - 确认正确关闭
document.querySelectorAll('#mobileMenu .nav-item').forEach(link => {
    link.addEventListener('click', (e) => {
        e.preventDefault();
        const targetId = link.getAttribute('href').substring(1);
        const targetSection = document.getElementById(targetId);
        if (targetSection) {
            targetSection.scrollIntoView({ behavior: 'smooth' });
        }
        mobileMenu.classList.remove('active');
    });
});
```

### Step 5: P1-2 添加 Open Graph 标签

**文件**: `index.html` 的 `<head>` 部分

```html
<!-- 在现有 meta 标签后添加 -->
<meta property="og:title" content="许信园 - AI产品经理 | Xu Xinyuan - AI Product Manager">
<meta property="og:description" content="南京大学地图学与地理信息系统硕士，专注于AI产品规划与落地">
<meta property="og:image" content="https://yoursite.com/og-image.png">
<meta property="og:url" content="https://yoursite.com/">
<meta property="og:type" content="website">
<meta name="twitter:card" content="summary_large_image">
```

### Step 6: P1-1 处理 profile.html

**方案**: 删除或用 index.html 替换

```bash
# 如果 profile.html 不再需要
rm profile.html

# 并更新任何引用 profile.html 的链接
```

### Step 7: P2-1~5 SEO 和细节优化

| 任务 | 操作 |
|------|------|
| keywords | 在 `<head>` 添加 `<meta name="keywords" content="AI产品经理,南京大学,许信园,产品经理,GIS">` |
| wechat-qr.png | 确认文件存在，如无则添加 |
| 头像 | 用真实照片替换 SVG 占位符 |
| sitemap.xml | 创建 sitemap.xml 提交到搜索引擎 |
| robots.txt | 创建 robots.txt |

---

## 修复状态跟踪

| 序号 | 问题 | 状态 | 备注 |
|:---:|------|:----:|------|
| P0-1 | 简历下载路径硬编码 | ✅ 已修复 | 改为 `./南京大学_许信园简历.pdf` |
| P0-2 | 联系表单无实际功能 | ✅ 已修复 | 改进提示并显示邮箱联系方式 |
| P0-3 | 社交链接占位符 | ✅ 已修复 | GitHub 链接改为真实地址 |
| P1-1 | profile.html 模板 | ✅ 已修复 | 已删除 profile.html |
| P1-2 | 缺少 Open Graph | ✅ 已修复 | 添加 og:* 和 twitter:* 标签 |
| P1-3 | 移动端导航 | ✅ 已修复 | 点击后平滑滚动并关闭菜单 |
| P2-1 | 缺少 keywords | ✅ 已修复 | 已添加到 meta |
| P2-2 | 微信二维码 | ✅ 已验证 | wechat-qr.png 文件存在 |
| P2-3 | 缺少真实头像 | ✅ 已修复 | 替换为 `avatar.jpg` 证件照 |
| P2-4 | 缺少 sitemap.xml | ✅ 已添加 | |
| P2-5 | 缺少 robots.txt | ✅ 已添加 | |

---

## 附录: 验证检查清单

修复完成后，请验证:

- [ ] 简历下载按钮点击后能正常下载
- [ ] GitHub 链接指向真实仓库
- [ ] 微信按钮点击显示真实二维码
- [ ] 移动端菜单点击链接后正常关闭
- [ ] LinkedIn 等链接指向真实页面
- [ ] 分享到微信/QQ 时有预览图
- [ ] 表单提交有真实反馈（不管是发邮件还是提示邮箱）
