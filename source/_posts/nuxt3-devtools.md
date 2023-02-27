---
title: nuxt3 開發者工具
keywords: >-
  frontend, nuxt, nuxt3, nuxt devtools, nuxt3 devtools, nuxt 開發者工具, nuxt3開發者工具,
  nuxt3 開發者工具, nuxt3開發者工具
tags:
  - Nuxt3
  - Vue
categories:
  - Frontend
  - Nuxt3
top_img: /images/default_banner.png
cover: /images/nuxt3-introduction/cover.png
date: 2023-02-27 15:31:12
# id: post-9
---

# Nuxt3 開發者工具 / Nuxt3-Devtools

## 前言

就在二月上旬，Nuxt 大佬 [antfu (Anthony Fu)](https://github.com/antfu)，發佈了[Nuxt3 開發者工具（nuxt-devtools）](https://github.com/nuxt/devtools)，可以讓我們這些 Nuxt 開發者，在開發中有更好的體驗和及時的追蹤。

## 版本限制

`nuxt-devtools`釋出前，nuxt 發行正式版一段時間了，所以`nuxt-devtools`有一些版本的限制：

- Nuxt DevTools requires Nuxt v3.1.0 or higher.

只要是在 Nuxt v3.1.0 以上的版本，皆可以安裝，也可以直接進行 Nuxt 更新，更新到最新的版本。
若要更新，可以使用 Nuxt 內建的函式庫 Nuxi 來更新。

```BASH
npx nuxi upgrade
```

## 安裝方式

### 簡短介紹

`nuxt-devtools`官方文件有提供兩個方式安裝，這邊簡單的用條列式，分析一下兩個方式的差異：

1. 使用 nuxi 指令依據各 Project 啟動
   1.1 使用 nuxi 指令就可以開啟
   1.2 是依照 Project 來執行 nuxt-devtools 的，不會因為開啟後，所有 nuxt3 Project 都開啟
   1.3 設定會儲存在本地的資料夾`~/.nuxtrc`裡，不會上傳到儲存庫，影響到整體專案
2. 手動進行安裝執行
   2.1 使用 npm 等指令安裝
   2.2 安裝後在 nuxt config 導入即可
   2.3 有更多控制選項可以定義在 nuxt config
   2.4 會上傳到儲存庫，所以會影響整個專案

### 使用 nuxi 指令

```BASH
npx nuxi@latest devtools enable
```

這樣一句簡短的指令，就可以讓`nuxt-devtools`啟用在你的 Project 了！
當然，當你不需要或是其他狀況時，可以停用他：

```BASH
npx nuxi@latest devtools disable
```

### 手動進行安裝執行

先從 NPM 安裝：

```BASH
npm i -D @nuxt/devtools
```

再導入至`nuxt.config.ts`：

```Typescript nuxt.config.ts
export default defineNuxtConfig({
  modules: [
    '@nuxt/devtools',
  ],
})
```

這樣就算是安裝炳在 Nuxt Project 中啟用`nuxt-devtools`了！
當然，他有更多可選的的配置可以進行設置：

```Typescript nuxt.config.ts
export default defineNuxtConfig({
  modules: [
    '@nuxt/devtools',
  ],
  devtools: {
    // Enable devtools (default: true)
    enabled: true,
    // VS Code Server options
    vscode: {},
    // ...other options
  }
})
```

兩種方式只要在 Nuxt 專案啟動後，在頁面中間下面看到一個 Nuxt 小 icon 就代表啟用/安裝成功囉！

![](/images/nuxt3-devtools/post_content_img_1.png)

## 特色

這邊舉例幾個很有意思的功能來做介紹：

### Overview

在`Overview`中，可以即時的看到目前 Nuxt 的版本、頁面（Pages）總數、元件（Components）總數、引入的 Composables 總數、使用的 Module 總數和 Nuxt plugin 總數，總體啽言，就是一個快速瀏覽的 DashBoard。

![](/images/nuxt3-devtools/post_content_img_2.png)

其中一個小發現，當你的 Nuxt 版本不是最新的時候，點 Nuxt 的版本的欄位，他會幫你到 VS Code 的終端機幫你下`npx nuxi upgrade`的更新指令，挺方便的吧！就不用定期到[npm/nuxt](https://www.npmjs.com/package/nuxt)看有沒有更新了。

### Pages

在`Pages`中，可以快速瀏覽到目前所有的 page，還可以知道現在 route 指向哪一個頁面，還有各頁面套用到哪一個`layout`。

![](/images/nuxt3-devtools/post_content_img_3.png)

最上面還有一個輸入框，可以更改現在的路徑，下面有一個區塊會即時顯示與你輸入的路徑有匹配到的路徑，輸入完成後 Enter 按下，就會立即跳到該路徑。

![](/images/nuxt3-devtools/post_content_img_4.png)

### Components

在`Components`中，可以看到該 Nuxt Project 中，所有被定義的 Components，且還有根據來源/用途進行分類。

![](/images/nuxt3-devtools/post_content_img_5.png)
Components 的分類有：

- User Components 使用者定義的元件
- Runtime Components 執行期引入的元件
- Built-in Components Nuxt 內建提供的元件
- Components from libraries 從 Library 裡面引入的元件

### Imports

在`Imports`中，可以看到該 Nuxt Project 中，有用到的 Composables，也有根據來源/用途進行分類。

- User Components 使用者定義的元件，要放在`./composables`的目錄裡。
- Runtime Components 執行期引入的元件
- Built-in Components Nuxt 內建提供的元件

會立即顯示你使用到哪些及使用次數，典籍使用處後，也會立即幫你在編輯器種顯示該檔案。

![](/images/nuxt3-devtools/post_content_img_6.png)

### Modules

在`Modules`中，會看到現在這個 Project 中有用到`nuxt.com/modules`裡的 Modules，可以簡單的看到大綱：

![](/images/nuxt3-devtools/post_content_img_7.png)

### Plugins

`Plugins`中列出你在`defineNuxtPlugin`或`./plugins`資料夾中，定義的所有`Plugins`：

![](/images/nuxt3-devtools/post_content_img_8.png)

### 小節

以上就是我覺得比較常用的功能，還有很多功能還沒有摸索，畢竟也才剛釋出，還有很多功能可以嘗試，而且`nuxt-devtools`也還在持續更新。

## 結論

`nuxt-devtools`真的很實用，可以快速追蹤 components/composables 等等的使用情況，方常方便、增加開發速度。雖然是選配的工具，但滿推薦開發時就搭配起來，他幫你彙整的資訊，也很清楚，在開發上也很有幫助！

在[nuxt-devtools - discussions](https://github.com/nuxt/devtools/discussions)中，也有**Ideas Collection**和**Roadmap**，可以討論，或是你有建議也可以提出！！！
