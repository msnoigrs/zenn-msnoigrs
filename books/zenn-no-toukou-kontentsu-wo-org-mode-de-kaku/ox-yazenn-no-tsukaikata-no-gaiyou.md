---
title: "ox-yazennの使い方の概要"
---

# ox-yazennを利用する準備

記事用のMarkdownを生成するには、 `(require 'ox-yazenn-article)` を実行します。本用のMarkdownを生成するには、 `(require 'ox-yazenn-book)` を実行します。どちらも生成する場合は両方を実行します。

```lisp
;; Zenn記事用
(require 'ox-yazenn-article)
;; Zenn本様
(require 'ox-yazenn-book)
```

さらに `org-yazenn-zenndev-username` 変数にZenn.devのサイトのユーザー名を設定します。この変数はリンク文字列の生成に利用されるので、正しく設定してください。

```lisp
(setq org-yazenn-zenndev-username "Zenn.devのユーザー名")
```

# Org modeのExporting機能の使い方

`ox-yazenn` はOrg modeのExporting機能から利用します。Exporting機能を使う方法には、大きく分けて2種類あります。1つはナビゲーションメニューから操作する方法、もう1つはバッチ的に変換する方法です。

- **ナビゲーションメニューから操作する場合:** Org modeファイルの編集作業中に `M-x org-export-dispatch` (C-c C-e) を呼び出すことでナビゲーションメニューが表示されます。続いて変換先の形式および出力先（ファイルまたはemacsバッファ）を選択します。変換後の内容をすぐに確認したい場合に有用です。（参照 [Org mode公式 The Export Dispatcher](https://orgmode.org/manual/The-Export-Dispatcher.html)）

- **バッチ的に変換する場合:** 予め `org-publish` のプロジェクトを定義しておき `M-x org-publish` や `M-x org-publish-all` を実行して変換します。複数ファイルをまとめて変換する場合に向いています。Zenn用のMarkdownを生成するには、この方法が便利です。（参照 [Org mode公式 Configuration](https://orgmode.org/manual/Configuration.html)）

参照

- [ox-yazennのナビゲーションメニュー](https://zenn.dev/msnoigrs/books/zenn-no-toukou-kontentsu-wo-org-mode-de-kaku/viewer/ox-yazenn-no-nabigeeshonmenyuu)
- [org-publishを利用して変換する](https://zenn.dev/msnoigrs/books/zenn-no-toukou-kontentsu-wo-org-mode-de-kaku/viewer/org-publish-wo-riyou-shite-henkan-suru)
- [Org mode公式 Exporting](https://orgmode.org/manual/Exporting.html)
