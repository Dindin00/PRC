---
title: Java小數顯示處理
date: 2018-04-25 14:25:39
categories:
- 自主學習
tags:
- Java
thumbnail: https://upload.wikimedia.org/wikipedia/zh/8/88/Java_logo.png
---

　　最近練習一些題目又需要用到小數顯示的處理，不過時常忘記總是需要上網爬文，這次就下定決心把這些整理起來，以後才不用每次都常常在找這些東西。

　　通常我們會遇到類似`87.1564239`的數值，不過有可能結果只會需要小數點後幾位，而且有時候會是要無條件捨去、進位或四捨五入，遇到這種情況的時候不用擔心，依照下面方法就可以輕鬆處理完。

### 進位方式
　　Java本身提供很多數學運算的方法，而這些處理數學運算的方法都放在`Math`類別裡面，所以在呼叫進位方法前要先呼叫`Math`類別。

* 四捨五入
    * 四捨五入使用的是`round()`方法，該方法會將傳入的數值四捨五入到整數位再回傳給我們，同時這個方法提供了兩種方式讓我們呼叫：
        1. 傳入`double`型態數值，回傳`long`型態數值。
        2. 傳入`float`型態數值，回傳`int`型態數值。
    * 可以依照自己所需的情況選擇要傳入的值，再進行處理，要注意的是，如果要設定一個`float`型態的數值必須　　要強制轉型為`float`，因為Java中預設所有浮點數(含小數點的數值)都為`double`型態，轉型方式及`round()`範例如下所示。
    ```java
    double valueD = 87.1564239;
    float valueF = (float) 87.5164239;
    long resultD = Math.round(valueD);
    long resultF = Math.round(valueF);
    System.out.print(resultD + " , " + resultF);
    //輸出結果為：87 , 88
    ```

* 無條件捨去
    * 無條件捨去使用的是`floor()`方法，這個方法只有提供一種方式給我們呼叫，必須要傳入一個`double`型態數值，接著它會將小數點以後所有的值都捨棄，並回傳一個`double`型態數值給我們，不過要注意的是由於它回傳的是`double`型態數值，可是我們已經將小數點後的值都捨棄了，因此會看到小數點後以類似`3.0`呈現，相關操作範例如下所示。
    ```java
    double valueD1 = 87.1564239;
    double valueD2 = 87.5164239;
    double resultD1 = Math.floor(valueD1);
    double resultD2 = Math.floor(valueD2);
    System.out.print(resultD1 + " , " + resultD2);
    //輸出結果為：87.0 , 87.0
    ```

* 無條件進位
    * 無條件進位使用的是`ceil()`方法，它與`floor()`一樣只提供一種方式呼叫，且需傳入一個`double`型態數值，會回傳`double`型態數值，操作方式範例如下所示。
    ```java
    double valueD1 = 87.1564239;
    double valueD2 = 87.5164239;
    double resultD1 = Math.ceil(valueD1);
    double resultD2 = Math.ceil(valueD2);
    System.out.print(resultD1 + " , " + resultD2);
    //輸出結果為：88.0 , 88.0
    ```

### 取值位數
　　我們已經會依照要求實作四捨五入、無條件捨去及無條件進位了，不過可以發現Java提供的方法都只能取到整數位，所以下面就來講解如何取到特定的位數。

1. 移動位數
    * 這邊以小數點後三位作為講解，我們第一步要先把小數點第三位移動到整數位，如此Java的方法才會幫我們進行進位處理，若要將小數點後第三位移動到整數位，直接將它乘上`1000`即可移動至整數位，其他位數方法雷同，只要乘上10的移動位數次方即可，操作範例如下所示。
    ```java
    double value = 87.1564239;
    //移動至小數點後三位，故乘上10的3次方，即1000
    double move = value * 1000;
    System.out.print(move);
    //輸出結果為：87156.4239
    ```

2. 進位處理
    * 移至整數位後，就可以依照你所需要的進位方式進行處理，基本上三種進位方式作法相同，這邊以四捨五入作為示範，範例如下所示。
    ```java
    double value = 87.1564239;
    double move = value * 1000;
    long carry = Math.round(move);
    System.out.print(carry);
    //輸出結果為：87156
    ```

3. 強制轉型
    * 接著我們要將剛剛做完進位處理的值強制轉型為`int`，至於為什麼呢？因為Java在作乘除運算時，若兩個數值型態不同，會使用空間最大者作為運算結果的型態，由於我們最後要的會是浮點數(`float`或`double`)它們的空間較`int`大，如此可以避免在運算過程中有些輸出`double`或`long`的問題，因此需要將剛剛的數值轉換為`int`型態，操作方法如下範例所示。
    ```java
    double value = 87.1564239;
    double move = value * 1000;
    long carry = Math.round(move);
    int change = (int) carry;
    System.out.print(change);
    //輸出結果為：87156
    ```

4. 移位還原
    * 最後我們要把前面移動的位數復原，因此需要除剛剛乘上的數值，這邊範例的數值為`1000`，不過要特別注意，除回去的值最後方要補上`.0`，因為只要有小數點Java會自動設定為`double`型態，如此就可以符合上一步驟我們所說的，讓最後結果變為`double`型態，若需要為`float`可於最後再將其強制轉型即可，操作範例如下所示。
    ```java
    double value = 87.1564239;
    double move = value * 1000;
    long carry = Math.round(move);
    int change = (int) carry;
    double result = change / 1000.0;
    System.out.print(result);
    //輸出結果為：87.156
    ```

### 重點整理
```java
double value = 53.26549;

//四捨五入至小數點後三位，輸出結果為：53.265
System.out.print((int) Math.round(value * 1000) / 1000.0);
//四捨五入至小數點後四位，輸出結果為：53.2655
System.out.print((int) Math.round(value * 10000) / 10000.0);

//無條件捨棄至小數點後三位，輸出結果為：53.265
System.out.print((int) Math.floor(value * 1000) / 1000.0);
//無條件捨棄至小數點後四位，輸出結果為：53.2654
System.out.print((int) Math.floor(value * 10000) / 10000.0);

//無條件進位至小數點後三位，輸出結果為：53.266
System.out.print((int) Math.ceil(value * 1000) / 1000.0);
//無條件進位至小數點後四位，輸出結果為：53.2655
System.out.print((int) Math.ceil(value * 10000) / 10000.0);
```
