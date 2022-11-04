---
title: 物件導向 概念 - 物件導向四大概念
date: 2022-11-04 12:57:45
tags:
- Object Oriented
- C#
categories:
- Object Oriented
top_img: /images/default_banner.png
cover: 
---
## 前言
本篇要來介紹物件導向的四大概念，那就來簡單介紹物件導向是怎麼樣的概念~
物件導向是將大部分的程式碼變成像是物件的方式進行呈現，程式互相會是以物件的方式進行溝通。物件跟物件之間的溝通，可以想成是人與人進行溝通，每個人都會有姓名、性別、年齡等等，這些在物件就是**屬性（Attributes）**；每個人也都會有技能或是動作，在物件我們就稱它為**方法（Methods）**。
一個物件會由屬性跟方法所組成，但這個時候你可能會有一些困惑，如果人是一個物件，但有可能我有一些事情是不想要給別人知道的，那該怎麼辦？
物件的屬性和方法都有它的**可視性（Visibility）**，可以使比較敏感的資料不被揭露，達到資料隱藏（Information Hiding）的效果。
簡單的介紹完物件導向的基本後，就來介紹物件導向的四大基本概念吧~

## 物件導向四大基本概念
1. 封裝（Encapsulation）
2. 抽象（Abstraction）
3. 繼承（Inheritance）
4. 多型（Polymorphism）

### 封裝（Encapsulation）
通常一個物件會有一些讀寫的限制，像是public或是private等等的限制。封裝是對一個物件的規範，物件不能將所有資訊顯露給使用者（caller），若是一個物件的部分資訊比較敏感，這些資訊又全都顯露給使用者，就會有安全的問題，可以想像使用者只要知道物件有甚麼方法，不需要知道是怎麼實作的，也就是資料隱藏（Information hiding）。

以下以一個C#簡單的getter和setter來解釋：
``` C#
public class Student
{
  public int Age { get; set; }
}
// 上面是C#的寫法，在其他物件導向語言，大致上要表達的意思下
public class Student
{
  private int _age;
  public int GetAge()
  {
    return _age;
  }
  public void SetAge(int age)
  {
    _age = age;
  }
}
```
有一個Student Class有Age這個屬性（Attribute），且`定義{ get; set; }`，代表他有public的getter和setter，皆為任何使用者都可以使用的。
``` C#
public class Student
{
  public int Age { get; private set; }
}
// 上面是C#的寫法，在其他物件導向語言，大致上要表達的意思下
public class Student
{
  private int _age;
  public int GetAge()
  {
    return _age;
  }
  private void SetAge(int age)
  {
    _age = age;
  }
}
```
有一個Student有Age這個屬性(Attribute)，且`定義{ get; private set; }`，代表他有public的getter和private的setter，private的setter只有在自己這個物件裡面可以被調用。

### 抽象（Abstraction）
會定義一個父類別，父類別會先有一線基本功能，也可以擁有一些已經被定義的方法和抽象方法，抽象方法像是abstract function或implement function，這類型的方法不能夠再父類別被實作，會在繼承的子類進行實作，但抽象方便規方便但不能濫用。

定義一個抽象的父類，且有抽象方法並未實作：
``` C#
abstract class Bird{
    public abstract void fly();
}
```
繼承抽象父類後，一定要實作父類的抽象方法：
``` C#
public class SomeBird:Bird{
  public override void fly() => Console.WriteLine("I can fly.");
}
```

### 繼承（Inheritance）
會基於某個父類別對物件的定義加以擴充，子類別可以繼承父類別原來的某些定義，並也可能增加原來的父類別所沒有的定義，或者是重新定義父類別中的某些特性（Override），但在定義父類別與繼承子類別時一定要遵守IS-A的概念。
IS-A概念就是：假如要使A Class繼承B Class，那A就一定要是一種B，A is a kind of B.

以下是bird的父類，有一個fly()的方法。
``` C#
public class Bird{
  public void fly(){
    Console.WriteLine("Bird can fly.");
  }
}
```
以下是繼承bird父類並擴充的子類"鴿子"（Pigeon）：
``` C#
public class Pigeon:Bird{
  private int speed = 20;
  public override void fly(){
    Console.WriteLine("Pigeon can fly.");
  }
}
```
下列未遵守IS-A概念：
``` C#
public class Penguin:Bird{
  public override void fly(){
    Console.WriteLine("Penguin ???");
  }
}
```
企鵝不會飛，但是卻給牠一個飛行的方法，明顯的企鵝不是一種會飛的鳥類。

### 多型（Polymorphism）
多型是指的是使用同一個操作介面，以操作不同的物件實例，多型操作在物件導向上是為了降低對操作介面的依賴程度，這是一種抽換概念，之後在各式Design Pattern中會常用到此概念。

今天有一個Service的class專門計算一台手機打折過後的價格：
``` C#
public class SaleService{
  private double discount = 0.8;
  public double iphonePriceWithDiscount(Iphone phone){
    return phone.price * this.discount;
  }
  public double androidPriceWithDiscount(Android phone){
    return phone.price * this.discount;
  }
}
```
從上述程式碼發現，如果我每有一種作業系統的手機，我就要新增一種方法去計算那種作業系統手機的價格。
所以定義一種操作介面為父類，讓各式作業系統的手機繼承，並對一種操作介面進行操作即可。
``` C#
public class Phone{
  public int PhonePrice { get; set; }
  public Phone(int price){
    PhonePrice = price;
  }
}
public class Iphone:Phone{
  public Iphone(int price):base(price){}
}
public class Android:Phone{
  public Android(int price):base(price){}
}
```
根據上方更改後的程式碼，可以對Service進行改造：
``` C#
public class SaleService{
  private double _Discount = 0.8;
  public double phonePriceWithDiscount(Phone phone){
    return phone.phonePrice * this._Discount;
  }
}
```

## 小結