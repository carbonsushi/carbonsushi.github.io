---
title: "2chGear ChMate proxyを作った"
categories: ["Android"]
---

ChMateという掲示板書き込みアプリには「外部アプリで書き込む」というオプションがある。  
恐らく対応しているのは2chGearというアプリのみなのだが、5chが強制SSL化してから使えなくなっているようだ。  
<https://play.google.com/store/apps/details?id=jp.emprise.android.x2chGear>

作者が`<intent-filter>`を変更してくれればそれで済むのだが、いっこうに修正される様子がない。  
なので間に挟まりIntentを受け渡すプロキシアプリを作ってみた。  
<https://github.com/carbonsushi/2chgear-chmate-proxy>

## 使い方

2chGearと2chGear ChMate proxyをインストール後、ChMateの「外部アプリで書き込む」オプションを有効化する。  
あとはChMateの書き込みボタンを押すだけ。  
ちなみにスレ立ては2chGear側が対応していないっぽいので不可能です。
