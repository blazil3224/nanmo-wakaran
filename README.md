# nanmo-wakaran

自分の書いた説明が相手に伝わるかを、前提を知らない読み手（Haiku）に読ませて確かめ、伝わる形に書き直す Claude Code のプラグインです。
「説明がわかりにくい」と言われたときや、説明を出す前に確かめたいときに使います。

## 何をするか

Claude が書いた説明の文だけを、会話の経緯もプロジェクトの前提も知らない Haiku に読ませます。
Haiku が読み違えた箇所や理解できなかった箇所が、説明に足りない情報の手がかりになります。

1. 対象の説明の文を、そのまま抜き出す（前提や補足は足さない）
2. Haiku に読ませ、「何が言われているか」「何を頼まれたと思ったか」「分からなかった言葉」「分からなかったこと」を書かせる
3. その理解を、本来伝えたかったことと1点ずつ比べ、ずれを書き出す
4. ずれの原因になった情報（今どうなっているか・言葉の意味・何のためか・何を頼んでいるか）をまとめる
5. 1文目に答えを置き、足りない情報を足して、要らない文を削って書き直す
6. 書き直した文をもう一度 Haiku に読ませる（読ませるのは2回まで）
7. 書き直した説明だけを出す

## インストール

Claude Code で次の2つを打ちます。

```
/plugin marketplace add blazil3224/nanmo-wakaran
/plugin install nanmo-wakaran@nanmo-wakaran
```

ターミナルから入れる場合は次のとおりです。

```
claude plugin marketplace add blazil3224/nanmo-wakaran
claude plugin install nanmo-wakaran@nanmo-wakaran
```

入れたあとに開いた新しい会話から使えます。
入ったかどうかは `claude plugin details nanmo-wakaran@nanmo-wakaran` で確かめられます。

## 使い方

説明がわかりにくかったときに、次のように打ちます。

```
/nanmo-wakaran
```

対象を指さなければ、直前の Claude の説明を書き直します。特定の説明を直したいときは、その説明を指して頼みます。

```
/nanmo-wakaran さっきの PR の説明
```

「わかりにくい」「何もわからん」と伝えたときに、Claude が自分から使うこともあります。

使う前と後の例:

- 前: 「Hook の matcher が Edit にしか当たっていないので、Write 経由の変更が拾えていません」
- 後: 「ファイルを新しく作ったときに、チェックが動いていません。チェックを動かす条件が『既存ファイルの書き換え』だけになっているためです。新規作成も条件に足せば直ります」

## ファイル構成

```
.claude-plugin/marketplace.json            配布元の情報
plugins/nanmo-wakaran/
  .claude-plugin/plugin.json               プラグインの名前・版・説明
  skills/nanmo-wakaran/SKILL.md            手順を書いたスキル（メインの Claude が読む）
  agents/nanmo-wakaran.md                  前提なしで読む役のエージェント（Haiku で動く）
```

エージェントは、プラグインに入れると `nanmo-wakaran:nanmo-wakaran` という名前で呼ばれます。スキルはこの名前でエージェントを呼びます。

## 注意

- 1回の説明につき、Haiku を最大2回呼びます。その分の利用量がかかります
- 読み手のエージェントには、渡した説明の文しか見せません。ファイルも CLAUDE.md も読ませません
- スキルとエージェントの中身は日本語です。日本語の説明を直す前提で作っています
- リポジトリが非公開のあいだは、招待された人しか入れられません

## ライセンス

MIT License。詳しくは [LICENSE](./LICENSE) を見てください。
