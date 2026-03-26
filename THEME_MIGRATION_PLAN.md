# Hugo 站点维护手册: digio-theme 使用说明
 
 > 适用项目: `hugosorodo`
 > 当前目标: 用这份文档指导后续内容更新、模块调整和主题功能使用
 
 ---
 
 ## 一、当前站点的实际结构
 
 当前站点已经切换到 `digio-theme`，并且为了适配 GitHub Pages 子路径部署，项目根目录增加了自定义 `layouts/` 覆盖主题模板。
 
 ### 你真正需要关注的目录
 
 ```text
 config.toml                     # 站点主配置，Hugo 实际读取的配置文件
 content/
   _index.md                     # 首页内容
   about.md                      # About 页面
   me.md                         # Me 页面，使用 digio-theme 的 me 布局
   research/                     # 研究方向栏目
   publications/                 # 论文栏目
   projects/                     # 项目栏目
 layouts/                        # 对 digio-theme 的本地覆盖模板
   _partials/
   baseof.html
   home.html
   me.html
   section.html
   term.html
 themes/digio-theme/             # 主题子模块
 .github/workflows/gh-pages.yml  # GitHub Pages 自动部署
 ```
 
 ### 当前维护原则
 
 - **配置文件只看 `config.toml`**
 - **页面内容主要改 `content/`**
 - **样式和渲染逻辑优先看根目录 `layouts/`，不要直接改主题源码**
 - **主题升级后，如果渲染异常，优先检查 `layouts/` 覆盖模板是否仍兼容**
 
 ---
 
 ## 二、日常更新最常见的 3 类操作
 
 ### 1. 发布一篇新博客或新文章
 
 你可以把文章放到以下任一栏目目录下：
 
 - **`content/research/`**：研究记录、方法、实验笔记
 - **`content/publications/`**：论文解读、论文介绍、论文补充说明
 - **`content/projects/`**：项目总结、项目阶段记录、Demo 说明
 
 一个最简文章模板如下：
 
 ```toml
 +++
 title = "文章标题"
 date = 2026-03-26
 draft = false
 listIcon = "writing.png"
 teaser = "这是一段展示在栏目列表中的摘要"
 tags = ["research"]
 +++
 ```
 
 正文直接写 Markdown 即可。
 
 ### 2. 修改一个栏目页
 
 每个栏目目录下的 `_index.md` 控制这个栏目首页：
 
 - **标题**
 - **栏目说明文字**
 - **栏目图标**
 - **外部链接条目**
 
 例如：
 
 - **`content/research/_index.md`**：研究方向栏目页
 - **`content/publications/_index.md`**：论文栏目页
 - **`content/projects/_index.md`**：项目栏目页
 
 ### 3. 更新首页个人信息
 
 首页内容在 `content/_index.md`，主要维护：
 
 - **头像图** `portraitImage`
 - **首页标题** `introTitle`
 - **首页介绍** `introBody`
 - **当前状态** `statusText`
 - **最近关注内容** `watchingText`
 - **外链** `watchingUrl`
 
 ---
 
 ## 三、`digio-theme` 在你项目里的页面映射
 
 ### 首页
 
 文件：`content/_index.md`
 
 当前可用参数：
 
 ```toml
 +++
 portraitImage = "portrait.png"
 introTitle = "Sorodo 的科研空间"
 introBody = """
 这里写首页简介。
 """
 statusImage = "status.png"
 statusText = "这里写当前状态"
 watchingImage = "watching.png"
 watchingText = "这里写最近关注内容"
 watchingUrl = "https://example.com"
 +++
 ```
 
 注意：
 
 - **这里的图片路径建议使用相对文件名**，例如 `portrait.png`
 - **不要写成 `/portrait.png`**，否则在 GitHub Pages 子路径下可能再次出现资源路径错误
 
 ### Me 页面
 
 文件：`content/me.md`
 
 该页面已经设置：
 
 ```toml
 layout = "me"
 ```
 
 可维护字段：
 
 - **`likes`**
 - **`dislikes`**
 - **`hobbies`**
 
 这三个数组会自动渲染成 digio-theme 的三栏样式。
 
 ### About 页面
 
 文件：`content/about.md`
 
 这是一个普通 Markdown 页面，适合放：
 
 - **个人简介长文**
 - **学术履历**
 - **教育经历**
 - **联系方式**
 - **FAQ**
 
 如果你希望 About 也做成更强定制页，可以后续再新增专用 layout。
 
 ---
 
 ## 四、如何新增、删除、修改栏目模块
 
 你现在的导航模块由 `config.toml` 中的 `[[menu.main]]` 控制。
 
 当前已有：
 
 - **Research**
 - **Publications**
 - **Projects**
 - **Me**
 - **About**
 
 ### 新增一个模块
 
 例如你想新增 `Notes`：
 
 #### 第一步：在 `config.toml` 中加菜单
 
 ```toml
 [[menu.main]]
   identifier = "notes"
   name       = "Notes"
   url        = "/notes"
 ```
 
 #### 第二步：创建目录和栏目首页
 
 新建 `content/notes/_index.md`：
 
 ```toml
 +++
 title = "Notes"
 icon = "writing.png"
 +++
 
 这里是 Notes 栏目的说明。
 ```
 
 #### 第三步：往这个目录里放文章
 
 例如：`content/notes/my-first-note.md`
 
 ### 删除一个模块
 
 删除模块通常做三件事：
 
 - **从 `config.toml` 删除对应 `[[menu.main]]`**
 - **删除对应 `content/栏目名/` 目录**
 - **如果首页或其他地方提到它，也同步删掉相关描述**
 
 ### 修改模块名称
 
 只需同时修改：
 
 - **`config.toml` 里的 `name`**
 - **栏目 `_index.md` 里的 `title`**
 
 如果 URL 不想变，就保留原来的 `url` 和目录名不动。
 
 ---
 
 ## 五、栏目页和文章页的 front matter 用法
 
 ### 栏目页 `_index.md` 推荐写法
 
 ```toml
 +++
 title = "研究方向"
 icon = "writing.png"
 +++
 
 这里写栏目介绍。
 ```
 
 支持的常用字段：
 
 - **`title`**：栏目标题
 - **`icon`**：栏目图标
 - **`externalLinks`**：把外部链接也渲染进栏目列表
 
 ### 文章页推荐写法
 
 ```toml
 +++
 title = "一篇文章"
 date = 2026-03-26
 draft = false
 listIcon = "writing.png"
 teaser = "文章摘要"
 tags = ["research"]
 +++
 ```
 
 常用字段说明：
 
 - **`title`**：标题
 - **`date`**：日期
 - **`draft`**：是否草稿
 - **`listIcon`**：列表图标，通常写 `writing.png`
 - **`teaser`**：列表摘要
 - **`tags`**：标签页归类
 
 ### 关于 `tags`
 
 当前自定义栏目模板已经直接使用栏目目录下的文章，不再强依赖标签筛选。
 
 但仍建议你保留标签，用于：
 
 - **后续标签页整理**
 - **跨栏目聚合检索**
 - **主题扩展时复用**
 
 推荐示例：
 
 - **研究文章**：`tags = ["research"]`
 - **论文相关文章**：`tags = ["publications"]`
 - **项目记录**：`tags = ["projects"]`
 
 ---
 
 ## 六、如何使用 `externalLinks`
 
 如果一个栏目里你不想写本地文章，而是想直接挂外链，可以在栏目 `_index.md` 中加 `externalLinks`。
 
 示例：
 
 ```toml
 +++
 title = "发表论文"
 icon = "writing.png"
 
 [[externalLinks]]
   title = "Paper A"
   url = "https://arxiv.org/abs/xxxx.xxxxx"
   date = "2026-03-01"
   icon = "writing.png"
   teaser = "这是一篇论文简介"
 
 [[externalLinks]]
   title = "Project Demo"
   url = "https://github.com/yourname/yourproject"
   date = "2026-03-10"
   icon = "writing.png"
   teaser = "项目演示地址"
 +++
 ```
 
 注意：
 
 - **日期建议使用 `YYYY-MM-DD`**
 - **`icon`` 也建议写相对文件名**
 - **同一个栏目可以同时混合本地文章和外链条目**
 
 ---
 
 ## 七、图片与资源使用规则
 
 当前项目为了兼容 GitHub Pages 子路径，资源路径有以下规则：
 
 - **优先使用主题自带资源文件名**，例如 `writing.png`
 - **如果你有自定义图片，放到 `static/` 下**
 - **front matter 里尽量写相对文件名或相对资源路径**
 - **避免写以 `/` 开头的绝对路径**
 
 ### 推荐写法
 
 - **`writing.png`**
 - **`portrait.png`**
 - **`status.png`**
 - **`watching.png`**
 
 ### 不推荐写法
 
 - **`/writing.png`**
 - **`/portrait.png`**
 
 如果你要增加自己的图片，例如：
 
 - `static/my-avatar.png`
 - `static/icons/lab.png`
 
 那么 front matter 可以写：
 
 - **`portraitImage = "my-avatar.png"`**
 - **`icon = "icons/lab.png"`**
 
 ---
 
 ## 八、博客发布工作流
 
 以后你每次更新内容，按这个流程即可：
 
 ### 写新内容
 
 - **新增或修改 `content/` 下的 Markdown**
 - **确认 `draft = false`**
 - **补齐 `title`、`date`、`teaser`、`listIcon`**
 
 ### 提交发布
 
 ```bash
 git add .
 git commit -m "feat: add new article"
 git push origin main
 ```
 
 推送后 GitHub Actions 会自动部署到：
 
 `https://sorodo.github.io/hugosorodo/`
 
 ---
 
 ## 九、如果以后要改主题模板，优先改哪里
 
 你现在应该优先修改根目录 `layouts/`，而不是直接改 `themes/digio-theme/`。
 
 ### 常见对应关系
 
 - **首页布局**：`layouts/home.html`
 - **栏目列表页**：`layouts/section.html`
 - **标签页**：`layouts/term.html`
 - **整体骨架**：`layouts/baseof.html`
 - **头部资源**：`layouts/_partials/head.html`
 - **导航菜单**：`layouts/_partials/menu.html`
 - **Me 页面布局**：`layouts/me.html`
 
 这样做的好处：
 
 - **升级主题时不容易把你的修改冲掉**
 - **自定义逻辑集中在项目本身，更容易维护**
 
 ---
 
 ## 十、主题更新方法
 
 `digio-theme` 当前是子模块管理，后续更新方式：
 
 ```bash
 git submodule update --remote themes/digio-theme
 git add themes/digio-theme
 git commit -m "chore: update digio-theme"
 git push origin main
 ```
 
 更新主题后，重点检查：
 
 - **首页是否正常**
 - **栏目页列表是否正常**
 - **Me 页面是否正常**
 - **图片路径是否仍然正常**
 - **GitHub Pages 子路径部署是否仍然正常**
 
 ---
 
 ## 十一、常见修改示例
 
 ### 1. 修改首页状态
 
 编辑 `content/_index.md`：
 
 ```toml
 statusText = "当前状态：正在撰写新论文"
 watchingText = "正在阅读：某篇最新 arXiv 论文"
 watchingUrl = "https://arxiv.org/abs/xxxx.xxxxx"
 ```
 
 ### 2. 新增一篇项目文章
 
 新建 `content/projects/project-a.md`：
 
 ```toml
 +++
 title = "项目 A"
 date = 2026-03-26
 draft = false
 listIcon = "writing.png"
 teaser = "项目 A 的阶段总结"
 tags = ["projects"]
 +++
 ```
 
 ### 3. 给 Publications 增加外链
 
 编辑 `content/publications/_index.md`，加入：
 
 ```toml
 [[externalLinks]]
   title = "My Paper"
   url = "https://doi.org/xxxx"
   date = "2026-03-26"
   icon = "writing.png"
   teaser = "论文简介"
 ```
 
 ### 4. 新增一个导航模块
 
 - **在 `config.toml` 新增 `[[menu.main]]`**
 - **创建对应 `content/模块名/_index.md`**
 - **往目录里加文章**
 
 ---
 
 ## 十二、注意事项
 
 - **不要再新增 `hugo.toml`，当前项目只保留 `config.toml`**
 - **不要直接恢复旧主题相关文件**
 - **不要轻易删除根目录 `layouts/`，它负责 GitHub Pages 子路径兼容**
 - **修改 front matter 时注意 TOML 语法，不要写坏数组和字符串**
 - **如果页面没显示，先检查是否还是 `draft = true`**
 
 ---
 
 ## 十三、建议你后续优先完善的内容
 
 - **完善 `about.md` 的真实个人介绍**
 - **完善 `me.md` 的 likes / dislikes / hobbies**
 - **给 `research`、`projects`、`publications` 各增加 2 到 5 篇真实内容**
 - **如果需要单独博客流，可以再新增 `notes/` 或 `blog/` 模块**
 - **如果需要简历页，可以新建 `cv.md` 或 `experience/` 模块**
 
 ---
 
 *最后更新：2026-03-26*
 *用途：hugosorodo 后续维护与内容更新手册*
