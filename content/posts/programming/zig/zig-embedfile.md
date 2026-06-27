---
title: "zig でファイル読み込みは@embedfileが便利"
description: "zig でファイル読み込みは@embedfileが便利という話"
date: "2023-06-06T14:58:38+09:00"
thumbnail: ""
categories:
  - "プログラミング"
tags:
  - "zig"
author: "takuchalle"
images: ["/img/ogp/embedfile.png"]
---

どうも、たくチャレ([@takuchalle](https://twitter.com/takuchalle))です。

簡単なスクリプトを`zig`で書いてみた時に、`@embedfile`が便利だったので紹介です。

## 環境

```bash
$ zig version
0.11.0-dev.3380+7e0a02ee2
```

ドキュメントは[こちら](https://ziglang.org/documentation/master/#embedFile)です。

## 基本的なファイル読み込み

以下のコードは[こちらの記事](https://zenn.dev/slowhand/articles/88888e94504f27)からの引用ですが、こんな感じでちょっとファイルを読みたいだけなのに記述するコードが多いです。

```zig
const std = @import("std");

pub fn main() !void {
    const stdout = std.io.getStdOut().writer();

    const file_name = "xxxx";
    const file = try std.fs.cwd().openFile(fileName, .{});
    defer file.close();

    const file_size = try file.getEndPos();
    try stdout.print("file size: {d}\n", .{file_size});

    var reader = std.io.bufferedReader(file.reader());
    var instream = reader.reader();

    const allocator = std.heap. page_allocator;
    const contents = try instream.readAllAlloc(allocator, file_size);
    defer allocator.free(contents);

    try stdout.print("read file value: {c}\n", .{contents});
}
```

## @embedfile を使う

同様のことを`@embedFile`を使って行います。

```zig
const std = @import("std");

pub fn main() !void {
    const stdout = std.io.getStdOut().writer();
    const inputFile = @embedFile("xxxx");

    try stdout.print("file size: {d}\n", .{inputFile.len});
    try stdout.print("read file value: {s}\n", .{inputFile});
}
```

`@embedFile`に指定したファイルをコンパイル時に読み込んで配列に格納してくれるので、ファイル操作のエラー処理などを実行時に行う必要がなくなります。

コンパイル時にファイルがないとコンパイルエラーになりますが、ちょっとしたスクリプトやテストコードでは`@embedFile`で十分なケースが多いかと思います。