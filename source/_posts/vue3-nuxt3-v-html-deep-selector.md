---
title: Vue3 v-html / Nuxt3 v-html / css深度選擇器
keywords: >-
  frontend, nuxt, nuxt3, Nuxt3 v-html, nuxt3 v-html, vue3 v-html, Vue3 v-html,
  Vue v-html, vue v-html, css深度選擇器, css 深度選擇器, 深度選擇器, deep selector
tags:
  - Frontend
  - Vue
  - Nuxt3
  - CSS
categories:
  - Frontend
  - Nuxt3
top_img: /images/default_banner.png
cover: /images/vue3-nuxt3-v-html-deep-selector/cover.png
abbrlink: c035b82a
date: 2023-01-12 16:11:17
---

# Vue3 v-html / Nuxt3 v-html / Css 深度選擇器

## Vue v-html

最近在接公司前後台資訊，其中在編輯商品時，是使用[CKEditor5](https://ckeditor.com/ckeditor-5/)這個套件進行編輯的，存在資料庫的內容就會是 HTML 標籤語言，如下：

```HTML
'<p><a href="/">商品描述商品描述</a></p>'
```

在做 Vue 前端時，就不能使用`{{}}`大括號在模板語言中直接使用，因為字串中包含實體字符，至於關於實體字符可以參考[這篇](https://ithelp.ithome.com.tw/articles/10202232#v-html)。

那這邊馬上來實際運用一下：
父元件會傳 productDetail 這個 props，定義好 props 及預設值後，我用一個 computed 先進行處理，CKEditor5 在嵌入 iframe 影片時，要做一些處理，因為 CKEditor5 轉換出來的 HTML 在前端顯示不出來。
那`data_processEmbedTag`就是處理後的字串。

```Typescript
<script setup lang="ts">
interface IProps {
  productDetail?: string;
}
const props = withDefaults(defineProps<IProps>(), {
  productDetail: '',
});
console.log(props.productDetail);
// translate ck editor5 youtube iframe
const data_processEmbedTag = computed(
  () =>
    props.productDetail?.replaceAll('oembed', 'iframe').replaceAll('url', 'src').replaceAll('watch?v=', 'embed/') ?? ''
);
</script>
```

接下來就可以使用`v-html`綁進去 vue 模板語言裡面的 DOM 元素了：

```HTML
<template>
  <div class="flex flex-col">
    <div v-html="data_processEmbedTag"></div>
  </div>
</template>
```

這是`data_processEmbedTag`的內容：
![](/images/vue3-nuxt3-v-html-deep-selector/post_content_img_1.png)
這是`v-html`呈現的內容：
![](/images/vue3-nuxt3-v-html-deep-selector/post_content_img_2.png)

綁進去後你會發現，v-html 裡面所有的元素都吃不到 css，只有存進去資料庫的 inline style 會作用而已。所以這時候你就要使用 vue 的 css 的深度選擇器。

## Vue 的 css 的深度選擇器

先說說為什麼要使用到深度選擇器：
Vue 在選染模板語言時，都會賦予元素一個 Hash 屬性，所以在瀏覽器的開發人員工具(F12)中常常會看到`data-v-hash`的元素，但是使用`v-html`渲染出來的不會有這樣的 Hash 屬性。
再來因為 Vue 在寫 css 樣式，可以使用關鍵字`<style scoped>`，可是使你在子物件裡定義的樣式，不會向外汙染全域的樣式，所以在正常情況會使用`scoped`關鍵字。但這時你寫了一個 style，就會只渲染在同一個 Hash 的原件，沒有 Hash 的是吃不到你設定的樣式的，例如：`v-html`產生出來的 DOM 元素。
元件在開發人員工具呈現的樣子：

```HTML
<div data-v-bc5f04ec="">...</div>
```

那冠有`scoped`的樣式就會這樣呈現：

```css
table[data-v-bc5f04ec],
th[data-v-bc5f04ec],
td[data-v-bc5f04ec] {
  @apply border-1 border-border-color;
}
```

所以這樣是沒有辦法發揮樣式的作用的。所以就套上深度選擇器吧：

```css
// deep selector change v-html style
:deep(*) {
  @apply text-sm tracking-wide;
  table,
  th,
  td {
    @apply border-1 border-border-color;
  }
}
:deep(figure.media) {
  iframe {
    @apply w-full;
    height: var(--global-iframe-height);
  }
}
:deep(iframe) {
  @apply w-full;
  height: var(--global-iframe-height);
}
```

套用上深度選擇器後，就可以正常呈現了：
![](/images/vue3-nuxt3-v-html-deep-selector/post_content_img_3.png)

### 深度選擇器其他寫法

深度選擇器其實還有其他寫法：
寫法一：`::v-deep`

```
::v-deep table,
::v-deep th,
::v-deep td {
  @apply border-1 border-border-color;
}
```

這種寫法在 Vue3 會被警告：

```bash
[@vue/compiler-sfc] ::v-deep usage as a combinator has been deprecated. Use :deep(<inner-selector>) instead.
```

寫法二：`>>>`

```
>>> table,
>>> th,
>>> td {
  @apply border-1 border-border-color;
}
```

寫法三：`/deep/`

```
/deep/ table,
/deep/ th,
/deep/ td {
  @apply border-1 border-border-color;
}
```

寫法二、三寫法在 Vue3 也會被警告：

```bash
[@vue/compiler-sfc] the >>> and /deep/ combinators have been deprecated. Use :deep() instead.
```

#### 結論

所以是 Vue3 的狀態下，還是使用`:deep(<inner-selector>)`的寫法吧！
