---
title: 物件導向的五大原則 - SOLID
date: 2022-11-05 00:45:12
tags:
- Object Oriented
- C#
categories:
- Object Oriented
top_img: /images/default_banner.png
cover: 
# id: post-4
---
## 前言
俗話說的好「不聽老人言，吃虧在眼前」，物件導向程式設計的五個基本原則是早期程式開發就存在的原則，既然能套用到至今，代表一定有它的道理，那今天就是要來窺探其中的奧妙（？
這些原則都同時被遵守時，它們可以使一個軟體更容易進行維護和系統的擴充變得更加彈性。那我們就來看看這些原則吧~

## SOLID為物件導向的五大原則
- **S : Single Responsibility Principle**
- **O : Open Close Principle**
- **L : Liskov Substitution Principle**
- **I : Interface Segregation Principle**
- **D : Dependency Inversion Principle**

### Single Responsibility Principle 單一職責原則
#### 說明

**一個類別（class）只能負責專一的一種職責**。換句話說就是，**一個類別（class）中的所有，只能對一種角色負責**。

舉例來說，今天郵差要配送一個包裹：
``` C#
public class 郵差{
  取得聯絡資訊(){};
  撿貨(){};
  騎車配送(){};
  送達收件者地址(){
    Console.WriteLine("抵達收件者地址");
    Console.WriteLine("打開郵箱並放入包裹");
  };
}
```
這四件方法都跟送一件包裹有關，從取得收件者聯絡資訊->撿貨->騎車配送->送達收件者地址，但這四個方法中，**取得收件者聯絡資訊和撿貨跟郵差完全沒有關係或是關係甚微，這部分就違反了單一職責原則**。通常遇到此情況，會把與此類別無關的方法進行抽離做成其他物件。

接著還有一個問題，在方法`送達收件者地址()`中，進行了兩個動作，第一是抵達地址，第二是打開郵箱並放入包裹，這兩個動作也不符合單一職責原則，因為`送達收件者地址()`，做了送達收件者地址之外的事情，就是打開郵箱並放入包裹。一般來說遇到這種情況就會在抽離做一個方法。

#### 小結
通常單一職責原則會希望**一個類別一個方法中的內聚力（Cohesion）高一點，讓一個邏輯都在一個類別一個方法裡面做處理，不要四散各處**。**不讓邏輯四散各處，也降低了耦合性（Coupling）**，各物件之間也不會有過多的溝通，也方便管理程式碼。

### Open Close Principle 開放封閉原則
#### 說明

對擴展開放，而對修改封閉。修改就是把東西拆開來改，把原有的程式碼進行修改；而擴展就是對原有的東西額外加裝模組，使他符合需要更改的內容。只要變化都有成本，例如變動的難易度、變動造成的影響範圍等等都會影響到成本，若是程式碼冗長、內部邏輯複雜，類別之間互相耦合、影響範圍很廣，常常改壞東西卻不知道是哪裡造成的。使得修改很困難，讓開發效率變低。

通常會區分為**主要邏輯和附加邏輯**，將主要邏輯增加一些條件去符合成要修改的內容，這就是附加邏輯。主要邏輯通常不會進行變動，如果需要新增不同的需求，會去增加附加邏輯，讓附加邏輯達成需求，才不會對同一個主要邏輯一直修改，到最後破壞了主要邏輯。

#### 小結
**以組合取代繼承**。在使用繼承上，如果要增加繼承類的方法，則要整個繼承類都增加這些方法，會導致更改困難，過度耦合；但是如果採用組合，可以根據抽換的實作的類去更改實行的內容（其實也是多型的一種應用，也是後續很多設計模式的核心）。

### Liskov Substitution Principle 里氏替換原則
#### 說明

原則的定義：**子類別要能完全代理父類別的所有事情**。要**符合IS-A的規則，也就是SubType**。
以下會簡單介紹如何去判別一個繼承類是否符合Liskov替換原則！
``` C#
class Rectangle{
  private int width;
  private int height;
  public void setWidth(int w){
    width = w;
  }
  public void setHeight(int h){
    height = h;
  } 
}

class Square : Rectangle {
  public void setWidth(int w){
    width = w;
    height = w;
  }
  public void setHeight(int h){
    height = h;
    width = h;
  }
}
```
根據上述矩形和繼承他的正方形來看，分成7點Liskov替換原則應遵守之條件來檢查，有沒有違反：

1. Covariance of argument
    - 在Square中`public void setHeight(int h);`， 在Rectangle中`public void setHeight(int h);`
    - **兩者Function傳入進去的參數數皆一致**，故此條件不違反。
2. Covariance of result
    - 在Square中`public void setHeight(int h);`， 在Rectangle中`public void setHeight(int h);`
    - **兩者Function的回傳型態皆一致**，故此條件不違反。
3. Exception rule
    - 父類規定的內容沒有例外處理，子類也無例外處理。
    - 如果父類無例外處理，子類也就不能多了例外處理；如果父類有例外處理，那子類例外處理的類別就要和父類一致。
4. Pre-condition rule
    - Pre-condition rule是指**執行此方法前，一定要達成的條件**，所以有"Pre"的稱呼。
    - 此範例沒有特別的Pre-Condition。
    - 其實很重要，但因為這裡例子不用判斷，所以顯得不太重要。
5. Post-condition rule
    - Post-condition rule是指**執行此方法後，一定要達成的條件**，所以有"Post"的稱呼。
    - 此範例沒有特別的Post-Condition。
    - 跟Pre-condition一樣，但因為這裡例子不用判斷，所以顯得不太重要。
6. Invariant rule
    - Invariant rule是指**不管方法前後，一定要成立的條件**，像是計算機，一定要是數字才能做計算。
``` C#
public void setWidth(int w){
  width = w;
  height = w;
}
public void setHeight(int h){
  height = h;
  width = h;
}
```
    - 在正方形的Invariant裡，規定的應該是長與寬應該要相等，所以設定長或設定寬的時候，也要一併設定對應的數值，故在此有符合。
7. Constraint rule
``` C#
public void setWidth(int w){
  width = w;
}
public void setHeight(int h){
  height = h;
} 
```
    - 在矩形的規則中，設定長只能設定到長，設定寬只能設定到寬，所以繼承矩形的正方形也要符合這個規則。但是正方形不符合，故違反Liskov替換原則。

#### 小結
**繼承在沒有好的規劃、設計下，盡量不要隨意使用**。因為繼承會造成很大的依賴性（Dependency），很常會因為設計或是非預期的行為，會讓你的架構越來越大，大到有些錯誤不可預期，會更難以維護。

### Interface Segregation Principle 介面分割原則
#### 說明

**模組與模組之間的依賴，不應有用不到的功能可以被對方呼叫**。每個實作都應該有契合的介面。

這是一台車的實作介面：
``` C#
interface car{
  public void 引擎發動();
  public void 油門();
  public void 剎車();
}
```
今天有一台休旅車來實作car這個interface：
``` C#
public class Suv : car{
  public void 引擎發動(){
    Console.WriteLine("轟隆隆隆🤣🤣隆隆隆隆衝衝衝衝😏😏😏拉風😎😎😎引擎發動🔑🔑🔑引擎發動+🚗+👉+🚗");
  }
  public void 熄火(){
    Console.WriteLine("0⃣到💯K only 4⃣秒鐘😏😏");
  } 
  public void 剎車(){
    Console.WriteLine("只怕警察👮♂👮♂BI BI BI 叫我路邊靠");
  }
}
```
以上是正確的介面實作方法。但如果今天出現了一台噴射車車，多了一個噴射加速的功能，直接在interface car裡面新增方法，這時就違反介面分割原則，因為Suv並不會噴射加速的功能。
所以要新增一個噴射加速功能時，可以新增一個interface，讓有這個功能的車去實作這個介面即可，也不會去動到原有車子的介面！實作完後各interface和class如下：
``` C#
// 正常車會有的車功能介面
interface car{
  public void 引擎發動();
  public void 油門();
  public void 剎車();
}
// 噴射加速車會有的功能介面
interface 噴射加速功能{
  public void 噴射加速();
}
// Suv實作正常車會有的功能介面即可
public class Suv : car{
  public void 引擎發動(){
    Console.WriteLine("轟隆隆隆🤣🤣隆隆隆隆衝衝衝衝😏😏😏拉風😎😎😎引擎發動🔑🔑🔑引擎發動+🚗+👉+🚗");
  }
  public void 熄火(){
    Console.WriteLine("0⃣到💯K only 4⃣秒鐘😏😏");
  } 
  public void 剎車(){
    Console.WriteLine("只怕警察👮♂👮♂BI BI BI 叫我路邊靠");
  }
}
// 噴射加速車車實作一般車和噴射加速車功能
public class 噴射加速車車 : car, 噴射加速功能{
  public void 引擎發動(){
    Console.WriteLine("轟隆隆隆🤣🤣隆隆隆隆衝衝衝衝😏😏😏拉風😎😎😎引擎發動🔑🔑🔑引擎發動+🚗+👉+🚗");
  }
  public void 熄火(){
    Console.WriteLine("0⃣到💯K only 4⃣秒鐘😏😏");
  } 
  public void 剎車(){
    Console.WriteLine("只怕警察👮♂👮♂BI BI BI 叫我路邊靠");
  }
  public void 噴射加速(){
    Console.WriteLine("讓😯 看到的人以為是夢😱😱 還沒醒來😴😴 就已經無影無蹤👻👻");
  }
}
```

#### 小結
將介面分離好，只讓**該實作的類別去實作、減少不必要的操作介面出現在類別中**，可以讓耦合性（Coupling）更低，**把實作隱藏起來、保持抽象，可以讓程式夠有彈性**。

### Dependency Inversion Principle 依賴反轉原則
#### 說明

**高模組不應該依賴低模組，應該讓高、低模組去依賴抽象；換句話說，適當的抽象，可以讓你的程式架構有更多的彈性。**

假設今天我想要吃一個漢堡，我要吃漢堡，才會有飽足感，就是我依賴漢堡。換成程式的寫法，如下：
``` C#
public class People{
  private readonly Hamburger _hamburger;
  public People(Hamburger h){
    _hamburger = h;
  }
  public void eat(){
    _hamburger.eat();
    Console.WriteLine("吃飽啦");
  }
}
```
這樣哪天你像要吃薯條，或是其他甚麼食物，你都要製作新的方法，和給予空間儲存該類變數的記憶體空間。上述的寫法很依賴漢堡，沒有漢堡，People就沒有eat的方法，極度依賴。所以**讓Hamburger去擁有父類別，並將父類別傳入People作為參數，這樣我今天想要吃其他食物，不用去為了新的食物去新增方法，直接增加新的食物的類別，並傳入即可，便可達到依賴反轉的效果**。
``` C#
// 製作新的food介面
interface Ifood{
  public void eat();
}
// 讓漢堡去實作food介面
public class Hamburger : IFood{
  public void eat(){
    Console.WriteLine("我被吃掉了QQ");
  }
}
// 改寫people過度依賴漢堡的寫法
public class people{
  private readonly IFood _food;
  public people(IFood food){
    _food = food;
  }
  public void eat(){
    _food.eat();
    Console.WriteLine("吃飽啦");
  }
}
```

#### 小結
遵守依賴反轉原則，可以减少class之間的耦合性（Coupling），也可以提高系统的可讀及維護性。而且在開發過程中，*模組之間可能經常變化，若是太過依賴低模組，會造成程式碼冗長、過度依賴，所以將依賴反轉、依賴抽象，可以降低開發時的風險**。


## 總結
這些原則都很抽象，需要有實際的例子或是實際的去操作，會比較好理解，非常建議看完文章的各位，用自己的想法、構思，去實作自己的範例，會有更多的理解~那SOLID所有的原則都是為了使小至程式碼，大至整個系統架構，變得更彈性更靈活。不會讓一些小更動，讓原本的程式碼造成巨大的影響。

那最後用一句簡單的句子來描敘一下各個原則吧：
**S：單一類別、方法對應單一責任、降低耦合。**
**O：開放擴充、封閉修改。**
**L：類別間的相容性。**
**I：介面要特定目的、易懂、可再用性高。**
**D：抽象化，依賴抽象。**

一樣如果本篇有誤，可以聯繫我，讓我修正QAQ，希望未來可以帶給大家更好的文章~