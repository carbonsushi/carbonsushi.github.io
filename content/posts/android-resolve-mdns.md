---
title: "AndroidでmDNSを名前解決する"
categories: ["Android"]
---

主要OSの中でAndroidだけがmDNSを名前解決できないらしい。  
<https://qiita.com/maccadoo/items/48ace84f8aca030a12f1>

Chromium・AOSPにissueが提出されているがまったくもって動きがない。  
<https://bugs.chromium.org/p/chromium/issues/detail?id=405925>  
<https://issuetracker.google.com/issues/140786115>

ということでDotLocalFinderというアプリを~~丸パクリ~~インスパイアしてmDNSの名前解決をしてみる。  
<https://github.com/Network-Revolution/DotLocalFinder>

**DNSもmDNSもDNS-SDも全く知らないので聞きかじり見かじり注意。**

## そもそもmDNSとは

そもそもmDNSという規格がよくわからないので調べてみる。  
Wikipedia様によると、RFC 6762で定義されたZeroconf技術とか言うものらしい。  
ZeroconfはDHCPやらmDNSやらUPnPやらの技術を一纏めにした言い方なのかな？

似た技術の中にRFC 6763で定義されたDNS-SDというものがあるがこっちはネットワーク内の機器を検索する技術みたい。

## mDNSをAndroidで名前解決するには

まず「mDNS Android Developers」でGooglingするとNSDというAPIが出てくるがこれはDNS-SD関連のものでmDNSとは関係ないらしい。  
<https://developer.android.com/training/connect-devices-wirelessly/nsd>

他にもJmDNSやらmdnsjavaなど使えそうなライブラリが出てくるがどれもDNS-SD関連のものでmDNSを名前解決できるものは出てこない。  
自分の解釈が間違ってるのかな？

するとDotLocalFinderという唯一自分の目的にあってそうなアプリを見つけたのでコードを見てみると「xyz.gianlu.mdnsjava:mdnsjava」というライブラリを使用している。  
<https://github.com/Network-Revolution/DotLocalFinder/blob/09b15a5bafe28175a86e7be7b2079dc5d26486fd/app/build.gradle.kts#L126>

mavenのsource.jarを見る限り前述したmdnsjavaのフォークのようだ。
<https://repo1.maven.org/maven2/xyz/gianlu/mdnsjava/mdnsjava/2.2.1/>

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
    val multicastLock = (applicationContext.getSystemService(WIFI_SERVICE) as WifiManager).createMulticastLock("ResolvemDNS")
    multicastLock.setReferenceCounted(true)
    multicastLock.acquire()

    val records = withContext(Dispatchers.IO) {
        Lookup(resolveHost, Type.A, DClass.IN).lookupRecords()
        // IPv6 (AAAAレコード)
        // Lookup(resolveHost, Type.AAAA, DClass.IN).lookupRecords()
    }

    multicastLock.release()

    if (records.isEmpty()) {
        // 名前解決に失敗した場合
    } else {
        val resolvedIP = (records[0] as ARecord).address.hostAddress
        // IPv6 (AAAAレコード)
        // val resolvedIP = (records[0] as AAAARecord).address.hostAddress
    }
}
```

~~MulticastLockはやらなくても良いっぽい。（ライブラリ側でよしなにしてくれてるのかな？）~~

　 　　　*　　　　　　*  
　　＊　　　　　＋　　うそです  
　 　　 n ∧＿∧　n  
　＋　(ﾖ（* ´∀｀）E)  
　 　 　 Y 　　　 Y　　　　＊

別によしなにはしてくれていなかった。  
端末毎に動作が違うようで、MulticastLockしないと動作しない端末もあるようなので念の為しておいたほうがいい。
