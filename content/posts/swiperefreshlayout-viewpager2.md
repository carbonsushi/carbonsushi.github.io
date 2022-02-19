---
title: "SwipeRefreshLayoutの子にViewPager2を使うなら1.2.0-alpha01を使え"
categories: ["Android"]
---

タイトル通り。

公式にもろにはっきり書かれてるけどメモ。  
<https://developer.android.com/jetpack/androidx/releases/swiperefreshlayout>

## 解決策

```xml
<androidx.swiperefreshlayout.widget.SwipeRefreshLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <androidx.viewpager2.widget.ViewPager2
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</androidx.swiperefreshlayout.widget.SwipeRefreshLayout>
```

みたいに使うとSwipeRefreshLayoutとViewPager2が競合してタッチ操作がおかしくなる。  
なので修正されてる1.2.0-alpha01を使えばサクッと解決。

> requestDisallowInterceptTouchEvent(boolean) が、他の ViewGroup と同様に、リクエストを受け入れるようになりました。この新しい動作は setLegacyRequestDisallowInterceptTouchEventEnabled で無効にできますが、無効にしないことを強くおすすめします。（I968da、b/141855018）

```groovy
implementation 'androidx.swiperefreshlayout:swiperefreshlayout:1.2.0-alpha01'
```

1.1.0-alpha03で直されてるのはViewPager2が親の場合の話なのかな？  
テストしていないので不明。

> requestDisallowInterceptTouchEvent(boolean) が常に親にまで伝播するようになりました。この新しい動作は setLegacyRequestDisallowInterceptTouchEventEnabled で無効にできますが、無効にしないことを強くおすすめします（aosp/1108540）

それにしても検索でViewPagerとViewPager2の情報がまあよく混ざる。  
もうちょっと名前変えてくれ。
