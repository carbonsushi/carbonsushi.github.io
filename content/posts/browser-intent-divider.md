---
title: "Browser intent dividerを作った"
categories: ["Android"]
---

いつの間にか実機でIntent（正確には`Intent.ACTION_VIEW`の暗黙的インテント）がまともに機能しなくなってしまった。  
MIUIの妙なカスタムが悪いのか、はたまたブラウザーが悪いのかまったくわからない。

アプデで修正されると思っていたが一向に修正されない。  
なのでブラウザーとして振る舞い、Intentを適切なアプリに振り分けるアプリを作った。  
<https://github.com/carbonsushi/browser-intent-divider>

## 使い方

設定 → アプリ → デフォルトのアプリ → ブラウザアプリ → 「Browser intent divider」を選択するだけ。  
誤作動があるのでその時は設定の「Exclude package list」・「Priority package list」を弄ればOK。  
「Exclude package list」よりも「Priority package list」が優先されます。
