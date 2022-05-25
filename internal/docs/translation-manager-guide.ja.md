# quarkus.io 翻訳プロジェクト 翻訳マネージャー向けガイド

※ このガイドは、ローカライゼーションチームの翻訳マネージャー向けのガイドです。 一般の翻訳参加者は [翻訳ガイド](../../translation-guide.ja.md) を参照して下さい。

## 翻訳方式

[quarkus.io](https://quarkus.io)はJekyllを用いた静的サイトであり、そのコンテンツはコンテンツは asciidoctor (.adoc) で記述されています。
レポジトリは [quarkusio/quarkusio.github.io](https://github.com/quarkusio/quarkusio.github.io ) に存在し、CC BY 3.0 に基づき公開されています。
本プロジェクトでは、.adocファイルからpo4aというユーティリティを用いてテキストを抽出し、翻訳して.adocファイルに書き戻してビルドすることで日本語版サイトを構築する方式を採っています。
po4aを用いたテキストの抽出、翻訳メモリを用いた訳文の適用、機械翻訳による下訳、書き戻し処理のワークフローは、本レポジトリのGitHub Actionsによって自動化されており、
抽出されたテキストは翻訳テキストを管理するファイル形式である、.poファイルとして、[l10nディレクトリ](../../l10n) 以下に保存されています。

## 翻訳対象の選定

[l10nディレクトリ](../l10n) 配下の.poファイルすべてを将来的には翻訳していきたいところですが、まずは主要なコンテンツである「ガイド」と
「ブログ」を翻訳する為に、[l10n/po/ja_JP/_guidesディレクトリ](../../l10n/po/ja_JP/_guides) と
[l10n/po/ja_JP/_postsディレクトリ](../../l10n/po/ja_JP/_posts) ディレクトリを中心に翻訳を進めています。

ローカライゼーションチームは、未翻訳Word数の多いファイルを中心に取り組むという方針になりましたが、
[未翻訳Word数順にファイル名をソートしたリスト](../../l10n/stats/translation.csv)を用意したので、こちらのリストの上から着手下さい。

## 翻訳マネージャーのワークフロー

### 翻訳着手の宣言

同じファイルを複数のメンバが同時に作業着手し、重複して作業して無駄が発生するのを避ける為、翻訳作業に着手する際は、
GitHubのIssueで、どのファイル、範囲を対象に作業着手するか宣言をお願いします。
Issueを立てる際は逆に既存のIssueで既に誰かが対象に対して作業着手を宣言していないか、ご確認下さい。

### gitレポジトリの最新化

作業着手にあたり、手元にダウンロードした.poファイルをGitHub上のファイルと同期し、最新化する必要があります。
以下のコマンドを実行し、手元のレポジトリの `main` ブランチををGitHub上のja.quarkus.ioレポジトリの `main` ブランチと同期してください。

```
# 手元のja.quarkus.ioレポジトリのディレクトリで作業して下さい
#
# 手元のja.quarkus.ioレポジトリのブランチをmainブランチに切替
git checkout main
# GitHub上の ja.quarkus.io レポジトリの変更内容をダウンロード
git fetch origin
# 手元のja.quarkus.ioレポジトリの内容を GitHub上の ja.quarkus.io レポジトリの `main` ブランチと同期
git reset --hard origin/main
```

### 翻訳作業

.poファイルは、テキストファイルですが、様々な翻訳補助ツールが編集に対応しています。
Memsourceを利用し、Memsource上のワークフローで翻訳作業を進められる場合は、
.poファイルをMemsourceにアップロードし、翻訳作業を進め、翻訳が完了したらダウンロードして元のファイルを更新して下さい。

Memsourceをご利用される場合、各センテンスの「翻訳要確認」フラグの除去が行われないようなので、
翻訳作業後の.poファイルをPOEditで開き、全翻訳センテンスを選択の上、「編集」メニューから「翻訳要確認」フラグを外して下さい。

### 翻訳結果の送信

翻訳した成果物は、GitHub上にアップロードし、レビューを受けた上で元のja.quarkus.ioレポジトリに取り込んでもらい、サイトを更新する必要があります。
GitHubでは、レポジトリに対して変更を取り込んでもらうよう提案する為の単位を「Pull-Request」と呼称しており、「Pull-Request」を作成する必要があります。
「Pull-Request」を作成するには、変更が含まれた「ブランチ」が必要であり、「ブランチ」にはファイルの変更をまとめた「コミット」で構成されています。

#### ブランチの作成

ブランチを以下の手順に従い、作成してください。

```
# "gitレポジトリの最新化"の手順が完了しており、"main"ブランチにいる前提
#
# 手元のja.quarkus.ioレポジトリで新たに <ブランチ名> というブランチを作成。<ブランチ名>は対象のファイル名等、適宜被らない名称とする必要があります。
git checkout -b <ブランチ名>
# GitHub上の ja.quarkus.io レポジトリの変更内容をダウンロード
```

#### コミットの作成

コミットを以下の手順に従い、作成してください。これにより、現在のブランチにコミットが追加されます。

```
# コミットを作成。<コミット メッセージ>は"Translate <翻訳対象ファイル名>"等が適当でしょう
git commit -a -m "<コミット メッセージ>"  
```

#### ブランチの送信

続いて、GIｔHub上にブランチを以下のコマンドで送信して下さい。

```
git push origin <ブランチ名>
```

#### Pull-Requestの作成

ブランチの送信が完了したら、 [GitHub上のPull-Request一覧ページ](https://github.com/quarkusio/ja.quarkus.io/pulls) に移動して下さい。
ブランチの送信がされた旨の通知が出ていると思いますので、その通知の「Compare & pull request」ボタンを押下し、Pull-Request作成画面から
Pull-Requestを作成して下さい。


