---
title: "動的に読み込まれるDexをダンプするDexDumperを作った"
categories: ["Android"]
---

とあるアプリのリバースをしていたところ動的にDexを生成して読み込まれてそうなクラスを発見。  
良さげな既製モジュールを発見するも謎エラーで動かず。  
<https://github.com/rednaga/DexHook>

なので適当に作った。  
~~ストレージ関連面倒なのでminSdk29にしちゃいました。~~  
<https://github.com/carbonsushi/dexdumper>

## 使い方

Xposedマネージャーで該当アプリを選択して起動するだけ。  
後は自動で`/sdcard/Download/`（`MediaStore.Downloads`）にダンプされる。

`dalvik.system.DexFile`しか対応してないのでダンプできないDexもあるかも。
