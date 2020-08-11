---
title: ターミナル起動時のLast loginメッセージを非表示にする
date: 2020-08-12T02:28:07.651+09:00
description: ターミナル起動時に表示される前回のログイン日時は、ホームディレクトリに空ファイルを作成することで抑制できる。
---

ターミナルを起動した際に、最初のプロンプトが返る前に、通常は以下のようなメッセージが表示されます。

```
Last login: Wed Aug 12 02:22:15 on ttys000
```

これを消すには以下のコマンドを実行して、空ファイルを作成します。

```bash
$ touch ~/.hushlogin
```

hushは、[人差し指を口の前に立てる仕草](https://www.google.com/search?q=hush+gesture&tbm=isch)にも使われる言葉のようです。

`ttysXXX` は[擬似端末に割り振られる識別子](https://superuser.com/questions/812086/ttysxxx-in-who-command)で、次のコマンドで一覧を確認できます。

```bash
$ who
ikhr     console  Aug 12 02:21
ikhr     ttys000  Aug 12 02:48
```
