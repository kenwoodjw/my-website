---
title: "Hugo"
date: 2019-03-24T20:52:13+08:00
draft: false
---


### 1. 安裝 Hugo

```
brew install hugo
```

### 2. 建立新网站

```
hugo new site my-website
cd my-website
```

`my-website` 可以替换成任意名称，但你也可以跟我一样，避免麻煩。找到名为 `my-website` 的文件夾，观察文件夹结构。

### 3. 新增主題 [(theme)](https://themes.gohugo.io)：此处以 [Casper](https://themes.gohugo.io/casper/) 为例

```
git clone https://github.com/vjeantet/hugo-theme-casper themes/casper
```

你也可以选其他主題，进到该主題的 GitHub repo，將上面的网址改成 repo 的网址、`themes/casper` 改成 `themes/你的主題名称`。

### 4. 將 /themes/casper 中的 static 和 layouts 文件夹复制，取代根目录中的 static 和 layouts 文件夹

### 5. 建立新文章

```
hugo new posts/my-first-post.md
```

此指令会在 `/content/posts` 文件夾中建立 `my-first-post.md`。使用任意文字编輯器打开此 markdown 文件，將 draft 改成 false，文件內容任意。以后建立文章都是以此方式。

使用文字编輯器打开 `config.toml`，并调整內容为上面这样。

### 6. 编輯 config.toml

```
baseURL = "https://kenwoodjw.github.io/"    #改成你的GitHub帳號名稱
languageCode = "zh-cn"
title = "kenwood Blog"    #自由命名
```

### 7. 本地测试

```
hugo server -D
```

执行完后，在浏览器中輸入网址 http://localhost:1313/ ，就可以看到网站的雏形了！

## 第二部分：部署到 GitHub

[Hugo 部署在 GitHub 的方式](https://gohugo.io/hosting-and-deployment/hosting-on-github/)有两种，这边介绍部署个人唯一主页的方式。

### 1. 在 GitHub 建立两个 repo

在 GitHub 建立 `my-website` 和 `kenwoodjw.github.io` (改成自己的帐号名称) 兩個 repo。

### 2. 建立 /public 文件夹

```
hugo
```

执行 `hugo`，就会在 `my-website` 內产生名为 `public` 的文件夾。

### 3. 将 /public 文件夾连接到 GitHub 上的 kenwoodjw.github.io

```
cd public
git init
git remote add origin https://github.com/chswei/kenwoodjw.github.io.git
git add .
git commit -m "Initial commit"
git push -u origin master
```

### 4. 将整个 my-website文件夹连接到 GitHub 上的 website-hugo

```
cd ../
git init
git remote add origin https://github.com/kenwood/my-website.git
git add .
git commit -m "Initial commit"
git push -u origin master
```

在执行 `git add .` 时出现如下面的 warning 时不用在意



等个几分钟，输入网址 https://kenwoodjw.github.io/ 就大功告成！



### References:

1. [Hugo Documentation](https://gohugo.io/documentation/)
2. [用Hugo搭建个人网站](https://brent-li.github.io/post/build-personal-site-with-hugo/)
3. [Github个人网站维护](https://chengjunwang.com/note/note_archive/2016-08-03-github/)
