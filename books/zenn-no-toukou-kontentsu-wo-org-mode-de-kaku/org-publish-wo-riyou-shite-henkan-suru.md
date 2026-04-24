---
title: "org-publishを利用して変換する"
---
`org-publish` を利用してOrg modeのファイルをZennの記事や本のMarkdownに変換する方法を説明します。

`org-publish` を利用すると、特定のディレクトリの下にあるOrg modeファイルをまとめて変換できます。変換結果はキャッシュされるため、更新したファイルだけが変換対象になります。変換を実行する単位はプロジェクトと呼ばれます。 

org-publishの機能を実行するには以下の関数のどれかを呼び出します。

- **org-publish-all:** 全プロジェクトを実行します。
- **org-publish/org-publish-project:** プロジェクトを指定して実行します。
- **org-publish-current-project:** 開いているOrg modeファイルが属するプロジェクトを実行します。
- **org-publish-current-file:** 開いているOrg modeファイルを対象にプロジェクトの定義に沿って実行します。

ここで `org-publish-project-alist` 変数の設定例を示します。
この例では、3つのプロジェクトを定義しています。

- **zenn:** articlesプロジェクトとbooksプロジェクト両方を実行する。
- **articles:** 記事だけを変換する。orgaディレクトリの下のOrg modeファイルを記事として変換し、articlesディレクトリの下に保存する。
- **books:** 本を変換する。orgbディレクトリの下のOrg modeファイルを本として変換し、booksディレクトリの下に保存する。

出力先をZenn CLIのプレビュー機能が参照するディレクトリにしておくと、org-publishを実行しただけでレンダリング結果が確認できて便利です。

```lisp
(require 'ox-publish)
(require 'ox-yazenn-article)
(require 'ox-yazenn-book)

(setq my-working-directory "Org原稿があるディレクトリ")
(setq my-target-directory "Zenn CLIが参照するディレクトリ")
(setq org-yazenn-zenndev-userid "Zenn.devのユーザーID")

(cd my-working-directory)

(setq org-publish-project-alist
      `(;;("zenn" :components ("articles"))
        ("zenn" :components ("articles" "books"))
        ("articles"
         :base-directory "./orga"
         :recursive nil
         :publishing-function org-yazenn-article-publish-to-md
         ;; :publishing-directory "./articles"
         :publishing-directory ,(file-name-concat my-target-directory "articles")
         :with-author nil
         :with-creater nil
         :with-toc nil
         :section-numbers nil
         :yazenn-with-published t)
        ("books"
         :base-directory "./orgb"
         :recursive nil
         :publishing-function org-yazenn-book-publish-to-zenn-book
         ;; :publishing-directory "./books"
         :publishing-directory ,(file-name-concat my-target-directory "books")
         :with-author nil
         :with-creator nil
         :with-toc nil
         :section-numbers nil
         :yazenn-with-published t)))
```

さらに、この例のような内容をファイルとして保存しておき、コマンドラインからemacsを実行して変換をすることもできます。例えば、 `zennproject.el` というファイルで保存したと仮定すると、以下のように実行できます。

```shell
# zennプロジェクトを実行
emacs --batch --no-init-file --load zennproject.el --eval '(org-publish "zenn")'
# キャッシュを無視して作成
emacs --batch --no-init-file --load zennproject.el --eval '(org-publish "zenn" t)'
# プロジェクトを選択する場合
emacs --batch --no-init-file --load zennproject.el --funcall org-publish
```

参照

- [Org mode公式 Publishing](https://orgmode.org/manual/Publishing.html)
