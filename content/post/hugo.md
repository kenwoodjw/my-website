---
title: "Hugo"
date: 2019-03-24T20:52:13+08:00
draft: true
---
![hugo-1](/images/hugo/hugo-1.png)

第一次搭建個人網站，選擇了 Hugo。儘管[官方文件](https://gohugo.io/documentation/)寫得很詳細，但 Programming 新手如我在建立的過程中還是遇到了一些困難，因此紀錄一下，希望能幫到跟我同樣情況的人。

以下分為兩部分介紹：本地建立檔案、部署到 GitHub。(使用 MacOS)

## 第一部分：本地建立檔案

老實說 Hugo 真的很人性化，簡單幾個步驟就能讓使用者建立網站的雛形。

### 1. 打開 terminal，安裝 Homebrew

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### 2. 安裝 Hugo

```
brew install hugo
```

### 3. 建立新網站

```
hugo new site website-hugo
cd website-hugo
```

`website-hugo` 可以替換成任意名稱，但你也可以跟我一樣，避免麻煩。找到名為 `website-hugo` 的資料夾，觀察一下內部資料夾結構。

### 4. 新增主題 [(theme)](https://themes.gohugo.io)：此處以 [Casper](https://themes.gohugo.io/casper/) 為例

```
git clone https://github.com/vjeantet/hugo-theme-casper themes/casper
```

你也可以選其他主題，進到該主題的 GitHub repo，將上面的網址改成 repo 的網址、`themes/casper` 改成 `themes/你的主題名稱`。

### 5. 將 /themes/casper 中的 static 和 layouts 資料夾複製，取代根目錄中的 static 和 layouts 資料夾

### 6. 建立新文章

```
hugo new posts/my-first-post.md
```

此指令會在 `/content/posts` 資料夾中建立 `my-first-post.md`。使用任意文字編輯器打開此 markdown 文件，將 draft 改成 false，文件內容任意。以後建立文章都是以此方式。

使用文字編輯器打開 `config.toml`，並調整內容為上面這樣。

### 7. 編輯 config.toml

```
baseURL = "https://chswei.github.io/"    #改成你的GitHub帳號名稱
languageCode = "zh-tw"
title = "Chswei Blog"    #自由命名
```

### 8. 本地測試

```
hugo server -D
```

執行完後，在瀏覽器中輸入網址 http://localhost:1313/ ，就可以看到網站的雛形了！

## 第二部分：部署到 GitHub

[Hugo 部署在 GitHub 的方式](https://gohugo.io/hosting-and-deployment/hosting-on-github/)有兩種，這邊介紹部署個人唯一主頁的方式。

### 1. 在 GitHub 建立兩個 repo

在 GitHub 建立 `website-hugo` 和 `chswei.github.io` (改成自己的帳號名稱) 兩個 repo。

### 2. 建立 /public 資料夾

```
hugo
```

執行 `hugo`，就會在 `website-hugo` 內產生名為 `public` 的資料夾。

### 3. 將 /public 資料夾連結到 GitHub 上的 chswei.github.io

```
cd public
git init
git remote add origin https://github.com/chswei/chswei.github.io.git
git add .
git commit -m "Initial commit"
git push -u origin master
```

### 4. 將整個 website-hugo 資料夾連結到 GitHub 上的 website-hugo

```
cd ../
git init
git remote add origin https://github.com/chswei/website-hugo.git
git add .
git commit -m "Initial commit"
git push -u origin master
```

在執行 `git add .` 時出現如下面的 warning 時不用在意

![hugo-2](/images/hugo/hugo-2.png)

等個幾分鐘，輸入網址 https://chswei.github.io/ 就大功告成囉！

![hugo-3](/images/hugo/hugo-3.png)

### References:

1. [Hugo Documentation](https://gohugo.io/documentation/)
2. [用Hugo搭建个人网站](https://brent-li.github.io/post/build-personal-site-with-hugo/)
3. [Github个人网站维护](https://chengjunwang.com/note/note_archive/2016-08-03-github/)
