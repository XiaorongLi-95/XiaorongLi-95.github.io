# 个人主页使用指南

本项目基于 Jekyll + al-folio 主题构建，部署在 GitHub Pages。

---

## 修改内容对照表

| 要修改的内容 | 文件位置 |
|-------------|---------|
| **网站标题、姓名、邮箱、社交链接** | `_config.yml` |
| **首页个人简介** | `_pages/about.md` |
| **头像照片** | `assets/img/profile.jpg` (替换文件) |
| **发表论文** | `_bibliography/papers.bib` (BibTeX格式) |
| **教育经历 / 工作经历 / 荣誉奖励** | `_data/cv.yml` |
| **PDF简历** | `assets/pdf/resume.pdf` (替换文件) |
| **新闻动态** | `_news/` 目录下新建 `.md` 文件 |
| **合作者链接** | `_data/coauthors.yml` |
| **展示的GitHub仓库** | `_data/repositories.yml` |
| **项目展示** | `_projects/` 目录下的 `.md` 文件 |
| **导航栏样式（大小写等）** | `_sass/_base.scss` |
| **删除/隐藏导航栏tab** | `_config.yml` (blog) 或对应页面的 `nav: false` |

---

## 常用修改示例

### 1. 修改基本信息 (`_config.yml`)
```yaml
title: XiaorongLi           # 网站标题
first_name: Xiaorong        # 名
last_name: Li               # 姓
email: your@email.com       # 邮箱
github_username: xxx        # GitHub用户名
scholar_userid: xxx         # Google Scholar ID
orcid_id: xxx               # ORCID ID
```

### 2. 添加新论文 (`_bibliography/papers.bib`)
```bibtex
@inproceedings{your_paper_2024,
  title={论文标题},
  author={作者},
  booktitle={会议/期刊},
  year={2024},
  abbr={CVPR},              # 显示的缩写标签
  selected={true},          # true则显示在首页精选
  pdf={论文链接},
  code={代码链接}
}
```

### 3. 添加新闻 (`_news/` 目录)
新建文件如 `_news/your_news.md`:
```markdown
---
layout: post
date: 2024-12-21 15:59:00-0400
inline: true
related_posts: false
---
这里写新闻内容。
```

### 4. 修改教育/工作经历 (`_data/cv.yml`)
直接编辑对应的 YAML 列表项即可。

### 5. 论文作者名字加粗 (`_config.yml`)
在 `_config.yml` 中找到 `scholar:` 配置块，设置你的名字：
```yaml
scholar:
  last_name: Xiaorong      # 姓（用于匹配加粗）
  first_name: [Li]         # 名（数组格式，支持多个变体）
```
**注意**：这里的 `last_name` 和 `first_name` 是用来匹配 BibTeX 中作者名的，需要与论文中你的署名格式一致。如果你的名字有多种写法（如 Li, X. 或 Li, Xiaorong），可以在 `first_name` 数组中添加多个变体：
```yaml
first_name: [Li, L.]
```

### 6. 导航栏标题改为大写 (`_sass/_base.scss`)
在 `_sass/_base.scss` 中找到 `.navbar` 相关样式（约第172行），添加 `text-transform`:
```scss
.navbar-nav .nav-item .nav-link {
  color: var(--global-text-color);
  text-transform: uppercase;  // 添加这行，标题变大写
  // ...
}
```

### 7. 删除 Blog 导航栏 (`_config.yml`)
在 `_config.yml` 中找到 `blog_nav_title`（约第104行），将其设为空：
```yaml
blog_nav_title:  # 留空即可隐藏 blog tab
```

---

## 改动如何生效

### 本地预览
```bash
eval "$(rbenv init - zsh)"
ruby -v  显示 3.3.0

# 安装依赖（首次）
bundle install

# 启动本地服务器
bundle exec jekyll serve
```
然后访问 `http://localhost:4000` 预览效果。

### 部署上线
```bash
git add .
git commit -m "更新内容描述"
git push origin master
```
推送后 GitHub Actions 会自动构建，约1-2分钟后访问 https://xiaorongli-95.github.io 即可看到更新。

### Git Push 认证失败解决方案

如果遇到错误：`Password authentication is not supported for Git operations`

**方法一：使用 SSH（推荐）**
1. 生成 SSH 密钥（如果没有）：
   ```bash
   ssh-keygen -t ed25519 -C "your_email@example.com"
   ```
2. 复制公钥：
   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```
3. 添加到 GitHub：Settings → SSH and GPG keys → New SSH key
4. 切换仓库为 SSH 地址：
   ```bash
   git remote set-url origin git@github.com:XiaorongLi-95/XiaorongLi-95.github.io.git
   ```
5. 再次 push：
   ```bash
   git push origin master
   ```

**方法二：使用 Personal Access Token**
1. GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic) → Generate new token
2. 勾选 `repo` 权限，设置过期时间，生成 token（**立即复制保存，只显示一次**）
3. push 时使用 token：
   ```bash
   # 方式A：直接在 URL 中使用（一次性）
   git push https://<你的用户名>:<你的token>@github.com/XiaorongLi-95/XiaorongLi-95.github.io.git master
   
   # 方式B：配置 credential 存储（推荐，避免每次输入）
   git config --global credential.helper store
   git push origin master
   # 首次会提示输入用户名和密码，用户名填 GitHub 用户名，密码填 token
   # 之后会自动记住
   ```

---

## 注意事项

1. 修改 `_config.yml` 后需要**重启本地服务器**才能看到效果
2. 图片/PDF等资源文件放在 `assets/` 对应目录下
3. 论文的 `selected={true}` 会让论文显示在首页"Selected Publications"
