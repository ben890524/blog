---
title: 前端知識區 / 前端面試題
date: 2022-12-16 11:18:45
tags:
  - Frontend
  - Html
  - CSS
  - Javascript
categories:
  - Frontend
top_img: /images/default_banner.png
cover:
# id: post-6
---

# For Front-End Learning

## HTML

> [行內元素&區塊元素](https://www.daconote.com/span-div-difference/)

## Css

> [Css 預處理器 Pre-procesor](https://changtsuiling.gitbooks.io/sass/content/chapter1/1-1-shi-me-shi-css-yu-chu-li-qi-ff1f.html)
> [CSS 選擇器 Selector](https://injerry.pixnet.net/blog/post/38847966#:~:text=%E9%81%B8%E5%8F%96%E5%99%A8%EF%BC%88Selector%EF%BC%89%EF%BC%8C%E4%B9%9F%E6%9C%89,%E9%80%99%E6%A8%A3%E7%BF%BB%E8%AD%AF%E4%B9%9F%E5%B0%8D%E5%95%A6%EF%BC%89)
> [CSS 權重](https://ithelp.ithome.com.tw/articles/10196454)
> [CSS Box Model](https://www.oxxostudio.tw/articles/202008/css-box-model.html)
> [CSS Reset / Normalized](https://ithelp.ithome.com.tw/articles/10196528?sc=rss.qu)
> [網上推的 CSS 面試題目](https://www.facebook.com/groups/f2e.tw/permalink/4925715637465762/)
1. 每一個瀏覽器在定義每一個HTML標籤時用的style不一樣，會出現你的應用程式前端在各個瀏覽器顯示出來的結果可能不一樣，所以製作出reset.css，強制讓Browser的基本屬性變為你設定的值，讓不統一的地方一致，缺點是不彈性。所以後續解決這個問題，才多了CSS Normalize去繼承Browser原有的CSS屬性，再去做加法。
2. 哪種CSS會被蓋掉、哪種CSS權重比較重，在同樣指定的CSS上，哪一個會被套用。簡單來說important>inline-style>id>class=attribute=psuedo-class>html-element。
3. html tags中的box element，由外往內有margin、border、padding和content。

> [CSS BEM設計模式](https://w3c.hexschool.com/blog/35afa83f)
> [CSS Cascade Layers @layer 解決css權重](https://blog.yuyansoftware.com.tw/2022/02/css-cascade-layers-specificity/)
> [Tailwind css v.s Bootstrap](https://blog.hiskio.com/tailwind-css-or-bootstrap/)

## JavaScript

> [JS 基本中的基本](https://ithelp.ithome.com.tw/articles/10190873)
> [JS Automatic Semicolon](https://shawnlin0201.github.io/JavaScript/JavaScript-Automatic-Semicolon-Insertion/)
> [JS IIFE](https://medium.com/helena-chang/js%E4%BA%8C%E9%83%A8%E6%9B%B2-%E7%AB%8B%E5%8D%B3%E5%87%BD%E5%BC%8F-iife-b60bd694bbea)
> [JS 宣告提升 Hoisting](https://ithelp.ithome.com.tw/articles/10266627)
> [JS 冒泡事件 Bubble Event](https://ithelp.ithome.com.tw/articles/10265819)
> [JS 冒泡事件/事件捕獲 Event Bubbling/Event Capturing](https://hackmd.io/@Heidi-Liu/note-fe201-dom)
> [JS 閉包 Closure](https://pjchender.dev/javascript/js-closure/)
> [JS 作用域 Scope](https://medium.com/take-a-day-off/js-scope-%E4%BD%9C%E7%94%A8%E5%9F%9F-ee536640963b)
> [JS Promise](https://pjchender.dev/javascript/js-promise/)
> [JS Event Loop](https://medium.com/itsems-frontend/javascript-event-loop-event-queue-call-stack-74a02fed5625)
> [JS 原型鏈 Prototype Chain](https://medium.com/@mengchiang000/js%E5%9F%BA%E6%9C%AC%E8%A7%80%E5%BF%B5-%E5%8E%9F%E5%9E%8B%E9%8F%88-prototype-chain-96c742893795)

## TypeScript

## Browser

> [TCP 三向交握](https://notfalse.net/7/three-way-handshake)
> [Http Methods](https://ithelp.ithome.com.tw/articles/10250980)
> [CORS](https://ithelp.ithome.com.tw/articles/10238084)
> [DOM v.s BOM](https://ithelp.ithome.com.tw/articles/10235079)
> [JWT JSON Web Token](https://5xruby.tw/posts/what-is-jwt)
> [SSR v.s CSR](https://shubo.io/rendering-patterns/)
> [LocalStorage v.s SessionStorage v.s Cookie](https://medium.com/@bebebobohaha/cookie-localstorage-sessionstorage-%E5%B7%AE%E7%95%B0-9e1d5df3dd7f)
> [What Can I use?](https://caniuse.com/)
> [REST v.s gRPC v.s OpenAPI](https://ikala.cloud/grpc-openapi-and-rest-1/#:~:text=gRPC%20%E6%98%AF%E4%B8%80%E7%A8%AE%E5%AF%A6%E4%BD%9C,%E5%B7%A5%E4%BD%9C%E6%96%B9%E5%BC%8F%E8%88%87%E6%AD%A4%E7%9B%B8%E5%8F%8D%E3%80%82)
