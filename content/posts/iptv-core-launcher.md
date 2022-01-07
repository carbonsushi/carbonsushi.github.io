---
title: "IPTV Core launcherを作った"
categories: ["Android"]
---

IPTVというそのまんまの名前のアプリでIPTVを視聴していたのだが、  
いつの間にか非公開になっていて再インストール不可に。  
<https://web.archive.org/web/20211108010120if_/play.google.com/store/apps/details?id=ru.iptvremote.android.iptv>

同じ作者が公開しているIPTV CoreとIPTV Core Launcherというアプリを使えばほぼ同じ機能・無料で使えるようだ。  
<https://play.google.com/store/apps/details?id=ru.iptvremote.android.iptv.core>  
<https://play.google.com/store/apps/details?id=ru.iptvremote.android.iptv.core.launcher>

しかしIPTV Core Launcherが毎回プレイリストのURLを入力しなければならなく不便なので自分でLauncherを作ってみた。  
おまけとしてmDNSの名前解決機能付き。  
<https://github.com/carbonsushi/iptv-core-launcher>

## 使い方

IPTV CoreとIPTV Core launcherをインストール後「IPTV Core launcher: Settings」を開いてプレイリストのURLを設定する。  
設定したら「IPTV Core launcher」を開くだけ。

## オチ

実はよくよく設定見てみたらPIP機能が付いていなかったり一部の設定が消えていたりとただの劣化版だった。  
有料版は公開されたままだけど買うほどじゃない…  
<https://play.google.com/store/apps/details?id=ru.iptvremote.android.iptv.pro>
