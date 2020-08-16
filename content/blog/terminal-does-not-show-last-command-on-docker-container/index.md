---
title: Dockerコンテナで直前のコマンド履歴が表示されない
date: 2020-08-12T09:51:43.732Z
description: Dockerコンテナでcontrol + Pを押しても、一つ前のコマンドが表示されない。これはDockerの方で、同じショートカットに別の処理が割り当てられているため。
---

コマンドの履歴を表示するためには `control + P` を押します。ただDockerコンテナ内では表示されず、もう一度押すと2つ前の履歴が表示されます。直前のコマンドを再実行することが一番多いと思うのでストレスです。

どうもDockerの方で `control + P` が[別の処理に割り当てられているのが原因](https://qiita.com/Yuki-Inoue/items/60ec916383025160fbb8#_reference-a2d9244a6c4496f4df05)とのこと。ターミナルを多用するツールなのに、このキーマップはあり得ない…。

上の記事の通りに `~/.docker/config.json` でキーマップを変更します。  `jq` と `sponge` がインストールされていれば、以下のコマンドで追加できます。

```bash
$ cat ~/.docker/config.json | jq '. + {"detachKeys": "ctrl-\\\\"}' | sponge ~/.docker/config.json
```

`jq` と `sponge` のHomebrewでのインストールは以下の通り。

```bash
$ brew install jq moreutils
```
