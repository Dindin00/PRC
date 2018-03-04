---
title: 如何在Android中使用gif圖
date: 2018-03-04 15:32:31
categories:
- 工作學習
tags:
- Android
thumbnail: https://media1.tenor.com/images/e9a2b43613bdde8dea94e81c4ca7e4c2/tenor.gif?itemid=5072286
---

今天因為工作需要，需要在ImageView中放.gif格式的圖片，湊巧發現原來Android中，並不支援直接顯示gif。當然就上網找找，發現如果用原生Android方式，必須要把gif的每個畫面都轉成一張圖，然後在XML中讀入每張圖片，再透過再`AnimationDrawable`類別才可以完成。不過這整個相當流程相當繁瑣，於是就去找看有沒有間單的東西可以完成。就發現了這個套件 [android-gif-drawable](https://github.com/koral--/android-gif-drawable)。

---

## android-gif-drawable

這個套件主要是可以讓Android開發者放入gif圖檔，除此也可以放入一般圖片，所以在開發時，如果遇到某一處可能為一般圖片或gif圖，就只要使用該套件提供的`GifImageView`就能兩者都套用，就可以取代掉原本的`ImageView`，並增加開發上的方便性，下面就做個簡單的範例來展示如何使用。

---

## 匯入套件

在`build.gradle`之中的`dependencies`內**加入**如下內容，再按下**Sync Now**。

```
dependencies {
    compile 'pl.droidsonroids.gif:android-gif-drawable:1.2.11'
}
```

---

## XML(UI)

一開始直接先拉入一個`ImageView`然後依照你想要的樣子做設定，設定好後切換到原始碼模式，再將`ImageView`改為`pl.droidsonroids.gif.GifImageView`，因為`GifImageView`可以跟`ImageView`做一樣的設定，因此建議先設定好再直接改過來就可以了，好了後應該可以看到如下的XML原始碼。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="tw.idv.lizhi.gifimageviewsample.MainActivity">

    <pl.droidsonroids.gif.GifImageView
        android:id="@+id/imageView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:srcCompat="@drawable/sample_image" />

</LinearLayout>
```

---

## Java

如果在`GifImageView`中的app:srcCompat屬性直接設定圖片為gif，其實可以發現他是不會顯示任何圖片的，因為`GifImageView`要透過Java程式碼來設定圖片才可以，因此我們先在程式中新增一個`GifImageView`，並跟剛剛XML的`GifImageView`做連結。

```java
GifImageView gifImageView = findViewById(R.id.imageView);
```

取得`GifImageView`後，利用`GifDrawable`將gif圖轉換成drawble，此時會發現出現紅線，主要是因為這個動作可能會產生輸出入例外，所以這時候只要用try catch包起來即可(可以利用**Alt+Enter**快速完成)。

```java
try {
    GifDrawable gifDrawable = new GifDrawable(getResources(), R.drawable.sample_image);
} catch (IOException e) {
    e.printStackTrace();
}
```

最後透過`GifImageView`的setImageDrawble將圖片設定上去，即可收工。
```java
gifImageView.setImageDrawable(gifDrawable);
```

如果需要查看範例的可以點此：[範例](https://github.com/Dindin00/GifImageViewSample)

