---
title: "本を書く"
---
本を書く場合、[1冊分すべてを一つのOrg modeファイルに記述する](#1%E5%86%8A%E5%88%86%E3%81%99%E3%81%B9%E3%81%A6%E3%82%92%E4%B8%80%E3%81%A4%E3%81%AEorg-mode%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%81%AB%E8%A8%98%E8%BF%B0%E3%81%99%E3%82%8B)方法と、[チャプター別にOrg modeファイルを記述する](#%E3%83%81%E3%83%A3%E3%83%97%E3%82%BF%E3%83%BC%E5%88%A5%E3%81%ABorg-mode%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%82%92%E8%A8%98%E8%BF%B0%E3%81%99%E3%82%8B)方法があります。それぞれ説明します。

# 1冊分すべてを一つのOrg modeファイルに記述する

一つのOrg modeファイルに1冊分の内容を記述します。top-levelヘッドラインがチャプターの役割を担います。top-levelヘッドラインとは、  `* はじめに` のようなアスタリスクが一つの形式のヘッドラインのことです。

変換後は、チャプターごとに別々のMarkdownファイルが生成されます。本の場合に必要なconfig.yamlファイルも生成されます。

以下は一つのOrgファイルで本を書く場合の記述例です。Org modeのファイル名  `your-awesome-book` がslugになります。Zennのslugの定義は、[Zennのスラッグ（slug）とは](https://zenn.dev/zenn/articles/what-is-slug)を参照してください。

```
  your-awesome-book.org:

  #+TITLE: 本のタイトル
  #+OPTIONS: ^:t
  #+ZENN_SUMMARY: 本の紹介文
  #+ZENN_PUBLISHED: true
  #+ZENN_PRICE: 0
  #+ZENN_TOC_DEPTH: 1

  * はじめに
  :PROPERTEIS:
  :ZENN_FREE: t
  :END:

  はじめにの文章。

  * 章

  章の文章。

  * さいごに
  :PROPERTIES:
  :EXPORT_FILE_NAME: your-awesome-book_last
  :END:

  さいごにの文章。
```

この例では、config.yamlと3つのチャプターに対応したMarkdownファイルが生成されます。

- your-awesome-book/config.yaml
- your-awesome-book/your-awsome-book_1.md
- your-awesome-book/your-awsome-book_2.md
- your-awesome-book/your-awsome-book_last.md

config.yamlは以下のような内容で生成されます。

```
  your-awesome-book/config.yaml:

  title: "本のタイトル"
  summary: "本の紹介文"
  published: true
  price: 0
  toc_depth: 1
  chapters:
    - your-awesome-book_1
    - your-awesome-book_2
    - your-awesome-book_last
```

各チャプターに対応するMarkdownファイルは以下のように生成されます。ファイル名は、本のslug名のあとに `_チャプター番号` が付与された形になります。 `EXPORT_FILE_NAME` キーワードで個別に指定することも可能です。
チャプターのslug文字列を返す関数を `org-yazenn-book-with-slug-function` 変数にセットすることでもカスタマイズ可能です。カスタマイズした場合でも、 `config.yaml` の出力に自動で反映されます。（参照 [slugについて](https://zenn.dev/msnoigrs/books/zenn-no-toukou-kontentsu-wo-org-mode-de-kaku/viewer/slug-nitsuite)）

```
  your-awesome-book/your-awesome-book_1.md:

  ---
  title: "はじめに"
  free: true
  ---

  はじめにの文章。

  your-awesome-book/your-awesome-book_2.md:

  ---
  title: "章"
  ---

  章の文章。

  your-awesome-book/your-awesome-book_last.md:

  ---
  title: "さいごに"
  ---

  さいごにの文章。
```

1冊分すべてを一つのOrg modeファイルに記述する場合、有効なメタデータは以下の通りです。

- **TITLE:** title:キーになります。本のタイトル。必須。
- **ZENN_SUMMARY:** summary:キーになります。必須。
- **ZENN_PUBLISHED:** published:キーになります。 `true` か `false` 。デフォルトは `true` 。
- **ZENN_PRICE:** price:キーになります。 `0` か `200 - 5000` 。デフォルトは `0` 。
- **ZENN_TOC_DEPTH:** `0 - 3` 。何も指定しない場合、Zennのデフォルトは `2` 。

部分的にチャプターを無料にする場合や、チャプターのslugを指定する場合は、以下のキーワードが利用できます。top-levelヘッドラインの `:PROPERTIES:` ドロワーに記述します。

:::message
`:PROPERTIES:` ドロワーを編集するときは `M-x org-property-action` を実行します。
:::

- **ZENN_FREE:** free:キーになります。有料に設定した本で部分的にチャプターを無料公開する場合に `true` にします。デフォルトは出力なしです。
- **EXPORT_FILE_NAME:** チャプターのslugを意図的に指定したい場合に使用します。

参照

- [Org mode公式 Setting up keywords for individual files](https://orgmode.org/manual/Per_002dfile-keywords.html)
- [Org mode公式 Drawers](https://orgmode.org/manual/Drawers.html)
- [Org mode公式 Property Syntax](https://orgmode.org/manual/Property-Syntax.html)
- [Org mode公式 Export Settings TITLEやEXPORT_FILE_NAMEについて、OPTIONSについて](https://orgmode.org/manual/Export-Settings.html)
- [Zenn公式 本の各ファイルの役割](https://zenn.dev/zenn/articles/zenn-cli-guide#%E6%9C%AC%E3%81%AE%E5%90%84%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%81%AE%E5%BD%B9%E5%89%B2)

# チャプター別にOrg modeファイルを記述する

チャプター別にOrg modeファイルを記述します。チャプターのタイトルは、メタデータの `#+TITLE:` キーワードに記入します。

以下はチャプターのOrg modeファイルの例です。 

```
  #+TITLE: チャプタータイトル
  #+ZENN_FREE: true

  チャプターの本文。
```

こちらの方法で本を書く場合は、 `config.yaml` を生成するためのOrg modeファイルも書く必要があります。

以下は、 `config.yaml` 用のOrg modeファイルの例です。 `chapters:` キーを生成するために `ZENN_CHAPTERS:` キーワードがあります。チャプターslugを意図した順に並べて記述してください。

```
  #+TITLE: 本のタイトル
  #+OPTIONS: ^:t
  #+ZENN_SUMMARY: 本の紹介文
  #+ZENN_PUBLISHED: true
  #+ZENN_PRICE: 0
  #+ZENN_TOC_DEPTH: 1
  #+ZENN_CHAPTERS: chapter1 chapter2 chapter3
```

上記の例からは、以下のような `config.yaml` が生成されます。

```
  title: "本のタイトル"
  summary: "本の紹介文"
  published: true
  price: 0
  toc_depth: 1
  chapters:
    - chapter1
    - chapter2
    - chapter3
```

チャプター別にOrg modeファイルを記述する場合、有効なメタデータは以下の通りです。

- **TITLE:** title:キーになります。本のタイトル。必須。
- **ZENN_SUMMARY:** summary:キーになります。必須。
- **ZENN_PUBLISHED:** published:キーになります。 `true` か `false` 。デフォルトは `true` 。
- **ZENN_PRICE:** price:キーになります。 `0` か `200 - 5000` 。デフォルトは `0` 。
- **ZENN_TOC_DEPTH:** `0 - 3` 。何も指定しない場合、Zennのデフォルトは `2` 。
- **ZENN_CHAPTERS:** chapters:キーになります。必須。

参照

- [Org mode公式 Setting up keywords for individual files](https://orgmode.org/manual/Per_002dfile-keywords.html)
- [Org mode公式 Drawers](https://orgmode.org/manual/Drawers.html)
- [Org mode公式 Property Syntax](https://orgmode.org/manual/Property-Syntax.html)
- [Org mode公式 Export Settings TITLEやEXPORT_FILE_NAMEについて、OPTIONSについて](https://orgmode.org/manual/Export-Settings.html)
- [Zenn公式 本の各ファイルの役割](https://zenn.dev/zenn/articles/zenn-cli-guide#%E6%9C%AC%E3%81%AE%E5%90%84%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%81%AE%E5%BD%B9%E5%89%B2)
