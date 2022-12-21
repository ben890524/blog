---
title: Vue 實作 Google reCAPTCHA / Google 非機器人驗證
date: 2022-12-21 13:58:25
tags:
  - Frontend
  - Vue
  - Google reCAPTCHA
categories:
  - Frontend
top_img: /images/default_banner.png
cover: /images/vue-google-recaptcha/cover.png
# id: post-7
---

# Vue 實作 Google reCAPTCHA / Google 非機器人驗證

## 前言

因為工作上的需要，所以要來研究 Google reCAPTCHA，公司是用 Vue，所以會來用 Vue 來實做看看，後續看看能不能整合進去公司的 Nuxt3 的前台。~最下面有本次實作的範例檔案，有需要可以直接取 XD~

## Google reCAPTCHA

因為網站安全性問題，或是要防止機器人來到你的網站，佔用你的伺服器空間，所以很多網站都會連接 Google reCAPTCHA，來讓 Google 幫你檢驗，當前使用者是不是機器人~
這功能應該很常見，想必大家一定有看過一個勾勾旁邊是我不是機器人，或是一個九宮格請你選取題目指示的圖片，這些都是 Google reCAPTCHA 的實作。
Google reCAPTCHA 主要有以下幾種：

1. google reCAPTCHA v2
   - ![](/images/vue-google-recaptcha/post_content_img_1.png)
   - 會有一個核取框，問你是不是機器人，在 google 的檢查機制下如果覺得你是機器人，就會出現九宮格選取框，再來依照題目的要求選取即可。
2. google reCAPTCHA v3
   - 全在背景執行，google 會給當前使用者進行評分，從 0.0~1.0，開發者可以自行決定，多少分以下視為機器人！
3. google reCAPTCHA 企業版
   - 目前據了解，會有更多管理上的功能，且也不會有驗證次數上的限制，因為**reCAPTCHA v2 v3 有每個月一百萬次**的上限！

## 註冊 Google reCAPTCHA

先到[Google reCAPTCHA](https://www.google.com/recaptcha/about/)，選取紅色框框的`v3 Admin Console`。

![](/images/vue-google-recaptcha/post_content_img_2.png)

接下來填入一些基本的資料進行註冊吧！有幾點可以注意：

- reCAPTCHA v2 v3 依自己的需求使用，如果都需要則要分為兩個進行創立
- 網站可以填入 127.0.0.1 和 localhost，如有需要可以填自己區網的 ip

![](/images/vue-google-recaptcha/post_content_img_3.png)

提交後，就可以看到金鑰了，可以不用複製，後續進到 Google reCAPTCHA 的站台還是可以看到~在前端(Vue)會使用到的都只會是`網站金鑰`！

![](/images/vue-google-recaptcha/post_content_img_4.png)

那 Google reCAPTCHA 的註冊前置的部分就到這邊，可以去準備 Vue 的環境了，這邊 reCAPTCHA v2 v3 都會實作，所以要去註冊兩個~~~

![](/images/vue-google-recaptcha/post_content_img_5.png)

## 建立 Vue 專案

那現在就先簡單的建立 Vue3 Template：

- 使用 Vue3
- Vite
- Typescript

可以使用你喜歡的套件管理器，以下範例皆使用 pnpm。

```bash
pnpm create vite@latest vue3-recaptcha -- --template vue-ts
cd vue3-recaptcha-example
pnpm install
pnpm run dev
```

先檢測看看是否能正常啟動~

## 實作 reCAPTCHA v2

可以正常啟動後，先下載套件[vue3-recaptcha2](https://www.npmjs.com/package/vue3-recaptcha2)：

```bash
pnpm install vue3-recaptcha2
```

然後建立 GoogleReCaptchaV2.vue 進行 import，這邊簡單的建立，可以依需求 import 在各個地方！

![](/images/vue-google-recaptcha/post_content_img_6.png)

接下來來實作 GoogleReCaptchaV2.vue：
我習慣將相關的方法用類似物件的方式封裝關聯起來：

```Typescript GoogleReCaptchaV2.vue
<script setup lang="ts">
import { reactive } from 'vue';
import vueRecaptcha from 'vue3-recaptcha2';
const instance_vueRecaptchaV2 = reactive({
  // 請換成你註冊的 SiteKey
  // Please change to your SiteKey.
  data_v2SiteKey: '*****************************',
  recaptchaVerified: (response_token: string) => {
    console.log(response_token);
    // 連接後端API，給後端進行認證
    // Connect to your Backend service.
  },
  recaptchaExpired: () => {
    // 驗證過期後進行的動作
    // After recaptcha is expired, the action you can do.
    console.log('驗證過期啦QAQ');
  },
  recaptchaFailed: () => {
    // 驗證失敗進行的動作
    // After recaptcha is failed, the action you can do.
  },
});
</script>
```

```HTML GoogleReCaptchaV2.vue
<template>
  <vue-recaptcha
    :sitekey="instance_vueRecaptchaV2.data_v2SiteKey"
    size="normal"
    theme="light"
    hl="zh-TW"
    @verify="instance_vueRecaptchaV2.recaptchaVerified"
    @expire="instance_vueRecaptchaV2.recaptchaExpired"
    @fail="instance_vueRecaptchaV2.recaptchaFailed"
  />
</template>
```

接下來在頁面上就可以看到，目前沒有調整樣式，可以依照需求去調整~
可以在 Component 階段用一個 div 包著，import 後進行排版！

![](/images/vue-google-recaptcha/post_content_img_7.png)

可以簡單測試看看，點擊我不是機器人後，核取筐會打勾，且在開發人員的 Console 也有印出這次 reCAPTCHA 的 Token，再去連接後端進行認證即可~

![](/images/vue-google-recaptcha/post_content_img_8.png)

當驗證過期時，功能也都正常，就可以依照需求進行操作啦！

![](/images/vue-google-recaptcha/post_content_img_9.png)

那到這邊，就已經完成了 Google reCAPTCHA v2 的 Vue 實作了！

## 實作 reCAPTCHA v3

接下來要實作 reCAPTCHA v3，先下載套件[vue-recaptcha-v3](https://www.npmjs.com/package/vue-recaptcha-v3)：

```bash
pnpm install vue-recaptcha-v3
```

再來到 main.ts 對 vue-recaptcha-v3 進行註冊，其中[loaderOptions](https://github.com/AurityLab/recaptcha-v3/#load-options-usage)可以在 vue-recaptcha-v3 套件說明這邊依照自己的需求選取。

```Typescript main.ts
import { createApp } from 'vue';
import './style.css';
import App from './App.vue';
import { VueReCaptcha } from 'vue-recaptcha-v3';

const app = createApp(App);
app.use(VueReCaptcha, {
  // 請換成你註冊的 SiteKey
  // Please change to your SiteKey.
  siteKey: '*****************************',
  loaderOptions: {
    useRecaptchaNet: true,
  },
});
app.mount('#app');
```

進行註冊完後，你就可以發現在畫面右下角有 Google reCAPTCHA v3 的圖標，這個圖標也可以用 Css style 強制隱藏。

![](/images/vue-google-recaptcha/post_content_img_10.png)

但如果你是**使用 TypeScript+Vue3 的 OptionApi**，就需要在 src 檔案目錄下新增`shims-vue-recaptcha-v3.d.ts`，才可以正常使用，如果是**CompositionApi 則可以跳過**！

```Typescript shims-vue-recaptcha-v3.d.ts
import { ReCaptchaInstance } from 'recaptcha-v3'

declare module '@vue/runtime-core' {
  interface ComponentCustomProperties {
    $recaptcha: (action: string) => Promise<string>
    $recaptchaLoaded: () => Promise<boolean>
    $recaptchaInstance: ReCaptchaInstance
  }
}
```

接著建立 GoogleReCaptchaV3.vue 進行 import，可以依需求 import 在各個地方！但這邊比較要注意的是 reCAPTCHA v3 主要是使用者在進行某一個事件時觸發，像是登入或是註冊發送表單的時候進行，這邊先以 Button 當作觸發條件！

```Typescript GoogleReCaptchaV3.vue
<script setup lang="ts">
import { reactive } from 'vue';
import { useReCaptcha } from 'vue-recaptcha-v3';
const { executeRecaptcha, recaptchaLoaded } = useReCaptcha()!;
const instance_vueRecaptchaV3 = reactive({
  executeRecaptcha: async () => {
    // (可選) 等待直到 recaptcha 載入完成
    await recaptchaLoaded();
    // 執行 "login" 狀態的 reCAPTCHA
    const token = await executeRecaptcha('login');
    // 後續傳給後端進行認證
    console.log(token);
  },
});
</script>
```

```HTML GoogleReCaptchaV3.vue
<template>
  <div class="v3-margin">
    <button type="button"
    @click="instance_vueRecaptchaV3.executeRecaptcha"
    >
        Submit to active reCAPTCHA
    </button>
  </div>
</template>
<style scoped lang="css">
.v3-margin {
  margin-top: 20px;
}
</style>
```

並在需要的地方進行 import：

![](/images/vue-google-recaptcha/post_content_img_11.png)

在畫面上就可以看到觸發 reCAPTCHA 的 Button，按下去也可以觸發功能：

![](/images/vue-google-recaptcha/post_content_img_12.png)

那由於[vue-recaptcha-v3](https://www.npmjs.com/package/vue-recaptcha-v3)，提供的方式是使用**provide/inject**的方式進行註冊，所以所有**有關套件的操作都要在 vue 的 setup 狀態**進行。
但依照上面的範例，所有狀態觸發都綁定在 Button 上面觸發，相對不彈性，所以將它額外移出去，到新建立的 Composable 去實作。我這邊想到的方法是建立 Composable：useReCaptchaV3.ts，提供一個方法，當要使用發法的時候，傳 vue-recaptcha-v3 的實例(Instance)進去即可，相關實作如下：
建立 useReCaptchaV3.ts，並實作：

```Typescript useReCaptchaV3.ts
import { IReCaptchaComposition } from 'vue-recaptcha-v3';
export async function useReCaptchaV3(useReCaptcha: IReCaptchaComposition) {
  const { executeRecaptcha, recaptchaLoaded } = useReCaptcha;
  // (可選) 等待直到 recaptcha 載入完成
  // (optional) Wait until recaptcha has been loaded.
  await recaptchaLoaded();
  // 執行 "login" 狀態的 reCAPTCHA
  // Execute reCAPTCHA with action "login".
  const token = await executeRecaptcha('login');
  // 後續傳給後端進行認證
  // Do stuff with the received token.
  console.log(token);
}
```

在需要用到 reCAPTCHA v3 的地方進行引入：

```Typescript GoogleReCaptchaV3.vue
<script setup lang="ts">
import { useReCaptcha } from 'vue-recaptcha-v3';
import { useReCaptchaV3 } from '../composables/useReCaptchaV3';
const useReCaptchaInstance = useReCaptcha()!;
</script>
```

```HTML GoogleReCaptchaV3.vue
<template>
  <div class="v3-margin">
    <button type="button"
    @click="useReCaptchaV3(useReCaptchaInstance)">
        Submit to active reCAPTCHA
    </button>
  </div>
</template>
<style scoped lang="css">
.v3-margin {
  margin-top: 20px;
}
</style>
```

這樣一樣可以完成觸發，且更彈性、靈活，可以在各處依照需求引入！

![](/images/vue-google-recaptcha/post_content_img_13.png)

## 結論

那到目前為止，就完成了 Google reCAPTCHA v2 v3 的實作了，在依照跟後端的溝通進行驗證即可~沒有遇到什麼困難，算是很好接的 Google Api 了 XD

那這邊附上實作的範例檔案：[vue3-recaptcha-example](https://github.com/ben890524/vue3-recaptcha-example)。

###### tags: `Vue` `Google reCAPTCHA`
