---
title: "書き始めるときの自動挿入"
---
記事や本を新規作成するときに、あらかじめ必要なメタデータ（
`#+TITLE:` や `#+ZENN_SUMMARY:` など）が自動で挿入された状態のOrg modeファイルが準備されると便利です。 

Zenn CLIには `new:article` コマンドがあります。実行するとarticlesディレクトリの下にMarkdownが新規に作成され、記事に必要となるfront matterが挿入された状態になっています。（参照 [Zenn公式 記事の作成](https://zenn.dev/zenn/articles/zenn-cli-guide#%E8%A8%98%E4%BA%8B%E3%81%AE%E4%BD%9C%E6%88%90)）

emacsではさまざまなauto insert機能やスニペット機能が利用できますので、それらを使うことでOrg modeファイルに対しても同様の機能を実現できます。
例えば、Org modeの機能の一つである `org-capture` にはテンプレートを挿入する方法があります。 `org-capture` の設定方法はここでは説明しませんが、 `M-x help-for-help v org-capture-templates` で表示される説明が詳しいです。
以下は、参考になりそうなページへのリンクです。

- [Writing an org-capture template and defun for blog posts](https://forum.systemcrafters.net/t/writing-an-org-capture-template-and-defun-for-blog-posts/1624)
- [org-captureのテンプレート ver. 2021](https://ladicle.com/post/20210120_105729/)

私は、[org-roam](https://www.orgroam.com/)を利用しているため、org-roamの機能の一つである `org-roam-capture` を利用して自動挿入を実現しています。設定方法は、やはり `M-x help-for-help v org-roam-capture-templates` で表示される説明が詳しいです。 `org-capture-templates` と非常に似ています。

以下に例として設定の抜粋を記します。この例では、タイトルの文字列からslugを自動生成する機能を実装しています。

```lisp
(setup org-roam
  (:option
   org-roam-capture-templates '(("d" "default" plain "%?"
                                 :target (file+head
                                          "%<%Y%m%d%H%M%S>-${customslug}.org"
                                          "#+title: ${title}\n")
                                 :unnarrowed t)
                                ("a" "an article for Zenn.dev" plain "* %?"
                                 :target (file+head
                                          "zenn/articles/${zennslug}.org"
                                          "#+title: ${title}\n#+ZENN_EMOJI: 🔖\n#+ZENN_TYPE: tech\n#+ZENN_PUBLISHED: nil\n#+KEYWORDS: \n")
                                 :unnarrowed t)
                                ("b" "a book for Zenn.dev" plain "* %?"
                                 :target (file+head
                                          "zenn/books/${zennslug}.org"
                                          "#+title: ${title}\n#+ZENN_SUMMARY: サマリ\n#+ZENN_PRICE: 0\n#+ZENN_PUBLISHED: nil\n#+KEYWORDS: \n")
                                 :unnarrowed t)))

  (:when-loaded
    (require 'kanji-mode)
    (require 'cl-lib)
    (require 'dash)
    (cl-defmethod org-roam-node-customslug ((node org-roam-node))
      "Return the slug of NODE."
      (let* ((title (km:all->romaji (org-roam-node-title node)))
             (title (if (string= title "") (my-random-string)
                      title))
             (slug-trim-chars '(;; Combining Diacritical Marks https://www.unicode.org/charts/PDF/U0300.pdf
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
          (let* ((pairs `(("[^[:alnum:][:digit:]]" . "_") ;; convert anything not alphanumeric
                          ("__*" . "-")                   ;; remove sequential underscores
                          ("^_" . "")                     ;; remove starting underscore
                          ("_$" . "")))                   ;; remove ending underscore
                 (slug (-reduce-from #'cl-replace (strip-nonspacing-marks title) pairs)))
            (downcase slug)))))

    (cl-defmethod org-roam-node-zennslug ((node org-roam-node))
      "Return the slug for Zenn.dev of NODE."
      (let ((cslug (org-roam-node-customslug node)))
        (truncate-string-to-width (concat cslug (make-string (max 0 (- 12 (length cslug))) ?_)) 50)))))
```

org-roamとの連携方法の詳細については、別の機会で文書にできればと思っています。
以下は、 `org-roam-capture-templates` に関して参考になりそうなページへのリンクです。

- [Org-roam Basics: How org-roam-capture-templates Work](https://org-roam.discourse.group/t/org-roam-basics-how-org-roam-capture-templates-work/3670)
