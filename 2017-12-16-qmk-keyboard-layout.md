---
title: "qmk-firmware でのキー配列"
date: 2017-12-16
---

## まえおき

この記事は[自作キーボード Advent Calendar 2017](https://adventar.org/calendars/2114)の16日目です

昨日は@ntoofu が作っているキーボードで紹介に預かりました。ハード側には詳しくないので、キーマップとかの方をがんばっています

主に ErgoDox と HHKB(Alternative Controller) を使用しています

## キー配列でのこだわり

以下はUS 配列を前提にします。

### QWERTY ベースであること

Dvorak とかオレオレ配列とかにすると、仮に慣れることができたとしても人間に戻れなくなるので、QWERTY から大きく外れないようにしています

外出先でノートを使う際には OS 側でリマップをしなければなりませんが、OS 側で再現させるのが大変、という理由もあります

### IME ON/OFF はトグルキーではなくそれぞれキーを用意する

通常の Windows JIS 配列のようにIME の状態を脳で記憶するのは非常に負担です

Mac OS JIS 配列のかな/英数キーのように、ワンショットでそれぞれの状態に変更するキーを作ると劇的に楽になります

Mac で Cmd のワンショットに割りあてる話をよく見ますが、自分はそれぞれ左右の Shift キーのワンショットを IME 変更に割り当てています

(そもそも左右の Shift すら Shift ではないのですが……)

こちらについては昨年、[ErgoDox「この中でいらないキーにはIME 担当になってもらいまーーーすwww」](https://qiita.com/euxn23/items/c78c40869e4870690910)という記事に詳細が書いてありますが、 HENK/MHEN キーを使って OS 側で制御する等しないといけなさそうです

### メタキーレイヤーでの機能は最小限にし、ベースレイヤーをなるべく引き継ぐ

レイヤー切り替えができるとついついメタキーレイヤーを作りがちですが、何かあるたびにメタキーを押す、というのも疲れるものです

可能であれば、ベースレイヤーと、そこに収まらないキーを置くレイヤーの2つにし、メタキーレイヤーに持たせる役割は少ない方が、なんだかんだで楽でした

自分は Emacs のカーソルをメタキーレイヤーに入れているくらいになりました

### Space キーに Shift を兼ねさせ、小指を解放する

これは私がもう人間に戻れなくなってしまった部分なのですが、 Space キーの長押しに Shift を割りあてるという、いわゆる SandS をやっています

Space の連打とか、Shift の空押しというキーバインドが使えなくなってしまいますが、まああまり弊害はありません

通常の Shift キーは小指で押すと思うのですが、左小指は Ctrl 、右小指は Enter に使うため、Shift と兼ねられなくなるという問題があります(ここで薬指を持ち出すとポジションが大きく崩れるのでつらい)

### Ctrl と GUI を入れ替えるキーを作る

母艦はゲーム用の Windows と開発用の Linux 、ノートは Mac という状況なので、 OS コンパチでないといけません

ただし、 qmk-firmware における GUI が Mac における Cmd に割当たるため、Ctrl だと問題が出てきます

(非 Mac ユーザ向けに説明すると、 Mac ではコピペとかは　Ctrl + C などではなく Cmd + C です。しかしターミナルではCtrl + C でプロセス終了となります。邪悪だなァーーー)

Mac の OS 標準機能でキーの入れ替えができるものの、なかなかつらいというのが現実。なのでキーボード側で入れ替えられるキーを作ることで解決しようとしました

こちらの切り替えもトグルではなくワンショットで行うことで、小指の位置にあるキーを Ctrl に Cmd にいつでも切り替えられるので楽になります

特にこのような半永久的なレイヤー切り替えは、OS 側で行うにはつらいものです

### モディファイヤキーを各所に配置する

長押しすることがない Esc や Tab 、Enter 等は、長押しでモディファイヤにとにかく割り当てていきます

モディファイヤが複数あればその時に近い指のキーで推せるので、非常に楽になります

## ソース

[github.com/euxn23/qmk-firmware](github.com/euxn23/qmk-firmware) にあります