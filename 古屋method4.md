# 古屋method4
キー設定について(2020-03-14)

# ChangeKey
古屋method3でちょっと紹介した[Change Key](https://forest.watch.impress.co.jp/library/software/changekey/)

親指シフトでもしない限りいらないキーは有効活用したほうがいいですよね 特に親指まわり｡
[親指シフトってなに? かな入力・ローマ字入力とも違う「指がしゃべる」入力方式 | ヨッセンス](https://yossense.com/oyayubi-shift/)


今の私の設定がこちら
![](2020-03-14-14-11-49.png)

Enterは親指で打てるととても楽です｡
ただ､どれをどうするかは完全に好みだとは思います｡
Enterをスペースの右にするとか

ちなみにScaneCodeはF13になるようにしてます｡
理由は後述｡
[ChangeKeyを使用してWindowsキーをF13キーに変更する方法 - 情報科学屋さんを目指す人のメモ（FC2ブログ版）](http://did2.blog64.fc2.com/blog-entry-349.html)


# AutoHotKey
方向キー よく使うキーなんですけど遠い｡｡

なんとかならんもんかと3日くらい悩みました｡
悩んだ理由としては､方向キー以外に使いたいキーとの兼ね合いがあったからです｡

半/全 delete あたりが欲しいでも足りない

何か+HJKLで方向キーはvimの関係で確定してたんですが､組み合わせをどうするかでめちゃくちゃ悩みました｡
[知識0から始めるVim講座 - Qiita](https://qiita.com/JpnLavender/items/fabcc79b4ab0d52e1f6d)


とりあえずの結論がこちら
古屋method2で紹介したAutoHotKey使ってます｡
[AutoHotkey Wiki](http://ahkwiki.net/Top)

```
;方向キー
F13 & H::Send,{Blind}{Left}
F13 & J::Send,{Blind}{Down}
F13 & K::Send,{Blind}{Up}
F13 & L::Send,{Blind}{Right}

;Alt+方向キー
LAlt & H::Send,{Blind}{Lalt}{Left}
LAlt & J::Send,{Blind}{Lalt}{Down}
LAlt & K::Send,{Blind}{Lalt}{Up}
LAlt & L::Send,{Blind}{Lalt}{Right}

;日英変換
LAlt & Backspace::Send {vkF3sc029}


;Deleteキー
Shift & Backspace::Send {Delete}
```

* 方向キーはF13+hjkl
F13はChangeKeyで作ったものです
* 地味にAlt+方向キーを使うので(VSCodeにて)F13抜きで行けるようにAlt+hjkl

* 親指だけで日英切り替えたかったので左alt+Backpackで対応
(打ち間違いしてたらBackspace打ちたくなるかなという理由もある)

* DeleteキーはShift+Backspaceで｡
完全に気分です｡


Twitterで見かけたんですが､
Ctrlの空打ちで日英切り替え という案はいいな思いました｡

