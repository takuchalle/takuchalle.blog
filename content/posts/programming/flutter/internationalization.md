---
title: "Flutter の多言語対応"
description: "Flutter の多言語対応した時のメモです。"
date: "2022-02-21T22:38:46+09:00"
categories:
  - "プログラミング"
tags:
  - "Flutter"
author: "takuchalle"
images: ["/img/ogp/internationalization.png"]
---

どうも、たくチャレ([@takuchalle](https://twitter.com/takuchalle))です。

Flutterで作ってるアプリの多言語対応した時のメモです。
多言語対応といっても今は日本語しかサポートしていません。例え1言語しかサポートしなくてもソースコードに直値があるのは気持ち悪いので、この対応を行うことでソースコードがスッキリすると思います。

公式サイトに詳しく記載されているので、英語に抵抗がない方・詳しく見たい方はこちらを参照してください。

{{< card url="https://docs.flutter.dev/development/accessibility-and-localization/internationalization" >}}

## 多言語対応方法
### `flutter_localizations`をインストールする

Flutterはデフォルトでは英語しかサポートしていないので、`flutter_localizations`パッケージをインストールことで他の言語をサポートすることができます。
サポートされている言語は[こちら](https://api.flutter.dev/flutter/flutter_localizations/GlobalMaterialLocalizations-class.html)で確認できます。

`pubspec.yaml`に記載します。

```yaml
dependencies:
  flutter:
    sdk: flutter
  flutter_localizations: # 追加
    sdk: flutter         # 追加
```

インストールすると下記のように`MaterialApp`の`localizationsDelegates`と`supportedLocales`を指定します。

```dart
import 'package:flutter_localizations/flutter_localizations.dart';

return const MaterialApp(
  title: 'サンプル',
  localizationsDelegates: [
    GlobalMaterialLocalizations.delegate,
    GlobalWidgetsLocalizations.delegate,
    GlobalCupertinoLocalizations.delegate,
  ],
  supportedLocales: [
    Locale('ja', ''), // 日本語追加
    Locale('en', ''), // 英語追加
  ],
  home: MyHomePage(),
);
```

Widgetのbuildメソッド内で`MaterialLocalizations.of(context)`で`MaterialLocalizations`にアクセスできるので、

```dart
Text(MaterialLocalizations.of(context).cancelButtonLabel)
```

と記述すると日本語の場合`キャンセル`、英語の場合`Cancel`と表示されます。[^1]

Widgetでよく使われる単語は`MaterialLocalizations`に定義されているので自分で用意する必要はありません。
こちらにすべて定義されているので、一度見てみることをオススメします。命名ルールも参考になるかと思います。

{{< card url="https://api.flutter.dev/flutter/flutter_localizations/MaterialLocalizationJa-class.html" >}}

### `intl`をインストール

ここから自分で定義した単語や文章を使用する準備をします。

大まかな流れは以下のようになります。
1. パッケージをインストールする
1. `.arb`拡張子の定義ファイルを用意する
1. `.dart`に変換する
1. `MaterialApp`に追加する
1. Widgetから参照する

まずいつもどおり`pubspec.yaml`に`intl`を追加します。

```yaml
dependencies:
  flutter:
    sdk: flutter
  flutter_localizations:
    sdk: flutter
  intl: # 追加
```

更に`flutter`セクションに`generate: true`も追加します。
これでビルド時に`.arb`から`.dart`に変換してくれます。
```yaml
flutter:
  generate: true
```

次に変換するための定義ファイルを作成します。
ルートディレクトリに`l10n.yaml`ファイルを作成し、以下のように書きます。

```yaml
arb-dir: lib/l10n # .arb を配置する任意のディレクトリ
template-arb-file: app_ja.arb # 日本語を定義するファイル
output-localization-file: app_localizations.dart # 変換後のファイル名
```

`lib/l10n/app_ja.arb`の中身を書いていきます。

```arb
{
    "helloWorld": "こんにちわ、世界",
    "@helloWorld": {
      "description": "生まれたてのプログラマの最初の挨拶です"
    }
}
```

VSCode を使用しているなら、[Flutter Intl](https://marketplace.visualstudio.com/items?itemName=localizely.flutter-intl)というプラグインで`.arb`ファイルの色付けとかしてくれるので便利です。

今後言語を増やしていきたい場合は、`app_ja.arb`の`ja`の部分を言語コードにして定義ファイルを書いていけば良いです。

ここまでの準備ができたらアプリをビルドします。すると`${FLUTTER_PROJECT}/.dart_tool/flutter_gen/gen_l10n`に`.arb`から変換された`app_localizations.dart`が生成されているはずです。

生成されたファイルは`flutter_gen/gen_l10n/app_localizations.dart`で参照できるのでインポートして、`AppLocalizations.delegate`を`localizationsDelegates`に指定します。
ひとまとめにしたコードがこちらです。

```dart
import 'package:flutter_localizations/flutter_localizations.dart';
import 'package:flutter_gen/gen_l10n/app_localizations.dart';

return const MaterialApp(
  title: 'サンプル',
  localizationsDelegates: [
    AppLocalizations.delegate, // 追加
    GlobalMaterialLocalizations.delegate,
    GlobalWidgetsLocalizations.delegate,
    GlobalCupertinoLocalizations.delegate,
  ],
  supportedLocales: [
    Locale('ja', ''), // 日本語追加
  ],
  home: MyHomePage(),
);
```

最後にWidget内で以下のように参照して表示できれば完成です。

```dart
Text(AppLocalizations.of(context)!.helloWorld);
```

`MaterialApp`は以下のように書くこともできます。これらは自動生成されるので、新規の言語の`.arb`ファイルを追加するだけで、ソースコードは修正せずに新規の言語に対応できます。

```dart
const MaterialApp(
  title: 'サンプル',
  localizationsDelegates: AppLocalizations.localizationsDelegates,
  supportedLocales: AppLocalizations.supportedLocales,
);
```

[^1]: もう少し短く書けるといいが…