---
title: "AndroidでmDNSを名前解決する"
categories: ["Android"]
---

[主要OSの中でAndroidだけがmDNSを名前解決できないらしい。](https://qiita.com/maccadoo/items/48ace84f8aca030a12f1)  
[Chromium](https://bugs.chromium.org/p/chromium/issues/detail?id=405925)・[AOSP](https://issuetracker.google.com/issues/140786115)にissueが提出されているがまったくもって動きがない。

ということで[DotLocalFinder](https://github.com/Network-Revolution/DotLocalFinder)というアプリを~~丸パクリ~~インスパイアしてmDNSの名前解決をしてみる。

**DNSもmDNSもDNS-SDも全く知らないので聞きかじり見かじり注意。**

## そもそもmDNSとは

そもそもmDNSという規格がよくわからないので調べてみる。  
[Wikipedia](https://ja.wikipedia.org/wiki/%E3%83%9E%E3%83%AB%E3%83%81%E3%82%AD%E3%83%A3%E3%82%B9%E3%83%88DNS)様によると、[RFC 6762](https://tex2e.github.io/rfc-translater/html/rfc6762.html)で定義されたZeroconf技術とか言うものらしい。  
[Zeroconf](https://ja.wikipedia.org/wiki/Zeroconf)とか言うのはDHCPやらmDNSやらUPnPやらの技術を一纏めにした言い方なのかな？

似た技術の中に[RFC 6763](https://tex2e.github.io/rfc-translater/html/rfc6763.html)で定義されたDNS-SDというものがあるがこっちはネットワーク内の機器を検索する技術みたい。

## mDNSをAndroidで名前解決するには

まず「mDNS Android Developers」でGooglingすると[NSD](https://developer.android.com/training/connect-devices-wirelessly/nsd)ってAPIが出てくるがこれはDNS-SD関連のものでmDNSとは関係ないらしい。  
他にも[JmDNS](https://github.com/jmdns/jmdns)やら[mdnsjava](https://github.com/posicks/mdnsjava)など使えそうなライブラリが出てくるがどれもDNS-SD関連のものでmDNSを名前解決できるものは出てこない。  
自分の解釈が間違ってるのかな？

すると[DotLocalFinder](https://github.com/Network-Revolution/DotLocalFinder)という唯一自分の目的にあってそうなアプリを見つけたのでコードを見てみると「xyz.gianlu.mdnsjava:mdnsjava」というライブラリを使用している。  
調べると[Stackoverflow](https://stackoverflow.com/questions/64713877/how-to-resolve-ipv4-and-ipv6-from-local-using-mdns-in-android)にこれを使えば名前解決できるとあったし間違いない。  
恐らくリポジトリは[これ](https://github.com/devgianlu/mdnsjava)だが現在削除されていて閲覧は不可能。  
[maven](https://repo1.maven.org/maven2/xyz/gianlu/mdnsjava/mdnsjava/2.2.1/)のSourceを見る限り前述したmdnsjavaのフォークのようだ。

## 実装

というわけで~~丸パクリ~~インスパイアしてテキトーに実装してみる。  
メインスレッドでやると怒られるのでDispatchers.IOでやろーね☆

**ちなみにTarget APIを30以上に上げると動作しません。**  
<https://github.com/Network-Revolution/DotLocalFinder/issues/2>  
（29だとプレイストア弾かれるらしいけど）

```kotlin
// 名前解決するホスト名
val resolveHost = ""

lifecycleScope.launch {
    val records = withContext(Dispatchers.IO) {
        Lookup(resolveHost, Type.A, DClass.IN).lookupRecords()
        // IPv6 (AAAAレコード)
        // Lookup(resolveHost, Type.AAAA, DClass.IN).lookupRecords()
    }
    if (records.isEmpty()) {
        // 名前解決に失敗した場合
    } else {
        val resolvedIP = (records[0] as ARecord).address.hostAddress
        // IPv6 (AAAAレコード)
        // val resolvedIP = (records[0] as AAAARecord).address.hostAddress
    }
}
```

MulticastLockはやらなくても良いっぽい。（ライブラリ側でよしなにしてくれてるのかな？）
