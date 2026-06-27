---
title: "Flutter でプロジェクトを作成してまず行ったこと"
description: "Flutter でプロジェクトを作成してまず行ったことを紹介します。"
date: "2022-02-21T18:39:41+09:00"
categories:
  - "プログラミング"
tags:
  - "Flutter"
author: "takuchalle"
images: ["/img/ogp/after-create-project.png"]
---

どうも、たくチャレ([@takuchalle](https://twitter.com/takuchalle))です。

久々に Flutter プロジェクトを作成したので、まず最初に行ったことを紹介します。

## VS Code にスニペットを導入

前はAndroid Studioを使っていたのですが、重かったので今回はVS Codeを使ってみることにしました。

* [Awesome Flutter Snippets](https://marketplace.visualstudio.com/items?itemName=Nash.awesome-flutter-snippets)
* [Flutter Riverpod Snippets](https://marketplace.visualstudio.com/items?itemName=robert-brunhage.flutter-riverpod-snippets)

ひとまず上記の2つのプラグインを入れてみました。
今のところ困ってませんが、必要に応じて自分でスニペットは追加していこうと思っています。[^1]

## リンターの設定

リンターはオススメの書き方や良くない書き方を教えてくれるツールです。人間いくら気をつけてもミスをするので、機械的にチェックしてもらうのが良いと思います。

前は自分で設定をしないといけなかったけど、[バージョン2.5](https://medium.com/flutter/whats-new-in-flutter-2-5-6f080c3f3dc)からプロジェクト作成時に設定してくれるようになったのですね。

[flutter_lints](https://pub.dev/packages/flutter_lints)というルールがデフォルトで指定されています。
`analysis_options.yaml`の`rules`セクションに自分の好きなルールを追加できますが、ひとまずデフォルトを使ってみます。

ルールは以下のサイトにすべて載ってますが、数がめちゃめちゃ多いです。
`style flutter`のタグがついているルールは先程の`flutter_lints`に入っているので、それ以外を見ると良いと思います。

{{< card url="https://dart-lang.github.io/linter/lints/" >}}

## CI の整備

GitHub Actionsでビルドとテストは設定しました。
自動デプロイとかはもうちょっとアプリが形になってからでいいかなと思ってます。

やり方は検索すればたくさん出てくるので、ここでは特に書きません。

## 多言語

多言語対応する予定はなくても、コードにベタ書きはイヤだったので公式のドキュメントを参考にやりました。

{{< card url="https://docs.flutter.dev/development/accessibility-and-localization/internationalization" >}}

これも前はもっと面倒だった気がしますが、楽になりましたね。
私が対応した時のメモも貼っておきます。

{{< card url="https://techlog.takuchalle.dev/post/2022/02/21/internationalization/" >}}" >}}

[^1]: パッと見た感じVS Code のスニペットってめちゃめちゃ書きにくそう