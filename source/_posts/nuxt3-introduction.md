---
title: Nuxt3介紹
keywords: >-
  frontend, nuxt, nuxt3, nuxt introduction, nuxt3 introduction, nuxt簡介, nuxt3簡介,
  nuxt3 簡介, nuxt3 介紹, nuxt3介紹, nuxt介紹
tags:
  - Nuxt3
  - Vue
categories:
  - Frontend
  - Nuxt3
top_img: /images/default_banner.png
cover: /images/nuxt3-introduction/cover.png
abbrlink: 4b107bab
date: 2022-12-16 11:02:37
---

# Nuxt3 簡介

## Nuxt3 template / Nuxt3 starter

##### [一個自製的 nuxt3 template](https://github.com/ben890524/nuxt3-template)

- nuxt3 stable
- tailwindcss
- pinia & pinia-plugin-persistedstate
- i18n(@intlify/nuxt3) and store in pinia-persist
- simple template for page/layout/error

## Description(簡述)：

Nuxt3 是以 Vue3 為基礎，用來在單頁應用程式（SPA, Single Page Application）上進行伺服器渲染（SSR, Server Side Render）。專案架構打包工具支援 webpack 5 和 Vite，且使用 Vue-Router 來管理客戶端（Client Side）的路由。但目前版本還在釋出版本候補（RC, Release Candidate），代表還相對不穩定，現階段比較建議使用 Npm、Yarn，其他套件管理程式（Package Manager）的支援度還不夠，例如 PNpM。
2022/11/16，Nuxt3 已經在今天正式釋出[穩定版本](https://github.com/nuxt/framework/discussions/9064)！！！

### SSR V.S CSR

#### SSR

HTML 網頁資料會在伺服器端進行編譯，編譯完成才會回傳回客戶端，大多後端 MVC 架構的網頁皆是 SSR。

##### 優點

- 搜尋引擎優化（SEO）的排名更佳。
- 使用者進到的首屏（第一屏）速度較快，因為不用下載 JS 和 CSS 檔執行後才看到頁面。
- 性能不好的手機或是網路不好的區域，在首屏（第一屏）畫面渲染也比較快。

##### 缺點

- 伺服器端會有更大的壓力，因為部分程式碼會在伺服器端執行。

#### CSR

所有 HTML 網頁資料都交給客戶端進行渲染，會等到 JS 和 CSS 檔完成執行後才會顯示畫面，也些是依照 Router 去顯示的，也會等 API 資料回傳後才顯示。

##### 優點

- 減少伺服器端的負荷，因為所有都是在客戶端上進行渲染。

##### 缺點

- SEO 排名極差，因為一開始 HTNL 會是空白的，所以網頁爬蟲機器人會爬不到資料。
- 首屏頁面渲染速度慢，因為要等 JS 和 CSS 下載完後才會有畫面。

#### SSR&CSR 總結

一些內部管理系統，追求效率或是操作平凡的系統就比較推薦使用 CSR；
高度需要 SEO 的就推薦使用 SSR，像是媒體平台或是電商平台，這類型的就需要被搜尋引擎搜尋到，就需要 SSR。

### Nuxt 在 SSR 上的運作原理

因應 SSR，所以 Nuxt 在原理上，就是在 Server Side 先執行過一次後，再 Client Side 再執行一次，第一次的執行比較可以應對 SEO 或是搜尋引擎的爬蟲機器人，第二次就是給一般的使用者做正常的網頁使用。

## 用途：

將 SPA 和 SSR 的好處結合在一起。在首個屏幕的頁面渲染較快，且支援良好的 SEO，且之後採用客戶端渲染（CSR, Client Side Render）的方式執行。所以常常會被同構、Hybird、混合等等的形容詞形容。

## 準備專案

### 創建專案

```bash
npx nuxi init [nuxt-app]
cd [nuxt-app]
```

若途中有詢問你是否要安裝nuxt@3.0.0-rc.*，請按 Y。

### 安裝依賴

```bash
npm install
```

推薦使用 Npm、Yarn 套件管理程式，PNpM 現階段要加參數（--shamefully-hoist）才可正常 Install。

### 啟動開發伺服器

```bash
yarn dev
npm run dev
pnpm dev
```

推薦使用 Npm、Yarn 套件管理程式。也可以在`package.json`新增跟 vue3 一樣的啟動指令！

```json
// In package.json
"scripts": {
    ...
    "serve": "nuxt dev",
}
```

```bash
npm serve
yarn serve
pnpm serve
```

## Nuxt 的檔案架構（Directory Structure）

Nuxt 的檔案結構大致上如下，下面會依次來做簡單介紹：
![](/images/nuxt3-introduction/post_content_img_1.png)

### .nuxt

Nuxt 用這個資料夾裡面的 Script，在 Devlopment 時產生 Vue 的應用程式。
**_官方建議是完全都不要碰，不然會在執行 nuxt dev 時，將/.nuxt 目錄重新建立。_**
**~~想了解有關 Nuxt 根據目錄結構生成的更多信息，可以往這個資料夾裡面的東西找。~~**

### .output

當你跑 Script 為了建立 Production(或是其他測試環境)的應用程式時，產出的檔案會被放在這個資料夾。
**_官方建議是完全都不要碰，不然會在執行 nuxt dev 時，將/.nuxt 目錄重新建立。_**

### assets

可以放置網站所有的靜態資產，例如：階層樣式表(CSS)、字體樣式和圖片等等，專案架構打包工具(webpack 5 和 Vite)會處理到。

### components

用來放置 Vue Component 的地方，這些 Components 用來之後可以匯入進頁面(pages)裡面，且達成重複利用的效果。
具有 Auto Import Components 的功能，不需要再各個頁面進行 Import，但 Auto Import 有它的限制。
下面兩種都是在/components 底下，在頁面上皆可用 < Footer /> 的方式取出來。
![](/images/nuxt3-introduction/post_content_img_2.png)
![](/images/nuxt3-introduction/post_content_img_3.png)
若是是有用資料夾進行分類的，且分類底下還有更小的 component 的話，也有對應的 Auto Import 功能。
![](/images/nuxt3-introduction/post_content_img_4.png)
在 header 裡面的 SlideMenu 就可以用 header-slide-menu 或 HeaderSlideMenu 進行使用。
![](/images/nuxt3-introduction/post_content_img_5.png)
目前還是推薦使用自己 Import 進來的命名的方式，去 Import 各 Component，因為這樣編輯器(Visual Studio Code)可以追蹤的到，以開發維護的角度來看，比較方便。

### composables

用來放置 Vue Composables，這些 Composables 用來之後可以在任何頁面進行使用、運算。
且也支援 Auto Import Composables。
在/composables 裡定義好，在 script setup 裡面，就可以直接使用你定義好的 composables。
![](/images/nuxt3-introduction/post_content_img_6.png)
![](/images/nuxt3-introduction/post_content_img_7.png)
一般的命名還是會以 Vue3 一貫的 Composition Api 的方式去命名，以 use 為開頭！

### content

Nuxt 的 Content 會讀取 content/目錄並解析.md、.yml、.csv 和.json 的文件，後產生以檔案為基礎的 CMS。

### layouts

Nuxt 提供了一個可自由制定的佈局框架，可以在整個應用程式中使用，非常適合將常見的可重用的佈局組件中，就可以迅速的在不同的內容頁面套用不同的樣板。

```typescript
// 透過這樣的方式可以替換layout
definePageMeta({
  layout: 'custom',
});
```

或是在 Vue template 裡面用 NuxtLayout 來做呈現：

```html
// 透過這樣的方式指定layout
<template>
  <NuxtLayout name="layout-name"> </NuxtLayout>
</template>
```

### middleware

Nuxt 提供一個可定制的路由中間件框架，就是 Middleware，可以在整個應用程序中使用，非常適合在轉導時，之前使用 Middleware 進行一些額外的操作。
實作方面可以參考：

- [路由守衛（Navigation Guards）](https://book.vue.tw/CH4/4-4-navigation-guards.html)
- [[Day 5] 認識 Nuxt3 專案結構 - middleware](https://ithelp.ithome.com.tw/articles/10295114)

#### 第一種中介層：全域的中介層

在 middleware 資料夾裡，有任何命名內有.global 的檔案(.ts file)，都會被當成全域的中介層，在每一個 route 時都會執行。

```typescript
// index.global.ts
export default defineNuxtPlugin(() => {
  addRouteMiddleware('global',() => {
      ...something;
    },{ global: true }
  );
});
```

#### 第二種中介層：指定的中介層

可以為你想要的頁面客製化 middleware，然後在頁面 setup 的區塊註冊 middleware。

```typescript
// xxx.ts
export default defineNuxtRouteMiddleware(async (_to, _from) => {});
```

```typescript
// account.vue
<script setup lang="ts">
    definePageMeta({
      middleware: ['account'],
    });
</script>
```

### node_modules

套件管理程式（Npm、Yarn 和 PNpM 等等）會創建/node_modules，用來存儲此專案所有的依賴套件。

### pages

Nuxt 依照 pages 資料夾目錄下面的內容，使用 Vue-Router 創建出來，所以資料夾的路徑即是網址的路徑，有一點像靜態路由。

### plugins

Nuxt 提供自動讀取 plugins 資料夾中的檔案，並在執行的時候自動加載。在 plugin 裡面的檔案要使用**defineNuxtPlugin**去定義讓 nuxtapp 使用。也可以在檔案名中增加.server 或.client 的後綴命名，讓 plugins 只在伺服器端或是客戶端中被執行。
使用 Nuxt 內建的 NuxtLink 進行跳轉的時候，Nuxt 的頁面是不會幫你把頁面跳到置頂與置左的，所以就可以透過設定 plugin 的方式解決：

```typescript
// router.global.ts
export default defineNuxtPlugin((nuxtApp) => {
  nuxtApp.hook('page:finish', () => {
    setTimeout(() => window.scrollTo({ top: 0, left: 0 }), 300);
  });
});
```

因為 plugin 會自動載入，所以這樣就可以達成所有的 Page 結束的同時，頁面的位置會幫你置頂置左。

### public

public/資料夾下直接在服務器根目錄提供必須保留其名稱（例如 robots.txt）或可能不會更改（例如 favicon.ico）的公共檔案。

### server

因為 Nuxt 是運行在伺服器端上的，所以可以有一些後端的操作，就會放在這個資料夾裡面。但還是希望說前後端分離的架構上，可以依照權責分工，除了一些特殊的需求。
主要會有下面幾個操作：

- ~/server/api
- ~/server/routes
- ~/server/middleware

### .gitignore

.gitignore 文件定義了 git 要忽略或是不追蹤的檔案。可以去到 git 官網看更多。

### .nuxtignore

Nuxt 提供了.nuxtignore 讓構建階段（Build Phase）時忽略跟目錄中的佈局（layouts）、頁面（pages）、元件（components）、（composables）和（middleware）中介層文件。

### app.config.ts

Nuxt 3 提供了 app.config 的配置文件來控制響應式配置，並能夠在生命週期內運行時更新，或者使用 Nuxt 插件並使用 HMR 熱更新（Hot Module Replacement）對其進行改變後馬上更新。

> Hot Module Replacement，就是在 Server 與瀏覽器建立了一個 websocket 連線，當程式碼被修改時，Server 會傳送訊息通知瀏覽器去請求修改模組的程式碼，完成熱更新，所以在這樣的情況下就能在不刷新瀏覽器的前提下進行更新。HMR 具有以下優點：
>
> 1. 修改程式碼時，可以即時更新畫面
> 1. 實現部分跟新，避免多餘請求
> 1. 保有原本狀態

### app.vue

這是 Nuxt 應用程式中，最主要的原件。

### nuxt.config.ts

Nuxt 提供 nuxt.config 檔案可以快速配置具有.js、.ts 或.mjs 等副檔名的檔案。

### package.json

package.json 檔案包含此 Nuxt 應用程式的所有依賴的套件、腳本，和其的版本。

### tsconfig.json

此檔案是.nuxt/tsconfig.json 的擴充檔，可以依照專案的需求，對需要的設定進行客製化的調整。

###### [HackMD 網址](https://hackmd.io/VZaRGDuXS8avfcF0g62qpg)

###### tags: `Nuxt3` `SSR`
