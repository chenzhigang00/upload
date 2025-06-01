---
title: Hexo 博客搭建
date: 2025-05-08 10:46:00
tags: 个人网站
#categories: 个人网站
---

### 部署

准备：linux环境

1. 第一步，`apt install` 下载`node`, `npm`, `git ` (若后续因为node、npm版本问题报错，则利用copilot提示下载新版本即可)

2. 第二步，连接Github

   ```bash
   git config --global user.name "GitHub 用户名"
   git config --global user.email "GitHub 邮箱"
   ```

   Github账户上`Setting-SSH and GPG keys`上放`pub`公钥文件，本地`root/.ssh`下要有私钥文件

   - `Git bash`下输入`ssh -T git@github.com`, 输入yes

3. 第三步创建Github Page仓库，到github上创建`用户名.github.io`仓库

4. 本地安装Hexo博客程序

   `npm install -g hexo-cli` 这一步可能出错，是因为npm，node版本问题，用报错信息问问ai应该下载哪个版本的

5. Hexo 初始化和本地预览

   ```bash
   hexo init      # 初始化
   npm install    # 安装组件
   ```

   访问`http://localhost:4000`

6. 部署Hexo到Github Pages`ynpm install hexo-deployer-git --save`

   **要求在空文件夹下按照，以后有关数据都在这个文件夹下操作**

   修改`.cofig,yml`文件末尾的`Deployment`部分

   ```yaml
   deploy:
     type: git
     repository: git@github.com:用户名/用户名.github.io.git
     branch: main
   ```

   - 运行`hexo d`  

   成功啦， 网址输入github.io网址康康（有一定延时，更换浏览器绕开缓存影响）

### 使用

#### 创建文章

- 第一种使用方式：命令创建新的markdown，后编辑

  ```
  hexo new "My New Post"
  ```

  - 在source文件夹下出现`My New Post.md`文件

- 第二种使用方式：修改已有的markdown文件

  - 在开头添加如下信息

    ```markdown
    ---
    title: Hello World # 标题
    date: 2019/3/26 hh:mm:ss # 时间
    categories: # 分类
    - Diary
    tags: # 标签
    - PS3
    - Games
    ---
    
    摘要
    <!--more-->
    正文
    ```

#### 发布文章

```bash
hexo g
hexo d
```

#### 网站设置

包括网站名称、描述、作者、链接样式等，全部在网站目录下的 `_config.yml`

#### 更换主题

```bash
git clone https://github.com/iissnan/hexo-theme-next themes/next
```

后在`config.yml`文件中设置

### 常用命令

```bash
hexo new "name"       # 新建文章
hexo new page "name"  # 新建页面
hexo g                # 生成页面
hexo d                # 部署
hexo g -d             # 生成页面并部署
hexo s                # 本地预览
hexo clean            # 清除缓存和已生成的静态文件
hexo help             # 帮助
```





[使用 Hexo+GitHub 搭建个人免费博客教程（小白向） - 知乎](https://zhuanlan.zhihu.com/p/60578464)







