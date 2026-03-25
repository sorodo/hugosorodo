# Hugo 主题更换方案: digio-theme 科研展示站点

> 目标: 将现有 hugosorodo 站点迁移至 [digio-theme](https://github.com/danapixels/digio-theme)
> 用途: 科研领域对外展示个人研究

---

## 一、主题特性分析

### digio-theme 核心特点
- **风格**: 像素风格 + ASCII 艺术 + 黑白极简复古
- **结构**: 首页个人展示 + 文章/分区混合展示
- **特色功能**:
  - 首页个人状态展示（Status + Watching）
  - 文章列表图标自定义（contribute.png / writing.png）
  - 外部链接作为文章展示
  - About Me 页面（likes/dislikes/hobbies）

### 适用科研展示的场景
- ✅ 清晰的研究方向分区
- ✅ 论文/项目混合展示（支持外部链接）
- ✅ 个人学术状态实时更新
- ✅ 极简风格突出内容

---

## 二、迁移执行步骤

### 步骤 1: 备份与清理（执行前必做）

```bash
# 创建备份分支
cd d:\development\Blog\hugosorodo
git checkout -b backup-original
git push origin backup-original

# 切回 main 分支继续操作
git checkout main
```

### 步骤 2: 添加新主题

```bash
# 添加 digio-theme 作为子模块（推荐，便于后续更新）
git submodule add https://github.com/danapixels/digio-theme themes/digio-theme
```

### 步骤 3: 更新配置文件

新建 `hugo.toml`（digio-theme 使用 TOML 格式，可兼容新版 Hugo）:

```toml
baseURL = 'https://sorodo.github.io/hugosorodo/'
languageCode = 'zh-cn'
title = 'Sorodo Research'
theme = 'digio-theme'

# 分页设置
paginate = 10

[params]
  # 站点描述
  description = "Sorodo 的科研主页 - 记录研究历程与学术思考"
  
  # 页脚版权
  copyright = "© 2025 Sorodo"

# 主菜单（科研导向）
[[menu.main]]
  name = "首页"
  url = "/"
  weight = 10

[[menu.main]]
  name = "研究"
  url = "/research"
  weight = 20

[[menu.main]]
  name = "论文"
  url = "/publications"
  weight = 30

[[menu.main]]
  name = "项目"
  url = "/projects"
  weight = 40

[[menu.main]]
  name = "关于"
  url = "/about"
  weight = 50
```

### 步骤 4: 删除旧主题（可选）

```bash
# 移除旧主题子模块（如存在）
git submodule deinit themes/hello-4s3ti 2>/dev/null || true
rm -rf .git/modules/themes/hello-4s3ti
git rm -f themes/hello-4s3ti 2>/dev/null || true
```

---

## 三、内容结构调整

### 新建目录结构

```
content/
├── _index.md          # 首页配置
├── about.md           # 关于我
├── me.md              # 个人详情（likes/dislikes/hobbies）
├── research/
│   ├── _index.md      # 研究方向主页
│   └── 研究方向文章
├── publications/
│   ├── _index.md      # 论文列表主页
│   └── 论文文章
└── projects/
    ├── _index.md      # 项目主页
    └── 项目文章
```

### 首页配置 `content/_index.md`

```markdown
+++
portraitImage = "/images/avatar.png"
introTitle = "Sorodo 的科研空间"
introBody = """
这里是 Sorodo 的科研主页，记录研究历程、分享学术思考。
研究方向：XXX / YYY / ZZZ
欢迎交流与合作。
"""
statusImage = "/images/status.png"
statusText = "当前状态：正在进行 XXX 项目的数据分析"
watchingImage = "/images/watching.png"
watchingText = "正在关注：机器学习在YYY领域的最新进展"
watchingUrl = "https://example.com/paper-link"
+++
```

### 关于我页面 `content/me.md`

```markdown
+++
title = "关于我"
likes = [
  "深入探索复杂问题",
  "跨学科研究",
  "开源科学",
  "数据可视化",
  "学术写作"
]
dislikes = [
  "形式主义",
  "重复造轮子",
  "数据不透明"
]
hobbies = [
  "阅读论文",
  "编程实验",
  "学术交流",
  "技术博客写作"
]
+++

这里是关于我的详细介绍...
```

### 分区配置 `content/research/_index.md`

```markdown
+++
title = "研究方向"
icon = "/images/research.png"
externalLinks = [
  {
    title = "代表性论文一",
    url = "https://arxiv.org/abs/xxx",
    date = "2024-12-01",
    icon = "/images/paper.png",
    teaser = "发表在XXX会议的研究成果"
  }
]
+++

我的主要研究方向介绍...
```

### 文章模板 `content/research/example-post.md`

```markdown
+++
title = "研究方向示例文章"
date = 2025-03-25
listIcon = "/images/writing.png"
teaser = "这是一篇关于研究方向的示例文章摘要"
+++

文章内容...
```

---

## 四、资源文件准备

### 图片资源清单

将以下图片放入 `static/images/` 目录:

| 文件名 | 用途 | 建议尺寸 |
|--------|------|----------|
| `avatar.png` | 首页头像 | 128x128 |
| `status.png` | 状态图标 | 32x32 |
| `watching.png` | 关注图标 | 32x32 |
| `research.png` | 研究分区图标 | 32x32 |
| `publications.png` | 论文分区图标 | 32x32 |
| `projects.png` | 项目分区图标 | 32x32 |
| `writing.png` | 文章图标 | 32x32 |
| `paper.png` | 论文/外部链接图标 | 32x32 |

**图片风格建议**: 黑白像素风格，与主题保持一致

---

## 五、GitHub Actions 配置

现有的 `.github/workflows/gh-pages.yml` **无需修改**，已支持子模块。

如需启用 Hugo Extended 版本（某些主题需要），修改如下:

```yaml
- name: Setup Hugo
  uses: peaceiris/actions-hugo@v2
  with:
    hugo-version: 'latest'
    extended: true  # 取消注释此行
```

---

## 六、本地测试步骤

```bash
# 1. 进入项目目录
cd d:\development\Blog\hugosorodo

# 2. 拉取子模块（首次克隆后）
git submodule update --init --recursive

# 3. 启动本地服务器
hugo server -D

# 4. 浏览器访问 http://localhost:1313/hugosorodo/
```

---

## 七、部署步骤

```bash
# 1. 提交更改
git add .
git commit -m "feat: migrate to digio-theme for research showcase"

# 2. 推送触发自动部署
git push origin main

# 3. 等待 GitHub Actions 完成（约 1-2 分钟）

# 4. 访问验证
# https://sorodo.github.io/hugosorodo/
```

---

## 八、后续内容管理指南

### 发布新研究文章

```bash
# 创建新文章
hugo new content research/my-new-research.md

# 编辑 front matter:
# title: 文章标题
# date: 发布日期
# listIcon: /images/writing.png
# teaser: 文章摘要
```

### 添加论文（外部链接）

在对应分区的 `_index.md` 中添加 `externalLinks` 条目:

```toml
externalLinks = [
  {
    title = "论文标题",
    url = "https://arxiv.org/abs/xxx",
    date = "2025-03-25",
    icon = "/images/paper.png",
    teaser = "论文摘要/简介"
  }
]
```

### 更新首页状态

编辑 `content/_index.md` 修改:
- `statusText`: 当前研究状态
- `watchingText` + `watchingUrl`: 正在关注的内容

---

## 九、主题更新方法

```bash
# 更新主题到最新版本
git submodule update --remote themes/digio-theme

# 提交并推送
git add themes/digio-theme
git commit -m "chore: update digio-theme"
git push
```

---

## 十、常见问题排查

### Q1: 页面样式错乱
- 检查 `hugo.toml` 中 `baseURL` 是否正确
- 确认 hugo 版本 >= 0.100.0

### Q2: 图片不显示
- 图片必须放在 `static/images/` 下
- 路径使用 `/images/xxx.png` 格式（以 / 开头）

### Q3: 菜单不显示
- 确认 `hugo.toml` 中 `[[menu.main]]` 配置正确
- 检查 URL 路径是否正确

### Q4: 外部链接不显示
- 确认在对应分区的 `_index.md` 中配置
- 日期格式必须为 `YYYY-MM-DD`

---

## 附件: 一键执行脚本

```bash
#!/bin/bash
# save as: migrate-theme.sh

set -e

echo "=== Hugo 主题迁移脚本 ==="

# 备份分支
git checkout -b backup-$(date +%Y%m%d) || true

# 切回 main
git checkout main

# 添加新主题
git submodule add https://github.com/danapixels/digio-theme themes/digio-theme || true

# 清理旧内容（确认不需要后取消注释）
# rm -rf content/posts content/pixiv

# 创建新目录结构
mkdir -p content/research content/publications content/projects static/images

echo "=== 迁移完成 ==="
echo "请手动:"
echo "1. 复制 hugo.toml 配置"
echo "2. 准备图片资源到 static/images/"
echo "3. 创建内容页面"
echo "4. 运行 hugo server -D 测试"
```

---

*文档生成时间: 2025-03-25*
*主题版本: digio-theme latest*
