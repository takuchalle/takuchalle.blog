---
title: "Ubuntu で GitHub アクセストークンを保存する方法"
description: "Ubuntu でセキュアに GitHub アクセストークンを保存する方法"
date: "2022-01-13T11:12:12+09:00"
categories:
  - "プログラミング"
tags:
  - "GitHub"
  - "Ubuntu"
author: "takuchalle"
images: ["/img/ogp/secure-store-token.png"]
---

どうも、たくチャレ([@takuchalle](https://twitter.com/takuchalle))です。

毎回 GitHub のアクセストークンをペーストするのが面倒なので、保存方法を調べてみました。

## 確認環境

* Ubuntu 20.04.3 LTS
* git 2.25.1

## 平文で保存

一番簡単な方法は平文で保存する方法です。

```
git config --global credential.helper store
```

個人の PC など自分しか触らない環境であれば平文でも良いかもしれませんが、少し不安です。
できれば暗号化したほうがいいと思います。

## libsecret で暗号化して保存

`libsecret`を使って暗号化します。
以前は`libgnome-keyring`が使われていたらしいですが、こちらは非推奨になり`libsecret`を使うことが推奨されています。

```
sudo apt install libsecret-1-0 libsecret-1-dev
sudo make --directory=/usr/share/doc/git/contrib/credential/libsecret
```

`apt`でインストールしたあとにビルドが必要です。ビルドできない場合は`build-essential`も入れる必要があるかもしれません。

```
sudo apt install build-essential
```

ビルドが成功すれば、下記コマンドで設定します。

```
git config --global credential.helper /usr/share/doc/git/contrib/credential/libsecret/git-credential-libsecret
```

設定した後一回目はアクセストークンを入れる必要がありますが、二回目以降は保存されたアクセストークンが自動で使われます。