---
title: dateコマンドでISO 8601形式のタイムスタンプを生成する
date: "2020-06-14T02:07:53.165+09:00"
description: ISO 8601形式のタイムスタンプを、dateコマンドで生成したい。macOSのdateはナノ秒をサポートしないため、coreutilsに含まれるgdateを使用する。
---

[Gatsby's blog starter](https://github.com/gatsbyjs/gatsby-starter-blog)を使ってこのブログを構築しています。記事はMarkdownで書けるのですが、予め作成されている記事のYAMLフロントマターのタイムスタンプが、以下のような形式になっていました。

```yaml
date: "2015-05-28T22:40:32.169Z"
```

この形式は基本的にISO 8601に定められているものらしく、 `T` の左側が日付、右側が時刻となっています。 `.` の右側の3桁はミリ秒で、これはISO 8601に拡張を加えたもののようです。末尾の `Z` はタイムゾーンがUTCであることを示します。日本の場合はプラス9時間なので、以下のようになりますね。

```
2015-05-28T22:40:32.169+09:00
```

これを手入力するのは嫌なので、 `date` を使って生成しようと思います。[このページ](https://www.cyberciti.biz/faq/linux-unix-formatting-dates-for-display/)を見ながらフォーマット指定子をポチポチ組み立てていきます。以下で動くはず。

```bash
$ date +%Y-%m-%dT%H:%M:%S.%3N%:z
2020-06-14T02:56:38.3NZ
```

`.` から右側の表示が展開されていません。実はmacOSの `date` はLinuxの `date` とは[異なるもの](https://apple.stackexchange.com/a/135749)らしく、ナノ秒の `%N` には対応していない模様。 `%N` をサポートするには、GNUの `coreutils` をインストールする必要があります。

```
$ brew install coreutils
```

これで `gdate` というコマンドが使えるようになります。先ほどの内容をコマンド名だけ変更して実行します。

```bash
$ gdate +%Y-%m-%dT%H:%M:%S.%3N%:z
2020-06-14T02:59:18.462+09:00
```

上手くいきました。
