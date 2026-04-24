---
title: "ox-yazennのナビゲーションメニュー"
---
Org modeファイルの編集作業中に `M-x org-export-dispatch` (C-c C-e) を呼び出すことでナビゲーションメニューが表示されます。
ナビゲーションメニューからZenn用のMarkdownファイルに変換する方法を説明します。

# 記事用のナビゲーションメニュー関数

記事用には、Markdownファイルを生成する関数と、一時的なバッファ上に書き出す関数の2つがあります。

- **org-yazenn-article-export-to-md:** Markdownファイルを生成。ファイルは、Orgファイルと同じディレクトリに生成されます。 `C-c C-e z a`
- **org-yazenn-article-export-as-md:** 一時的なemacsバッファ上に生成。 `C-c C-e z A`

# 本用のナビゲーションメニュー関数

本用には、一つのOrgファイルから `config.yaml` とチャプターに対応する複数のMarkdownファイルを生成する方法と、チャプター別にOrgファイルを記述して個別にMarkdownファイルを生成する方法があります。前者向けの関数は1つ、後者向けの関数は3つあります。

- **org-yazenn-book-export-to-zenn-book:** config.yamlとチャプターに対応するMarkdownが生成されます。 一つのOrg modeファイルに全チャプターを書く場合はこちらを利用します。 `C-c C-e z b`
- **org-yazenn-book-export-as-yaml:** config.yamlを生成します。 `C-c C-e z B`
- **org-yazenn-book-export-to-md:** チャプターのMarkdownを生成します。 `C-c C-e z c`
- **org-yazenn-book-export-as-md:** 一時的なemacsバッファ上にチャプターのMarkdownを生成します。 `C-c C-e z C`

![](/images/exporting.gif)
