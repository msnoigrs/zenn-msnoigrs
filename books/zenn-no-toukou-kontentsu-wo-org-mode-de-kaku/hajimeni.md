---
title: "はじめに"
---

# Zennの投稿コンテンツをOrg Modeで書く

この文書は、Zennで公開する文書をemacsのOrg modeで書きたい人に向けて書きました。

Org modeにはOrg modeの形式から別の形式に変換する仕組みが備わっています。（参照 [Org mode公式 Exporting](https://orgmode.org/manual/Exporting.html)）
この仕組みは、バックエンドを用意することで様々な形式に対応できるようになっています。
この度Zenn用のMarkdownを生成するバックエンドを書きましたので、ドッグフーディングを兼ねてZenn本を作りました。

書いたバックエンドは `ox-yazenn` という名前で、[Githubで公開](https://github.com/msnoigrs/ox-yazenn)しています。
`ox-yazenn` を利用すると、Org Modeの文書からZennの記事(Articles)や本(Books)の形式に対応したMarkdownを生成することができます。 `ox-yazenn` は[ZennのMarkdown記法一覧](https://zenn.dev/zenn/articles/markdown-guide)に対応したMarkdownを生成します。Zenn固有の記法にも対応しています。Zenn固有の表現を意識してOrg modeを書いた場合でも、他のバックエンドと互換性を維持するように設計してあります。
この文書では `ox-yazenn` の使い方を中心に説明します。
