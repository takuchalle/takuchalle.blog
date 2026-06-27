---
title: "zig で []f32のデータをファイルに出力する方法"
description: "zig で []f32のデータをファイルに出力する方法"
date: "2023-06-09T16:39:57+09:00"
thumbnail: ""
categories:
  - "プログラミング"
tags:
  - "zig"
author: "takuchalle"
images: ["/img/ogp/zig-write-f32-data-to-file.png"]
---

どうも、たくチャレ([@takuchalle](https://twitter.com/takuchalle))です。

`zig`で`f32`の配列データをファイルに書き出す方法がパッと分からなかったので、メモです。

`writeAll`の引数は`[]u8`なので、`[]f32`を渡せません。 なのでキャスト、もしくは変換を行う必要があります。

標準ライブラリの`std.mem.bytesAsValue`を使うと変換することができます。

```zig
var writer = /// ファイルオープン処理
var data: []f32 = /// データ作る
try writer.writeAll(std.mem.bytesAsValue(data));
```

[実装](https://github.com/ziglang/zig/blob/0.10.1/lib/std/mem.zig#L3296)を見ると、ポインタのキャストをしていそうです。