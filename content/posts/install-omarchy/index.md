+++
date = '2026-06-23T14:28:58+09:00'
title = 'Omarchy をインストールした'
+++

今まで開発環境は Ubuntu を使ってたんだけど違うディストロを使ってみたい欲が湧いてきて調べてたら Omarchy が気になった。

{{< article link="https://omarchy.org/" showSummary=true compactSummary=true >}}

Ruby on Rails の開発者である DHH が作ったディストロビューションで、Arch Linux + Hyprland をベースに予め開発用のソフトウェア群がインストールされてる。

前から Arch Linux は気になってたけどインストールが難しそうだなぁと思っていたけど、Omarchy だとあっさりインストールできた。インストール完了した時にインストールにかかった時間が表示されて面白いと思った。正確な時間は忘れたけど20分もかからなかったと思う。

## 日本語表示

Mozc を使っていく。

```sh
sudo yay -S --noconfirm --needed fcitx5-im fcitx5-mozc
```

でツールをインストールして、

```sh
fcitx5-configtool
```

で設定を行う。

`Input Method`のタブで Mozc を`Current Input Method`に登録する。`Global Options`タブの`Enumerate Input Method Forward`でホットキーを登録することができる。私はAlt+`にしている。

## zsh

ログインシェルはbashになってるのでzshに変更する。とはいえすごく凝った設定をしているわけではない。zshはインストールされてないのでインストールから。

```sh
sudo pacman -S zsh
which zsh
chsh # which zsh で表示されたパスを指定する
# 再起動する
```

## neovim

デフォルトでは[LazyVim](https://www.lazyvim.org/)が有効になっている。高機能過ぎて使いこなせないので自分でセットアップした。
ついでにdotfilesを管理するためのツール[chezmoi](https://www.chezmoi.io/)も使ってみる。

{{< github repo="takuchalle/dotfiles-chezmoi" showThumbnail=true >}}

育てていく。

## 感想

使い始めて1,2週間経つけどすごい体験がいい。

Hyperland で初めてタイル型のウィンドウマネージャを初めて使ったけどヌルヌル動いて気持ちがいい。気持ちがいいだけじゃなくてウィンドウの操作がキーボードで完結する。慣れるまで間違えてウィンドウ閉じちゃったりあわあわしたけど、慣れてしまえば快適そのもの。

デスクトップにWindowsとデュアルブートする形でインストールしたけど、気にったので古くなったIntel MacBook にもインストールした。MacBook の方は閉じたときのサスペンドの処理が微妙で復帰できないことがあるけど、起動が爆速なので作業終わったらシャットダウンする運用で全然問題ない。

もうUbuntuに戻れない。
