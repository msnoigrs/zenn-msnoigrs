---
title: "記事を書く"
---
記事を書く場合、一つの記事を一つのOrg modeファイルに記述します。Org modeのメタデータの書式を使ってZennの記事に必要なフロントマター情報を記述します。

以下は記事のOrgファイル記述例です。

```
  #+TITLE: Zennの記事をOrg Modeで書く
  #+OPTIONS: ^:t
  #+ZENN_TYPE: idea
  #+ZENN_EMOJI: 🌻
  #+KEYWORDS: orgmode, emacs

  * 見出し1

  内容
```

記事を書く場合に有効なメタデータは以下の通りです。

- **TITLE:** title:キーになります。記事のタイトル。必須。
- **ZENN_TYPE:** type:キーになります。 `tech` か `idea` 。指定しない場合、 `org-yazenn-article-type` 変数の値が使用されます。デフォルトは `tech` 。
- **ZENN_EMOJI:** emoji:キーになります。指定しない場合、 `org-yazenn-article-emoji` 変数の値が使用されます。
- **KEYWORDS:** topics:キーになります。カンマ区切り。Zennの仕様により、各トピック文字列に記号やスペースを含めることはできません。必須。
- **ZENN_PUBLISHED:** published:キーになります。 `true` か `false` 、 `t` か `nil` 。デフォルトは `true` 。
- **ZENN_PUBLISHED_AT:** published_at:キーになります。 `yyyy-mm-dd HH:MM` か  `yyyy-mm-dd` 。
- **EXPORT_FILE_NAME:** このキーワードを指定しない場合、生成されるMarkdownのファイル名はOrgファイルの名前と同じになりますが、意図的に指定したい場合にこのキーワードが利用できます。

参照

- [Org mode公式 Setting up keywords for individual files](https://orgmode.org/manual/Per_002dfile-keywords.html)
- [Org mode公式 Drawers](https://orgmode.org/manual/Drawers.html)
- [Org mode公式 Property Syntax](https://orgmode.org/manual/Property-Syntax.html)
- [Org mode公式 Export Settings TITLEやEXPORT_FILE_NAMEについて、OPTIONSについて](https://orgmode.org/manual/Export-Settings.html)
- [Zenn公式 記事の作成](https://zenn.dev/zenn/articles/zenn-cli-guide#%E8%A8%98%E4%BA%8B%E3%81%AE%E4%BD%9C%E6%88%90)
