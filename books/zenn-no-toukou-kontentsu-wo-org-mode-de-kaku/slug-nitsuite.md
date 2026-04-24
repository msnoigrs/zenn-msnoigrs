---
title: "slugについて"
---
slugはZenn.devで公開されるときのURLの一部になる文字列です。
記事の場合はファイル名がslugになります。本の場合は、コンテンツを置くディレクトリの名前と各チャプターのファイル名がslugになります。
よって執筆作業フローの中でslugを決める必要があるタイミングは、新規記事作成時、新規本作成時、本のチャプターを設ける時ということになります。
また、slugには、文字列の長さの制約、利用可能な文字種の制約、重複してはいけないという制約があります。完全にランダムな値を使うとファイル名を見ただけでは内容がわからなくなるという弊害があります。

私はslug文字列を考える作業に煩わしさを感じてしまいます。
文章を書く作業とはズレていて本質的ではないように思うからです。
（参照 [Zenn公式 Zennのスラッグ（slug）とは](https://zenn.dev/zenn/articles/what-is-slug)）

そこで、記事はタイトル文字列から、本はタイトルとチャプター文字列から自動でslugを生成するようにしています。kakasiを利用してタイトル文字列からローマ字文字列を生成するという方法を採用しています。[kanji-mode](https://github.com/wsgac/kanji-mode)のkm:all->romajiという関数が私の期待するものだったので、それを使用しています。

新規作成時は、org-roam-captureを利用します。タイトルの文字列を入力したら自動でファイル名が決まります。実現方法は前のチャプター（[書き始めるときの自動挿入](https://zenn.dev/msnoigrs/books/zenn-no-toukou-kontentsu-wo-org-mode-de-kaku/viewer/kakihajime-rutokino-jidou-sounyuu)）にある例を参照してください。

本のチャプターのファイル名は、同様にkakasiによるローマ字文字列を生成する関数を `org-yazenn-book-format-slug-function` に設定しています。
`org-yazenn-book-format-slug-function` 変数をカスタマイズすることで、チャプターのslug生成方法を制御できます。引数は以下の通りです。

- **n:** チャプター番号。1から始まります。
- **bookslug:** 本のslug文字列です。
- **title:** ヘッドラインタイトルの文字列です。
- **fname:** `EXPORT_FILE_NAME` の設定値です。

```lisp
(setq org-yazenn-book-format-slug-function
      (lambda (_n _bookslug title _fname)
        (let ((slug-trim-chars '(
                                 768 ; U+0300 COMBINING GRAVE ACCENT
                                 769 ; U+0301 COMBINING ACUTE ACCENT
                                 770 ; U+0302 COMBINING CIRCUMFLEX ACCENT
                                 771 ; U+0303 COMBINING TILDE
                                 772 ; U+0304 COMBINING MACRON
                                 774 ; U+0306 COMBINING BREVE
                                 775 ; U+0307 COMBINING DOT ABOVE
                                 776 ; U+0308 COMBINING DIAERESIS
                                 777 ; U+0309 COMBINING HOOK ABOVE
                                 778 ; U+030A COMBINING RING ABOVE
                                 779 ; U+030B COMBINING DOUBLE ACUTE ACCENT
                                 780 ; U+030C COMBINING CARON
                                 795 ; U+031B COMBINING HORN
                                 803 ; U+0323 COMBINING DOT BELOW
                                 804 ; U+0324 COMBINING DIAERESIS BELOW
                                 805 ; U+0325 COMBINING RING BELOW
                                 807 ; U+0327 COMBINING CEDILLA
                                 813 ; U+032D COMBINING CIRCUMFLEX ACCENT BELOW
                                 814 ; U+032E COMBINING BREVE BELOW
                                 816 ; U+0330 COMBINING TILDE BELOW
                                 817 ; U+0331 COMBINING MACRON BELOW
                                 )))
        (cl-flet* ((nonspacing-mark-p (char) (memq char slug-trim-chars))
                   (strip-nonspacing-marks (s)
                     (string-glyph-compose
                      (apply #'string
                             (seq-remove #'nonspacing-mark-p
                                         (string-glyph-decompose s)))))
                   (cl-replace (title pair)
                     (replace-regexp-in-string (car pair) (cdr pair) title)))
          (let* ((pairs `(("[^[:alnum:][:digit:]]" . "_")
                          ("__*" . "-")
                          ("^_" . "")
                          ("_$" . "")))
                 (slug (-reduce-from #'cl-replace (km:all->romaji (strip-nonspacing-marks title)) pairs))
                 (slug (truncate-string-to-width (concat slug (make-string (max 0 (- 12 (length slug))) ?_)) 50)))
               (downcase slug))))))
```

デフォルトの実装は以下のようになっています。 `EXPORT_FILE_NAME` が設定されていれば、それを優先し、設定されていなければ `bookslug_チャプター番号` になります。

```lisp
(defcustom org-yazenn-book-format-slug-function
    (lambda (n bookslug _title fname)
      (cond (fname fname)
            (t (concat bookslug "_" n))))
```
