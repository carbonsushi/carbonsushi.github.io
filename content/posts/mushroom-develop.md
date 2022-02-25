---
title: "マッシュルームの作りかた"
categories: ["Android"]
---

マッシュルームの開発ドキュメントがいつの間にか消失していたのでメモ。  
Internet Archiveに残ってました。  
<https://web.archive.org/web/20210303183042if_/simeji.me/blog/manuals/manuals_android/android_mushroom/make_mushroom/id=58>

## 要約（？）

actionは`com.adamrocker.android.simeji.ACTION_INTERCEPT`、  
categoryは`com.adamrocker.android.simeji.REPLACE`。  
（暗黙的インテントなので`android.intent.category.DEFAULT`もcategoryに追加する）

Simeji（呼び出し側）が送ってきた文字列は`replace_key`でgetStringExtraすると取得可能。  
Simeji（呼び出し側）に文字列を返す時は同じくresultに`replace_key`でputExtraすればOK。
