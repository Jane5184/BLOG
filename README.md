Markdown

# Django 个人博客系统 (Blog Project)

这是一个基于 Python Django 框架开发的个人博客管理系统。系统实现了完整的博文发布、图片上传、用户认证以及分级权限管理功能。

## 📂 项目目录结构说明

根据项目目录树，核心结构如下：
- **Blog/**: 项目全局配置目录，包含 `settings.py` (全局设置) 和 `urls.py` (总路由)。
- **blogs/**: 核心业务应用目录。
  - `models.py`: 定义博文实体（标题、内容、图片、作者）。
  - `views.py`: 处理业务逻辑（CRUD 操作、权限校验）。
  - `forms.py`: 处理博文发布与编辑的表单校验。
  - `templates/`: 存放 HTML 模板，采用母版继承机制。
- **media/**: 存放用户上传的博文配图。
- **db.sqlite3**: 轻量级关系型数据库，存储所有结构化数据。

---

## 🛠️ 系统逻辑架构

项目采用 **MVT (Model-View-Template)** 架构模式：

```mermaid
graph TD
    User((用户)) -->|请求| URL[blogs/urls.py]
    URL -->|路由调度| View[blogs/views.py]
    
    subgraph 逻辑处理层
        View -->|数据校验| Form[blogs/forms.py]
        View -->|权限检查| Auth{is_owner / is_admin?}
    end
    
    subgraph 数据持久化
        Auth -->|操作| Model[blogs/models.py]
        Model <--> DB[(db.sqlite3)]
    end
    
    subgraph 表现层
        View -->|渲染| Template[HTML Templates]
        Template -->|返回| User
        Template -.-> Media[media/ images]
    end
✨ 核心功能特性
1. 用户认证与权限 (Authentication & Authorization)
注册与登录：通过 django.contrib.auth 实现用户认证。

水平权限控制：普通用户登录后仅能编辑或删除属于自己的博文 (post.owner == request.user)。

垂直权限控制：超级用户 (Superuser) 拥有最高管理权限，可删除系统内任意用户的博文。

2. 博文管理 (CRUD)
发布 (Create)：支持文字内容与图片附件上传。

展示 (Read)：首页按时间倒序排列展示博文列表。

编辑 (Update)：作者或管理员可修正已发布的内容。

删除 (Delete)：集成 JavaScript 二次确认逻辑，安全执行数据清理。

3. 静态与媒体资源管理
系统实现了图片资产的物理存储（media/blog_images/）与数据库路径索引的分离，确保了高效的资源读取。

🚀 快速开始
环境准备: 确保已激活虚拟环境 ll_env。

Bash

# Windows
ll_env\Scripts\activate
启动服务器:

Bash

python manage.py runserver
访问系统:

首页: http://127.0.0.1:8000/

管理后台: http://127.0.0.1:8000/admin/

📝 实验总结
本项目通过 Django 框架展示了 Web 开发中核心的“数据-逻辑-视图”分离思想。通过自定义视图函数中的权限判定逻辑，实现了符合实际业务需求的层级化管理功能。# BLOG
