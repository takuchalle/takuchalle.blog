---
title: "MINISFORUMのUM773にデュアルブートでUbuntuをインストールした"
description: "MINISFORUMのUM773にデュアルブートでUbuntuをインストールした"
date: "2023-05-25T18:50:41+09:00"
thumbnail: ""
categories:
  - "日記"
tags:
  - "Ubuntu"
  - "PC"
author: "takuchalle"
images: ["/img/ogp/ubuntu-dualboot-on-um773.png"]
---

どうも、たくチャレ([@takuchalle](https://twitter.com/takuchalle))です。

前から気になっていた`MINISFORUM`の`UM773 lite`がセールになっていたのでポチってしまいました。

[Amazonの商品ページはこちら](https://amzn.to/434LXxq)(セールはもう終わりました)

マウスと比べると一回り大きい程度なので非常にコンパクトです。

{{< figure src="IMG_1589.jpg" >}}

昔は家に`CentOS`で自宅サーバーを設置したりしてましたが、最近ではクラウドのサービスを利用することが増えてきました。
しかし、手元に`Linux`の開発機も持ちたいなと徐々に思い始めて、今回買って`Ubuntu`をインストールしました。

`Windows`もあったら便利かなということでデュアルブートしてみました。

## デュアルブート

今回は`Windows`と`Ubuntu`で物理的にドライブを分けました。家に余っていた[Crucial SSD 500GB MX500](https://amzn.to/3qfPv1b)を増設して、ここに`Ubuntu`をインストールしました。
`7mm`以下の`SSD`なら増設できるようです。`UM773`に`SSD`と接続するケーブルとマウントするネジが同根されているので、プラスドライバーだけあれば簡単に増設できます。

`Ubuntu`のインストール方法は特筆する必要もないくらい簡単でした。今回ドライブを分けましたが、同じドライブでパーティションを分けたインストールも簡単そうでした。

## なぜUbuntuか

インストールしたらすぐに使えるからです。`Gentoo`とか`ArchLinux`とか使いこなすとかっこいいけど、欲しかったのは`Linux`の開発機なのでインストールが簡単ですぐに使い始められる`Ubuntu`にしました。

バージョンは`23.04`にしました。カーネルが`6.2`なので`Rust`も使えます。

久々に`dotfiles`を整理したり、いろいろ楽しみたいと思います。
