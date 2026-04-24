---
title: "Org mode記法とMarkdown表現"
---
ox-yazennは[ZennのMarkdown記法一覧](https://zenn.dev/zenn/articles/markdown-guide)に対応したMarkdownを生成します。Zenn固有の記法にも対応しています。

ここではOrg modeでの記述がどのようにMarkdownで表現されるかを説明します。Org mode記法を一通り網羅しています。リファレンスとして利用できるように、可能な限り関連する公式ドキュメントへのリンクを設けました。また、Zenn固有のMarkdown表現を利用したい場合に、どのようにOrg modeで記述するとよいかについても書いています。Zenn固有の表現を意識してOrg modeを書いた場合でも、他のバックエンドと互換性を維持するように設計してあります。

- 一般的なOrg mode記法

  - [パラグラフ中の改行](#%E3%83%91%E3%83%A9%E3%82%B0%E3%83%A9%E3%83%95%E4%B8%AD%E3%81%AE%E6%94%B9%E8%A1%8C)
  - [見出し](#%E8%A6%8B%E5%87%BA%E3%81%97)
  - [プレーンリスト](#%E3%83%97%E3%83%AC%E3%83%BC%E3%83%B3%E3%83%AA%E3%82%B9%E3%83%88)
  - [チェックボックス](#%E3%83%81%E3%82%A7%E3%83%83%E3%82%AF%E3%83%9C%E3%83%83%E3%82%AF%E3%82%B9)
  - [Markdownにおいて特別な意味を持つ文字や文字列](#markdown%E3%81%AB%E3%81%8A%E3%81%84%E3%81%A6%E7%89%B9%E5%88%A5%E3%81%AA%E6%84%8F%E5%91%B3%E3%82%92%E6%8C%81%E3%81%A4%E6%96%87%E5%AD%97%E3%82%84%E6%96%87%E5%AD%97%E5%88%97)
  - [Org modeの強制改行](#org-mode%E3%81%AE%E5%BC%B7%E5%88%B6%E6%94%B9%E8%A1%8C)
  - [詩句ブロック(verseブロック)](#%E8%A9%A9%E5%8F%A5%E3%83%96%E3%83%AD%E3%83%83%E3%82%AF%28verse%E3%83%96%E3%83%AD%E3%83%83%E3%82%AF%29)
  - [引用ブロック(quoteブロック)](#%E5%BC%95%E7%94%A8%E3%83%96%E3%83%AD%E3%83%83%E3%82%AF%28quote%E3%83%96%E3%83%AD%E3%83%83%E3%82%AF%29)
  - [exampleブロック](#example%E3%83%96%E3%83%AD%E3%83%83%E3%82%AF)
  - [srcブロック](#src%E3%83%96%E3%83%AD%E3%83%83%E3%82%AF)
  - [固定長リージョン](#%E5%9B%BA%E5%AE%9A%E9%95%B7%E3%83%AA%E3%83%BC%E3%82%B8%E3%83%A7%E3%83%B3)
  - [ダイナミックブロック](#%E3%83%80%E3%82%A4%E3%83%8A%E3%83%9F%E3%83%83%E3%82%AF%E3%83%96%E3%83%AD%E3%83%83%E3%82%AF)
  - [中央寄せブロック(centerブロック)](#%E4%B8%AD%E5%A4%AE%E5%AF%84%E3%81%9B%E3%83%96%E3%83%AD%E3%83%83%E3%82%AF%28center%E3%83%96%E3%83%AD%E3%83%83%E3%82%AF%29)
  - [コメントブロック(commentブロック)](#%E3%82%B3%E3%83%A1%E3%83%B3%E3%83%88%E3%83%96%E3%83%AD%E3%83%83%E3%82%AF%28comment%E3%83%96%E3%83%AD%E3%83%83%E3%82%AF%29)
  - [強調、モノスペース](#%E5%BC%B7%E8%AA%BF%E3%80%81%E3%83%A2%E3%83%8E%E3%82%B9%E3%83%9A%E3%83%BC%E3%82%B9)
  - [上付き文字、下付き文字(subscript,superscript)](#%E4%B8%8A%E4%BB%98%E3%81%8D%E6%96%87%E5%AD%97%E3%80%81%E4%B8%8B%E4%BB%98%E3%81%8D%E6%96%87%E5%AD%97%28subscript%2Csuperscript%29)
  - [LaTeXフラグメント](#latex%E3%83%95%E3%83%A9%E3%82%B0%E3%83%A1%E3%83%B3%E3%83%88)
  - [水平線](#%E6%B0%B4%E5%B9%B3%E7%B7%9A)
  - [表](#%E8%A1%A8)
  - [脚注](#%E8%84%9A%E6%B3%A8)
  - [ハイパーリンク](#%E3%83%8F%E3%82%A4%E3%83%91%E3%83%BC%E3%83%AA%E3%83%B3%E3%82%AF)
  - [外部リンク](#%E5%A4%96%E9%83%A8%E3%83%AA%E3%83%B3%E3%82%AF)
  - [内部リンク](#%E5%86%85%E9%83%A8%E3%83%AA%E3%83%B3%E3%82%AF)
  - [画像](#%E7%94%BB%E5%83%8F)
  - [Markdownフラグメント](#markdown%E3%83%95%E3%83%A9%E3%82%B0%E3%83%A1%E3%83%B3%E3%83%88)
  - [目次](#%E7%9B%AE%E6%AC%A1)
- Zenn固有機能対応

  - [画像のAltテキスト、キャプション、横幅指定](#%E7%94%BB%E5%83%8F%E3%81%AEalt%E3%83%86%E3%82%AD%E3%82%B9%E3%83%88%E3%80%81%E3%82%AD%E3%83%A3%E3%83%97%E3%82%B7%E3%83%A7%E3%83%B3%E3%80%81%E6%A8%AA%E5%B9%85%E6%8C%87%E5%AE%9A)
  - [コードブロックのファイル名およびdiff属性付加](#%E3%82%B3%E3%83%BC%E3%83%89%E3%83%96%E3%83%AD%E3%83%83%E3%82%AF%E3%81%AE%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E5%90%8D%E3%81%8A%E3%82%88%E3%81%B3diff%E5%B1%9E%E6%80%A7%E4%BB%98%E5%8A%A0)
  - [スペシャルブロック(message,alert,quote)](#%E3%82%B9%E3%83%9A%E3%82%B7%E3%83%A3%E3%83%AB%E3%83%96%E3%83%AD%E3%83%83%E3%82%AF%28message%2Calert%2Cquote%29)
  - [コンテンツの埋め込み](#%E3%82%B3%E3%83%B3%E3%83%86%E3%83%B3%E3%83%84%E3%81%AE%E5%9F%8B%E3%82%81%E8%BE%BC%E3%81%BF)
  - [コンテンツの埋め込みに関する設定](#%E3%82%B3%E3%83%B3%E3%83%86%E3%83%B3%E3%83%84%E3%81%AE%E5%9F%8B%E3%82%81%E8%BE%BC%E3%81%BF%E3%81%AB%E9%96%A2%E3%81%99%E3%82%8B%E8%A8%AD%E5%AE%9A)
  - [ダイアグラム](#%E3%83%80%E3%82%A4%E3%82%A2%E3%82%B0%E3%83%A9%E3%83%A0)

:::message
ox-yazennのMarkdownバックエンドはスクラッチから書いています。よってorg-modeの公式に含まれるMarkdown用のバックエンドox-mdとは挙動が異なります。ox-mdはox-htmlから派生して実装されており、Markdownで表現できない場合はHTML表現を使う傾向があります。ZennのようなMarkdownをレンダリングするサイトでは、HTML表現を受け付けずエスケープ処理して無効化する運用になっています。ox-yazennではOrg modeファイルから生成するMarkdownの中に、意図しないHTML表現が含まれないように実装しました。
:::

# パラグラフ中の改行

`org-yazenn-preserve-filling` 変数が `t` の場合、改行はOrgファイルで記述されたままMarkdownに出力されます。デフォルトは `t` です。 `nil` や `"false"` や `""` の場合は、改行前後に `:multibyte:` 文字がある場合、改行が取り除かれ前後の行が結合されます。 改行前後が `:unibyte:` 文字の場合は、結合されずそのまま出力されます。
各Orgファイルで `#+PRESERVE_FILLING:` キーワードを記述することでも制御可能です。

この機能は、単語区切りにスペースを用いない日本語の文章でMarkdown上の表現とWebレンダリング後の表現を調整するためのものです。

Org(org-yazenn-preserve-fillingがtの場合):

```
  #+PRESERVE_FILLING: t

  文章の途中に
  改行がある。

  文章の途中に改行があり、
  alphabetで始まる。

  文章の途中に改行があり、
      次の行頭にスペース4つある。

  文章の途中に改行があり、
  　次の行頭に全角スペースがある。

  文章の途中に改行があり、
  	次の行頭にタブがある。

  文章の途中に改行があり、
    
  スペースだけの行が挟まっている。別のパラグラフとして扱われる。

  A sentence with
  a line break.
```

Markdown(org-yazenn-preserve-fillingがtの場合):

```

  文章の途中に
  改行がある。

  文章の途中に改行があり、
  alphabetで始まる。

  文章の途中に改行があり、
  次の行頭にスペース4つある。

  文章の途中に改行があり、
  次の行頭に全角スペースがある。

  文章の途中に改行があり、
  次の行頭にタブがある。

  文章の途中に改行があり、

  スペースだけの行が挟まっている。別のパラグラフとして扱われる。

  A sentence with
  a line break.
```

Org(org-yazenn-preserve-fillingがnilの場合):

```
  #+PRESERVE_FILLING: nil

  文章の途中に
  改行がある。

  文章の途中に改行があり、
  alphabetで始まる。

  文章の途中に改行があり、
      次の行頭にスペース4つある。

  文章の途中に改行があり、
  　次の行頭に全角スペースがある。

  文章の途中に改行があり、
  	次の行頭にタブがある。

  文章の途中に改行があり、
    
  スペースだけの行が挟まっている。別のパラグラフとして扱われる。

  A sentence with
  a line break.
```

Markdown(org-yazenn-preserve-fillingがnilの場合):

```
  文章の途中に改行がある。

  文章の途中に改行があり、alphabetで始まる。

  文章の途中に改行があり、次の行頭にスペース4つある。

  文章の途中に改行があり、次の行頭に全角スペースがある。

  文章の途中に改行があり、次の行頭にタブがある。

  文章の途中に改行があり、

  スペースだけの行が挟まっている。別のパラグラフとして扱われる。

  A sentence with
  a line break.
```

# 見出し

Org modeのヘッドラインが、Markdownの見出しとして出力されます。

参照

- [Org mode公式 Headlines](https://orgmode.org/manual/Headlines.html)
- [Zenn公式 見出し](https://zenn.dev/zenn/articles/markdown-guide#%E8%A6%8B%E5%87%BA%E3%81%97)

Org:

```
  * 見出し1

  コンテンツ1

  ** 見出し1-1

  コンテンツ1-1

  *** 見出し1-1-3

  コンテンツ1-1-3
```

Markdown:

```
  # 見出し1

  コンテンツ1

  ## 見出し1-1

  コンテンツ1-1

  ### 見出し1-1-3

  コンテンツ1-1-3
```

# プレーンリスト

Org modeでは、番号なしリスト、番号付きリスト、説明リストの3種類が使えます。

参照

- [Org mode公式 Plain Lists](https://orgmode.org/manual/Plain-Lists.html)
- [Zenn公式 リスト](https://zenn.dev/zenn/articles/markdown-guide#%E3%83%AA%E3%82%B9%E3%83%88)
- [Zenn公式 番号付きリスト](https://zenn.dev/zenn/articles/markdown-guide#%E7%95%AA%E5%8F%B7%E4%BB%98%E3%81%8D%E3%83%AA%E3%82%B9%E3%83%88)

Org:

```
  1. リスト項目1
  1. リスト項目2
     + リスト+プラスで始まる1
     + リスト+プラスで始まる2
  3. リスト項目3
     - リスト-ハイフンで始まる1
     リスト項目の下、改行してある行
  ここからコンテンツ。
  以下は説明リスト:
  - 説明1 :: リスト内容1単一行
  - 説明2 :: リスト内容2複数行
    複数行つづき。

  1. リスト一つだけ

  + プラス一つだけ

  - マイナス一つだけ

  + 混在1
  - 混在2
```

Markdown:

```
  1. リスト項目1
  2. リスト項目2

     - リスト+プラスで始まる1
     - リスト+プラスで始まる2
  3. リスト項目3

     - リスト-ハイフンで始まる1

     リスト項目の下、改行してある行

  ここからコンテンツ。
  以下は説明リスト:

  - **説明1:** リスト内容1単一行
  - **説明2:** リスト内容2複数行
    複数行つづき。

  - リスト一つだけ

  - プラス一つだけ

  - マイナス一つだけ

  - 混在1
  - 混在2
```

# チェックボックス

Orgのチェックボックス状態には3態あります。

参照

- [Org mode公式 Checkboxes](https://orgmode.org/manual/Checkboxes.html)

Org:

```
  - [ ] 説明1 :: てすと1
  - [X] 説明2 :: てすと2

  - [ ] てすと1
  - [X] てすと2
  - [-] てすと3
    - [X] てすと4
    - [ ] てすと5
```

Markdown:

```
  - [ ] **説明1:** てすと1
  - [X] **説明2:** てすと2

  - [ ] てすと1
  - [X] てすと2
  - 🔳 てすと3

    - [X] てすと4
    - [ ] てすと5
```

# Markdownにおいて特別な意味を持つ文字や文字列

バックスラッシュは、Markdownにおいてエスケープ文字として扱わるため、2つ重ねて出力されます。
HTMLコメント形式の記述は、Markdownにおいてコメント扱いされるため、バックスラッシュでエスケープして出力されます。

Org:

```
  特殊文字 \ バックスペース
  <!-- HTMLコメント -->
  ​# (シャープ+スペース)で始まる行はOrgではコメント扱いなのでMarkdownには出力されない。
  #+MD: \# シャープで始まる行をMarkdownに出力したい場合は、後述するMarkdownフラグメント記法を利用する
```

`#+MD` の代わりに `#+MARKDOWN` `#+ZENN` でも可です。

Markdown:

```
  特殊文字 \\ バックスペース
  \<!-- HTMLコメント --\>
  \# シャープ+スペースで始まる行をMarkdownに出力したい場合は、後述するMarkdownフラグメント記法を利用する
```

# Org modeの強制改行

Org modeでは行末にバックスラッシュを2つ書くことで強制的に改行することを表現します。 Markdownではスペースを2つ出力します。

参照

- [Org mode公式 Paragraphs](https://orgmode.org/manual/Paragraphs.html)

Org:

```
  Org modeの強制改行バックスラッシュ2つ \\
  がある場合。
```

Markdown:

```
  Org modeの強制改行バックスラッシュ2つ   (←スペース2つ)
  がある場合。
```

# 詩句ブロック(verseブロック)

詩句ブロックは引用ブロックになります。

参照

- [Org mode公式 Paragraphs](https://orgmode.org/manual/Paragraphs.html)

Org:

```
  #+begin_verse
      怪物と戦う者は、戦ううちに自分も怪物とならないように用心した方がいい
      深淵をのぞく時、深淵もまたこちらをのぞいているのだ

                                                               -- ニーチェ
  #+end_verse
```

Markdown:

```
  > 怪物と戦う者は、戦ううちに自分も怪物とならないように用心した方がいい
  > 深淵をのぞく時、深淵もまたこちらをのぞいているのだ
  > 							 -- ニーチェ
```

# 引用ブロック(quoteブロック)

引用ブロックはそのまま引用ブロックになります。

参照

- [Org mode公式 Paragraphs](https://orgmode.org/manual/Paragraphs.html)
- [Zenn公式 引用](https://zenn.dev/zenn/articles/markdown-guide#%E5%BC%95%E7%94%A8)

Org:

```
  #+begin_quote
  クォートブロック

  目がさめるとベッドの上で大きな毒虫になっていた -- フランツ・カフカ
  #+end_quote
```

Markdown:

```
  > クォートブロック
  > 
  > 目がさめるとベッドの上で大きな毒虫になっていた -- フランツ・カフカ
```

# exampleブロック

exampleブロックは言語ラベルなしのコードブロックになります。

参照

- [Org mode公式 Literal Examples](https://orgmode.org/manual/Literal-Examples.html)

Org:

```
  #+begin_example
  exampleブロック1
    exampleブロック2
  #+end_example
```

Markdown:

```
  ​```
  exampleブロック1
    exampleブロック2
  ​```
```

# srcブロック

srcブロックはコードブロックになります。言語ラベルを指定可能です。Org modeのsrcブロックでは、 `+n` や `-n` で行数の指定が可能ですが、Markdownにはその情報は出力されません。

参照

- [Org mode公式 Working with Source Code](https://orgmode.org/manual/Working-with-Source-Code.html)
- [Org mode公式 Literal Examples](https://orgmode.org/manual/Literal-Examples.html)
- [Zenn公式 コードブロック](https://zenn.dev/zenn/articles/markdown-guide#%E3%82%B3%E3%83%BC%E3%83%89%E{x3%83%96%E3%83%AD%E3%83%83%E3%82%AF)

Org:

```
  #+begin_src js
    const great = () => {
      console.log("Awesome");
    };
  #+end_src
```

Markdown:

```
  ​```js
  const great = () => {
    console.log("Awesome");
  };
  ​```
```

Zennオリジナル機能のコードブロックにファイル名を表示する場合や、diffのシンタックスハイライト機能を使う場合は[コードブロックのファイル名およびdiff属性付加](#%E3%82%B3%E3%83%BC%E3%83%89%E3%83%96%E3%83%AD%E3%83%83%E3%82%AF%E3%81%AE%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E5%90%8D%E3%81%8A%E3%82%88%E3%81%B3diff%E5%B1%9E%E6%80%A7%E4%BB%98%E5%8A%A0)を参照してください。

# 固定長リージョン

Org modeの固定長リージョンは、exampleブロックと同様にコードブロックになります。
固定長リージョンは、 `:+スペース` で始まる行です。

参照

- [Org mode公式 Literal Examples](https://orgmode.org/manual/Literal-Examples.html)

Org:

```
  : ここは
  : 固定長のエリア
```

Markdown:

```
  ​```
  ここは
  固定長のエリア
  ​```
```

# ダイナミックブロック

ダイナミックブロックとはOrgファイル上でelispプログラムを実行し、その実行結果を `#+BEGIN:` と `#+END:` の間に挿入する機能です。
`#+BEGIN:` と `#+END:` の間の文字列だけがMarkdownに出力されます。

参照

- [ダイナミックブロック](https://orgmode.org/manual/Dynamic-Blocks.html)

Org:

```
  #+BEGIN: dynamic
  ダイナミックブロックの機能を使って出力された内容。
  #+END:
```

Markdown:

```
  ダイナミックブロックの機能を使って出力された内容。
```

# 中央寄せブロック(centerブロック)

centerブロックは無効です。

参照

- [Org mode公式 Paragraphs](https://orgmode.org/manual/Paragraphs.html)

Org:

```
  #+begin_center
  中央寄せ
  #+end_center
```

Markdown:

```
  中央寄せ
```

# コメントブロック(commentブロック)

コメントブロックは、除去されます。

参照

- [Org mode公式 Comment Lines](https://orgmode.org/manual/Comment-Lines.html)

Org:

```
  #+begin_comment
  コメント行1
  コメント行2
  #+end_comment
```

`# + スペース` で始まる行もコメント扱いになり、除去されます。

# 強調、モノスペース

Markdownにはアンダーラインに相当する機能はありません。

参照

- [Org mode公式 Emphasis and Monospace](https://orgmode.org/manual/Emphasis-and-Monospace.html)

Org:

```
  *ボールド* /イタリック/ _アンダーライン_ =バーベイタム= ~コード~ +打ち消し+
```

Markdown:

```
  **ボールド** _イタリック_ アンダーライン `バーベイタム` `コード` ~~打ち消し~~
```

組み合わせたときの、エッジケース。

Org:

```
  /*boldの処理をしたあとにitalicの処理がくる*/
  */italicの処理をしたあとにboldの処理がくる/*

  **bold**
  //italic//
  /*italic2*/
  /**italic3**/
  __underlined__
  ==verbatim==
  ~~code~~
  ++strike-through++
  +++++
  ++++
  ++*/mix/*++
```

Markdown:

```
  _**boldの処理をしたあとにitalicの処理がくる**_
  **_italicの処理をしたあとにboldの処理がくる_**

  **\*bold\***
  _\_italic\__
  _**italic2**_
  _**\*italic3\***_
  underlined
  `=verbatim=`
  `~code~`
  ~~+strike-through+~~
  ~~+++~~
  ~~++~~
  ~~+**_mix_**+~~
```

# 上付き文字、下付き文字(subscript,superscript)

org-export-with-sub-superscripts 変数の設定値によって、出力が変わります。
設定値は、nil か t か "{}" のいずれかです。
OrgファイルのOPTIONSキーワード行で、^:t などのように記述すると、ファイル毎に設定することが可能です。

参照

- [Org mode公式 Subscripts and Superscripts](https://orgmode.org/manual/Subscripts-and-Superscripts.html)
- [Org mode公式 Export Settings](https://orgmode.org/manual/Export-Settings.html)
- [Org mode公式 LaTex fragments](https://orgmode.org/manual/LaTeX-fragments.html)
- [Zenn公式 インラインで数式を挿入する](https://zenn.dev/zenn/articles/markdown-guide#%E3%82%A4%E3%83%B3%E3%83%A9%E3%82%A4%E3%83%B3%E3%81%A7%E6%95%B0%E5%BC%8F%E3%82%92%E6%8C%BF%E5%85%A5%E3%81%99%E3%82%8B)

Org (#+OPTIONS: ^:t):

```
  H_{2}O
  H_2 O
  E = mc^{2}
  E = mc^2
```

Markdown:

アンダースコア(_)やハット(^)は数式扱いで出力されます。

```
  H$_{2}$O
  H$_{2}$ O
  E = mc$^{2}$
  E = mc$^{2}$
```

Org (#+OPTIONS: ^:nil):

```
  H_{2}O
  H_2 O
  E = mc^{2}
  E = mc^2
```

Markdown:

アンダースコア(_)もハット(^)も数式扱いされずにそのまま出力されます。

```
  H_{2}O
  H_2 O
  E = mc^{2}
  E = mc^2
```

Org (#+OPTIONS: ^:{}):

`{}` で囲った場合のみ数式扱いになります。

```
  H_{2}O
  H_2 O
  E = mc^{2}
  E = mc^2
```

Markdown:

```
  H$_{2}$O
  H_2 O
  E = mc$^{2}$
  E = mc^2
```

# LaTeXフラグメント

`$` 1つで囲む場合、インラインでの数式扱いとなり、2つで囲むと数式ブロックとなります。Zennでは数式はKaTeX記法が使えます。

参照

- [Org mode公式 LaTex fragments](https://orgmode.org/manual/LaTeX-fragments.html)
- [Zenn公式 数式](https://zenn.dev/zenn/articles/markdown-guide#%E6%95%B0%E5%BC%8F)
- [Zenn公式 インラインで数式を挿入する](https://zenn.dev/zenn/articles/markdown-guide#%E3%82%A4%E3%83%B3%E3%83%A9%E3%82%A4%E3%83%B3%E3%81%A7%E6%95%B0%E5%BC%8F%E3%82%92%E6%8C%BF%E5%85%A5%E3%81%99%E3%82%8B)

Org:

```
  If $a^2=b$ and \( b=2 \), then the solution must be
  either $$ a=+\sqrt{2} $$ or \[ a=-\sqrt{2} \].
```

Markdown:

```
  If $a^2=b$ and $b=2$, then the solution must be
  either 

  $$

  a=+\sqrt{2}

  $$

  or 

  $$

  a=-\sqrt{2}

  $$

  .
```

# 水平線

Orgではダッシュを最低5つ書きます。

参照

- [Org mode公式 Horizontal Rules](https://orgmode.org/manual/Horizontal-Rules.html)
- [Zenn公式 区切り線](https://zenn.dev/zenn/articles/markdown-guide#%E5%8C%BA%E5%88%87%E3%82%8A%E7%B7%9A)

Org:

```
  -----
```

Markdown:

```
  ---
```

# 表

Orgでは、表の表現方法が3種類あります。どの表現方法にも対応しています。列のアライメント情報もMarkdown出力に反映されます。表のキャプションやリンクターゲットの情報は出力されません。

参照

- [Org mode公式 Tables](https://orgmode.org/manual/Tables.html)
- [Zenn公式 テーブル](https://zenn.dev/zenn/articles/markdown-guide#%E3%83%86%E3%83%BC%E3%83%96%E3%83%AB)

Org:

```
  | 列名1 | 列名2 | 列名3 |
  | 1     | 2     | 3     |
  | AAAA  | BBBB  | CCCC  |
  | END1  | END2  | END3  |

  | 列名1 | 列名2 | 列名3 |
  |     1 |     2 |     3 |
  |     4 |     5 |     6 |
  |     7 |     8 |     9 |

  | 列名1 | 列名2 | 列名3 |
  |-------+-------+-------|
  | 1     | 2     | 3     |
  | AAAA  | BBBB  | CCCC  |
  | END1  | END2  | END3  |

  |-------+-------+-------|
  | 列名1 | 列名2 | 列名3 |
  |-------+-------+-------|
  | 1     | 2     | 3     |
  |-------+-------+-------|
  | AAAA  | BBBB  | CCCC  |
  |-------+-------+-------|
  | END1  | END2  | END3  |
  |-------+-------+-------|

  #+CAPTION: 表キャプション
  #+NAME: tablelabel
  | 列名1 | 列名2 | 列名3 |
  |-------+-------+-------|
  | 1     | 2     | 3     |
  | AAAA  | BBBB  | CCCC  |
  | END1  | END2  | END3  |

  | 列名1 | 列名2 | 列名3 |
  |-------+-------+-------|
  | <r>   |<c>    | <l>   |
  | 1     | 2     | 3     |
  | AAAA  | BBBB  | CCCC  |
  | END1  | END2  | END3  |

  | 列名1 | 列名2 | 列名3 |
  |-------+-------+-------|
  | <l>  |<c>    | <r>  |
  | 1     | 2     | 3     |
  | AAAA  | BBBB  | CCCC  |
  | END1  | END2  | END3  |
```

Markdown:

```
  | 列名1 | 列名2 | 列名3 |
  | :---- | :---- | :---- |
  | 1     | 2     | 3     |
  | AAAA  | BBBB  | CCCC  |
  | END1  | END2  | END3  |

  | 列名1 | 列名2 | 列名3 |
  | ----: | ----: | ----: |
  |     1 |     2 |     3 |
  |     4 |     5 |     6 |
  |     7 |     8 |     9 |

  | 列名1 | 列名2 | 列名3 |
  | :---- | :---- | :---- |
  | 1     | 2     | 3     |
  | AAAA  | BBBB  | CCCC  |
  | END1  | END2  | END3  |

  | 列名1 | 列名2 | 列名3 |
  | :---- | :---- | :---- |
  | 1     | 2     | 3     |
  | AAAA  | BBBB  | CCCC  |
  | END1  | END2  | END3  |

  | 列名1 | 列名2 | 列名3 |
  | :---- | :---- | :---- |
  | 1     | 2     | 3     |
  | AAAA  | BBBB  | CCCC  |
  | END1  | END2  | END3  |

  | 列名1 | 列名2 | 列名3 |
  | ----: | :---: | :---- |
  |     1 |   2   | 3     |
  |  AAAA | BBBB  | CCCC  |
  |  END1 | END2  | END3  |

  | 列名1 | 列名2 | 列名3 |
  | :---- | :---: | ----: |
  | 1     |   2   |     3 |
  | AAAA  | BBBB  |  CCCC |
  | END1  | END2  |  END3 |
```

# 脚注

Orgの脚注をMarkdownの様式に変換します。脚注のラベルは番号を使います。
Markdownファイルの最後に「#脚注」見出しが設けられ、そこに脚注が書き出されます。
脚注は記事の場合のみ正しく動作します。本の場合には正しく動作しません。

参照

- [Org mode公式 Creating Footnotes](https://orgmode.org/manual/Creating-Footnotes.html)
- [Zenn公式 脚注](https://zenn.dev/zenn/articles/markdown-guide#%E8%84%9A%E6%B3%A8)

Org:

```
  Org mode[fn:org] is for keeping notes, maintaining TODO lists, planning projects, and authoring documents with a fast and effective plain-text system.

  [fn:org] org-mode [[http://orgmode.org]]
```

Markdown:

```
  Org mode[^1] is for keeping notes, maintaining TODO lists, planning projects, and authoring documents with a fast and effective plain-text system.

  # 脚注

  [^1]: org-mode <http://orgmode.org>
```

# ハイパーリンク

リンクを設けます。ただしリンク先が画像ファイルの場合は、[画像](#%E7%94%BB%E5%83%8F)を参照してください。

参照

- [Org mode公式 Hyperlinks](https://orgmode.org/guide/Hyperlinks.html)
- [Zenn公式 テキストリンク](https://zenn.dev/zenn/articles/markdown-guide#%E3%83%86%E3%82%AD%E3%82%B9%E3%83%88%E3%83%AA%E3%83%B3%E3%82%AF)

## 外部リンク

Orgの外部リンク。

参照

- [Org mode公式 External Links](https://orgmode.org/manual/External-Links.html)

Org:

```
  [[https://orgmode.org/][Org Mode]]
  [[https://orgmode.org]] はOrg Modeの公式サイトです。
  [[https://zenn.dev]]
```

Markdown:

```
  [Org Mode](https://orgmode.org)
  <https://orgmode.org> はOrg Modeの公式サイトです。
  https://zenn.dev
```

## 内部リンク

Orgの内部リンク機能には、以下の6種類があります。 `ox-yazenn` では一つを除いてすべて対応しています。Markdownではリンク先が属する見出しに自動生成されるアンカーへのリンクに代替します。Zennのアンカー用id文字列の生成規則に対応しています。
最後の `#+NAME:` キーワードを用いた内部リンクはMarkdownでは表現できません。

- 2重アングルブラケットを用いてリンク先 `<​<ターゲット>>` を設置して、 `[​[ターゲット]]` でリンクする
- ヘッドラインにCUSTOM_IDやIDプロパティを設置せずに、ヘッドラインに対して `[​[*ヘッドラインテキスト]]` でリンクする
- ヘッドラインにCUSTOM_IDプロパティを設置して、ヘッドラインに対して `[​[*ヘッドラインテキスト]]` でリンクする
- ファイルやヘッドラインにIDプロパティを設置して、ヘッドラインに対して `<id​:IDプロパティの値>` もしくは `<id​:IDプロパティの値::*ヘッドラインテキスト>` でリンクする
- 3重アングルブラケットを用いてリンク先 `<<<ラジオターゲット>​>>` を設置すると、 `ラジオターゲット` の文字列が自動でリンクとなる
- 表やリスト、ブロックに対して `#+NAME:` キーワードを用いて名前を設置、 `[​[名前]]` でリンクする

### 本のチャプターを跨ぐ内部リンクについて

一つのOrg modeファイルから本用のMarkdownを生成する場合、チャプター毎にMarkdownが生成されますが、見出しアンカーに対するリンクはファイルを跨いでも正しく機能します。そのためには `org-yazenn-zenndev-username` 変数にZenn.devのサイトのユーザー名を設定しておく必要があります。

参照

- [Org mode公式 Internal Links](https://orgmode.org/manual/Internal-Links.html)
- [Org mode公式 Handling Links](https://orgmode.org/manual/Handling-Links.html)
- [Org mode公式 Radio Targets](https://orgmode.org/manual/Radio-Targets.html)
- [ZennでのMarkdownページ内リンクの書き方](https://zenn.dev/k_kuroguro/articles/759fefb5b07667)

Org:

```
  * 見出し1
  :PROPERTIES:
  :CUSTOM_ID: customid1
  :END:

  2順アングルブラケットを用いてリンク先 <<2重アングルブラケット>> を設置する。

  * 見出し2
  :PROPERTIES:
  :ID: midashi2
  :END:

  ここに<<<ラヂオ>>>を置く。

  * リンクテスト

  [[*見出し1]]にリンクします。
  [[2重アングルブラケット]]がある見出しへのリンクです。
  midashi2のIDがある見出しは、<id:midashi2>です。
  ラヂオには自動でリンクが置かれる。
```

Markdown:

```
  # 見出し1

  2順アングルブラケットを用いてリンク先 2重アングルブラケット を設置する。

  # 見出し2

  ここに**ラヂオ**を置く。

  # リンクテスト

  [見出し1](#%E8%A6%8B%E5%87%BA%E3%81%971)にリンクします。
  [2重アングルブラケット](#%E8%A6%8B%E5%87%BA%E3%81%971)がある見出しへのリンクです。
  midashi2のIDがある見出しは、[見出し2](#%E8%A6%8B%E5%87%BA%E3%81%972)です。
  [ラヂオ](#%E8%A6%8B%E5%87%BA%E3%81%972)には自動でリンクが置かれる。
```

# 画像

Orgでは画像はリンクと同じ形式で記述します。Markdownの画像表現に変換されます。

参照

- [Zenn公式 画像](https://zenn.dev/zenn/articles/markdown-guide#%E7%94%BB%E5%83%8F)
- [Zenn公式 GitHubリポジトリ連携で画像をアップロードする方法](https://zenn.dev/zenn/articles/deploy-github-images)

Org:

```
  [[/images/image1.png]]
```

Markdown:

```
  ![](/images/image1.png)
```

画像にリンクを設置する場合は以下のように記述します。

参照

- [Zenn公式 画像にリンクを貼る](https://zenn.dev/zenn/articles/markdown-guide#%E7%94%BB%E5%83%8F%E3%81%AB%E3%83%AA%E3%83%B3%E3%82%AF%E3%82%92%E8%B2%BC%E3%82%8B)

Org:

```
  [[/images/image1.png][https://zenn.dev]]
```

Markdown:

```
  [![](/images/image1.png)](https://zenn.dev)
```

画像にAltテキストや、キャプション、横幅を指定するには、[画像のAltテキスト、キャプション、横幅指定](#%E7%94%BB%E5%83%8F%E3%81%AEalt%E3%83%86%E3%82%AD%E3%82%B9%E3%83%88%E3%80%81%E3%82%AD%E3%83%A3%E3%83%97%E3%82%B7%E3%83%A7%E3%83%B3%E3%80%81%E6%A8%AA%E5%B9%85%E6%8C%87%E5%AE%9A)を参照してください。Zenn仕様のMarkdownの機能を利用できます。

# Markdownフラグメント

Markdownとしてそのまま出力したい場合に利用します。
Orgの記法は3種類あります。

参照

- [Org mode公式 HTML Export](https://orgmode.org/guide/HTML-Export.html)
- [Org mode公式 Quoting LaTeX code](https://orgmode.org/manual/Quoting-LaTeX-code.html)

## `@@zenn:` `@@` を使って部分的にそのまま出力する

Org:

```
  ここは@@zenn: \# スニペットこのまま@@出力されます。
```

Markdown:

```
  ここは \# スニペットこのまま出力されます。
```

## `#+MD:` `#+MARKDOWN:` `#+ZENN:` キーワードで行をそのまま出力する

Org:

```
  #+MD: ## この行はそのまま出力される
```

Markdown:

```
  ## この行はそのまま出力される
```

## exportブロックを使ってブロック内をそのまま出力する

exportブロックのパラメータには、mdかmarkdownかzennのいずれかを指定します。

Org:

```
  #+begin_export md
  ここは
  そのまま
  出力されるブロック
  #+end_export
```

Markdown:

```
  ここは
  そのまま
  出力されるブロック
```

# 目次

目次は、 `#+OPTIONS: toc:t` (org-export-with-toc) の値に影響を受けることなく、Markdownには出力されません。ZennではMarkdownから自動で目次が生成されます。

参照

- [Org mode公式 Table of Contents](https://orgmode.org/manual/Table-of-Contents.html)

# 画像のAltテキスト、キャプション、横幅指定


## 画像にAltテキストを指定する

Orgでは表現方法はありませんが、HTMLバックエンドでは `#+ATTR_HTML: :alt Altテキスト` で指定するようですので、倣って `#+ATTR_ZENN:` キーワードを用います。

参照

- [Zenn公式 Altテキストを指定する](https://zenn.dev/zenn/articles/markdown-guide#alt%E3%83%86%E3%82%AD%E3%82%B9%E3%83%88%E3%82%92%E6%8C%87%E5%AE%9A%E3%81%99%E3%82%8B)

Org:

```
  #+ATTR_ZENN: :alt Altテキスト
  [[/images/image1.png]]
```

Markdown:

```
  ![Altテキスト](/images/image1.png)
```

## 画像にキャプションをつける

HTMLバックエンドに倣って `#+CAPTION:` キーワードを用います。

参照

- [Zenn公式 キャプションをつける](https://zenn.dev/zenn/articles/markdown-guide#%E3%82%AD%E3%83%A3%E3%83%97%E3%82%B7%E3%83%A7%E3%83%B3%E3%82%92%E3%81%A4%E3%81%91%E3%82%8B)

Org:

```
  #+CAPTION: キャプション
  [[/images/image1.png]]
```

Markdown:

```
  ![](/images/image1.png)
  *キャプション*
```

## 画像の横幅を指定する

`#+ATTR_ZENN: :width` を用います。Zenn固有のパラメータです。

参照

- [Zenn公式 画像の横幅を指定する](https://zenn.dev/zenn/articles/markdown-guide#%E7%94%BB%E5%83%8F%E3%81%AE%E6%A8%AA%E5%B9%85%E3%82%92%E6%8C%87%E5%AE%9A%E3%81%99%E3%82%8B)

Org:

```
  #+ATTR_ZENN: :width 250
  [[/images/image1.png]]
```

Markdown:

```
  ![](/images/image1.png =250x)
```

## Altテキスト、キャプション、横幅を指定する

組み合わせて指定できます。

Org:

```
  #+CAPTION: キャプション
  #+ATTR_ZENN: :alt Altテキスト :width 250
  [[/images/image1.png]]
```

Markdown:

```
  ![Altテキスト](/images/image1.png =250x)
  *キャプション*
```

# コードブロックのファイル名およびdiff属性付加

コードブロックにファイル名を指定したり、diffのシンタックスハイライトを適用したい場合は #+ATTR_ZENN: キーワードをsrcブロックの前に記述し、caption属性やdiff属性を指定します。

[Zenn公式 コードブロック](https://zenn.dev/zenn/articles/markdown-guide#%E3%82%B3%E3%83%BC%E3%83%89%E3%83%96%E3%83%AD%E3%83%83%E3%82%AF)

Org:

```
  #+ATTR_ZENN: :caption foobar.js
  #+begin_src js 
  const great = () => {
    console.log("Awesome")
  }
  #+end_src

  #+ATTR_ZENN: :diff t
  #+begin_src js 
  @@ -4,6 +4,5 @@
  +    const foo = bar.baz([1, 2, 3]) + 1;
  -    let foo = bar.baz([1, 2, 3]);
  #+end_src

  #+ATTR_ZENN: :diff t :caption foobar.js
  #+begin_src js 
  @@ -4,6 +4,5 @@
  +    const foo = bar.baz([1, 2, 3]) + 1;
  -    let foo = bar.baz([1, 2, 3]);
  #+end_src
```

Markdown:

```
  ​```js:foobar.js
  const great = () => {
    console.log("Awesome")
  }
  ​```

  ​```diff js
  @@ -4,6 +4,5 @@
  +    const foo = bar.baz([1, 2, 3]) + 1;
  -    let foo = bar.baz([1, 2, 3]);
  ​```

  ​```diff js:foobar.js
  @@ -4,6 +4,5 @@
  +    const foo = bar.baz([1, 2, 3]) + 1;
  -    let foo = bar.baz([1, 2, 3]);
  ​```
```

# スペシャルブロック(message,alert,quote)

Zenn独自の記法、メッセージ、警告メッセージ、アコーディオン、またはそれら要素のネストに対応しています。

参照

- [Zenn独自の記法 メッセージ](https://zenn.dev/zenn/articles/markdown-guide#%E3%83%A1%E3%83%83%E3%82%BB%E3%83%BC%E3%82%B8)
- [Zenn独自の記法 アコーディオン（トグル）](https://zenn.dev/zenn/articles/markdown-guide#%E3%82%A2%E3%82%B3%E3%83%BC%E3%83%87%E3%82%A3%E3%82%AA%E3%83%B3%EF%BC%88%E3%83%88%E3%82%B0%E3%83%AB%EF%BC%89)
- [Zenn独自の記法 要素をネストさせるには](https://zenn.dev/zenn/articles/markdown-guide#%E8%A6%81%E7%B4%A0%E3%82%92%E3%83%8D%E3%82%B9%E3%83%88%E3%81%95%E3%81%9B%E3%82%8B%E3%81%AB%E3%81%AF)

Org:

```
  #+begin_message
  メッセージ
  #+end_message

  #+attr_zenn: :type message
  #+begin_quote
  メッセージはquoteブロックでも可
  #+end_quote

  #+begin_alert
  警告メッセージ
  #+end_alert

  #+attr_zenn: :type alert
  #+begin_quote
  警告メッセージもquoteブロックでも可
  #+end_quote

  #+attr_zenn: :type details :title タイトル
  #+begin_quote
  表示したい内容
  #+end_quote
```

Markdown:

```
  :::message
  メッセージ
  :::

  :::message
  メッセージはquoteブロックでも可
  :::

  :::message alert
  警告メッセージ
  :::

  :::message alert
  警告メッセージもquoteブロックでも可
  :::

  :::details タイトル
  表示したい内容
  :::
```

quoteブロックとの併用で、ネストすることもできます。

Org:

```
  #+attr_zenn: :type message
  #+begin_quote
  メッセージ外
  #+begin_message
  メッセージ中
  #+end_message
  #+begin_alert
  メッセージ中アラート
  #+end_alert
  #+end_quote

  #+begin_message
  メッセージ外
  #+attr_zenn: :type details :title タイトル
  #+begin_quote
  メッセージ中
  #+end_quote
  #+end_message

  #+attr_zenn: :type details :title タイトル
  #+begin_quote
  #+begin_message
  アコーディオンメッセージ中
  #+end_message
  #+end_quote
```

Markdown:

```
  ::::message
  メッセージ外
  :::message
  メッセージ中
  :::
  :::message alert
  メッセージ中アラート
  :::
  ::::

  ::::message
  メッセージ外
  :::details タイトル
  メッセージ中
  :::
  ::::

  ::::details タイトル
  :::message
  アコーディオンメッセージ中
  :::
  ::::
```

# コンテンツの埋め込み

Zennのコンテンツの埋め込み機能に対応する記述方法です。
基本的には、URLのみの行を記述するだけで機能します。

[Zenn公式 コンテンツの埋め込み](https://zenn.dev/zenn/articles/markdown-guide#%E3%82%B3%E3%83%B3%E3%83%86%E3%83%B3%E3%83%84%E3%81%AE%E5%9F%8B%E3%82%81%E8%BE%BC%E3%81%BF)

## リンクカード

URLだけの行、もしくはURLのリンク形式だけの行は、Zennのリンクカード機能の対象になります。アンダースコアが含まれるURLの場合、 `@[card](URL}` 形式で出力されます。

参照

- [Zenn公式 リンクカード](https://zenn.dev/zenn/articles/markdown-guide#%E3%83%AA%E3%83%B3%E3%82%AF%E3%82%AB%E3%83%BC%E3%83%89)

Org:

```
  https://orgmode.org

  [[https://orgmode.org]]

  https://zenn.dev/__dummy

  [[https://zenn.dev/__dummy]]
```

Markdown:

```
  https://orgmode.org

  https://orgmode.org

  @[card](https://zenn.dev/__dummy)

  @[card](https://zenn.dev/__dummy)
```

## X（Twitter）のポスト（ツイート）

[リンクカード](#%E3%83%AA%E3%83%B3%E3%82%AF%E3%82%AB%E3%83%BC%E3%83%89)と同様に、x.comのポストに対するURLだけの行を記述します。前後に改行が必要です。

Org:

```

  https://x.com/elonmusk/status/1685096284275802112

  [[https://x.com/elonmusk/status/1685096284275802112]]
```

Markdown:

```

  https://x.com/elonmusk/status/1685096284275802112

  https://x.com/elonmusk/status/1685096284275802112
```

## YouTube

[リンクカード](#%E3%83%AA%E3%83%B3%E3%82%AF%E3%82%AB%E3%83%BC%E3%83%89)と同様に、www.youtube.comのポストに対するURLだけの行を記述します。前後に改行が必要です。

Org:

```

  https://www.youtube.com/watch?v=WRVsOCh907o

  [[https://www.youtube.com/watch?v=WRVsOCh907o]]
```

Markdown:

```

  https://www.youtube.com/watch?v=WRVsOCh907o

  https://www.youtube.com/watch?v=WRVsOCh907o
```

## GitHub

[リンクカード](#%E3%83%AA%E3%83%B3%E3%82%AF%E3%82%AB%E3%83%BC%E3%83%89)と同様に、github.comのポストに対するURLだけの行を記述します。前後に改行が必要です。

Org:

```

  https://github.com/octocat/Hello-World/blob/master/README

  https://github.com/octocat/Spoon-Knife/blob/main/README.md#L1-L3

  https://github.com/octocat/Spoon-Knife/blob/main/README.md#L3

  [[https://github.com/octocat/Hello-World/blob/master/README]]

  [[https://github.com/octocat/Spoon-Knife/blob/main/README.md#L1-L3]]

  [[https://github.com/octocat/Spoon-Knife/blob/main/README.md#L3]]
```

Markdown:

```

  https://github.com/octocat/Hello-World/blob/master/README

  https://github.com/octocat/Spoon-Knife/blob/main/README.md#L1-L3

  https://github.com/octocat/Spoon-Knife/blob/main/README.md#L3

  https://github.com/octocat/Hello-World/blob/master/README

  https://github.com/octocat/Spoon-Knife/blob/main/README.md#L1-L3

  https://github.com/octocat/Spoon-Knife/blob/main/README.md#L3
```

## GitHub Gist

[リンクカード](#%E3%83%AA%E3%83%B3%E3%82%AF%E3%82%AB%E3%83%BC%E3%83%89)と同様に、gist.github.comのポストに対するURLだけの行を記述します。

Org:

```
  https://gist.github.com/foo/bar

  [[https://gist.github.com/foo/bar]]

  https://gist.github.com/foo/bar?file=example.json

  [[https://gist.github.com/foo/bar?file=example.json]]
```

Markdown:

```
  https://gist.github.com/foo/bar

  https://gist.github.com/foo/bar

  https://gist.github.com/foo/bar?file=example.json

  https://gist.github.com/foo/bar?file=example.json
```

## CodePen

[リンクカード](#%E3%83%AA%E3%83%B3%E3%82%AF%E3%82%AB%E3%83%BC%E3%83%89)と同様に、codepen.ioの埋め込み用URLだけの行を記述します。

Org:

```
  https://codepen.io/ページ

  [[https://codepen.io/ページ]]
```

Markdown:

```
  @[codepen](https://codepen.io/ページ)

  @[codepen](https://codepen.io/ページ)
```

## SlideShare

[リンクカード](#%E3%83%AA%E3%83%B3%E3%82%AF%E3%82%AB%E3%83%BC%E3%83%89)と同様に、www.slideshare.netの埋め込み用URLだけの行を記述します。

Org:

```
  https://www.slideshare.net/slideshow/embed_code/key/testcode

  [[https://www.slideshare.net/slideshow/embed_code/key/testcode]]
```

Markdown:

```
  @[slideshare](testcode)

  @[slideshare](testcode)
```

## SpeakerDeck

[リンクカード](#%E3%83%AA%E3%83%B3%E3%82%AF%E3%82%AB%E3%83%BC%E3%83%89)と同様に、speakerdeck.netの埋め込み用URLだけの行を記述します。

Org:

```
  https://speakerdeck.com/player/testcode

  [[https://speakerdeck.com/player/testcode]]

  https://speakerdeck.com/player/testcode?slide=24

  [[https://speakerdeck.com/player/testcode?slide=24]]
```

Markdown:

```
  @[speakerdec](testcode)

  @[speakerdec](testcode)

  @[speakerdec](testcode?slide=24)

  @[speakerdec](testcode?slide=24)
```

## Docswell

[リンクカード](#%E3%83%AA%E3%83%B3%E3%82%AF%E3%82%AB%E3%83%BC%E3%83%89)と同様に、www.docswell.comの埋め込み用URLだけの行を記述します。

Org:

```
  https://www.docswell.com/s/{UserId}/{SlideId}-xxx-xxx

  https://www.docswell.com/slide/{Slided}/embed

  [[https://www.docswell.com/s/{UserId}/{SlideId}-xxx-xxx]]

  [[https://www.docswell.com/slide/{Slided}/embed]]
```

Markdown:

```
  @[docswell](https://www.docswell.com/s/{UserId}/{SlideId}-xxx-xxx)

  @[docswell](https://www.docswell.com/slide/{Slided}/embed)

  @[docswell](https://www.docswell.com/s/{UserId}/{SlideId}-xxx-xxx)

  @[docswell](https://www.docswell.com/slide/{Slided}/embed)
```

## JSFiddle

[リンクカード](#%E3%83%AA%E3%83%B3%E3%82%AF%E3%82%AB%E3%83%BC%E3%83%89)と同様に、jsfiddle.netの埋め込み用URLだけの行を記述します。

Org:

```
  https:://jsfiddle.com/ページ

  https:://jsfiddle.com/ページ/embedded/{Tabs}/{Visual}/

  [[https:://jsfiddle.com/ページ]]

  [[https:://jsfiddle.com/ページ/embedded/{Tabs}/{Visual}/]]
```

Markdown:

```
  @[jsfiddle](https:://jsfiddle.com/ページ)

  @[jsfiddle](https:://jsfiddle.com/ページ/embedded/{Tabs}/{Visual}/)

  @[jsfiddle](https:://jsfiddle.com/ページ)

  @[jsfiddle](https:://jsfiddle.com/ページ/embedded/{Tabs}/{Visual}/)
```

## CodeSandbox

[リンクカード](#%E3%83%AA%E3%83%B3%E3%82%AF%E3%82%AB%E3%83%BC%E3%83%89)と同様に、codesandbox.ioの埋め込み用URLだけの行を記述します。

Org:

```
  https:://codesandbox.io/iframeに含まれるsrcのURL

  [[https:://codesandbox.io/iframeに含まれるsrcのURL]]
```

Markdown:

```
  @[codesandbox](https:://codesandbox.io/iframeに含まれるsrcのURL)

  @[codesandbox](https:://codesandbox.io/iframeに含まれるsrcのURL)
```

## StackBlitz

[リンクカード](#%E3%83%AA%E3%83%B3%E3%82%AF%E3%82%AB%E3%83%BC%E3%83%89)と同様に、stackblitz.comの埋め込み用URLだけの行を記述します。

Org:

```
  https://stackblitz.com/埋め込み用URL

  [[https://stackblitz.com/埋め込み用URL]]
```

Markdown:

```
  @[stackblitz](https://stackblitz.com/埋め込み用URL)

  @[stackblitz](https://stackblitz.com/埋め込み用URL)
```

## Figma

[リンクカード](#%E3%83%AA%E3%83%B3%E3%82%AF%E3%82%AB%E3%83%BC%E3%83%89)と同様に、www.figma.comの埋め込み用URLだけの行を記述します。

Org:

```
  https://www.figma.com/ファイルまたはプロトタイプのURL

  [[https://www.figma.com/ファイルまたはプロトタイプのURL]]
```

Markdown:

```
  @[figma](https://www.figma.com/ファイルまたはプロトタイプのURL)

  @[figma](https://www.figma.com/ファイルまたはプロトタイプのURL)
```

## blueprintUE

[リンクカード](#%E3%83%AA%E3%83%B3%E3%82%AF%E3%82%AB%E3%83%BC%E3%83%89)と同様に、blueprintue.comのURLだけの行を記述します。

Org:

```
  https://blueprintue.com/ページのURL

  [[https://blueprintue.com/ページのURL]]
```

Markdown:

```
  @[blueprintue](https://blueprintue.com/ページのURL)

  @[blueprintue](https://blueprintue.com/ページのURL)
```

# コンテンツの埋め込みに関する設定

コンテンツの埋め込みに関する設定は、 `org-yazenn-special-link-alist` 変数に設定されています。 `ox-yazenn-conf.el` を参照してください。

```lisp
(defcustom org-yazenn-special-link-alist '(("github.com" . "")
                                           ("www.youtubu.com" . "")
                                           ("x.com" . "")
                                           ("twitter.com" . "")
                                           ("gist.github.com" . "gist")
                                           ("codepen.io" . "codepen")
                                           ("www.slideshare.net" . "slideshare")
                                           ("speakerdeck.com" . "speakerdeck")
                                           ("www.docswell.com" . "docswell")
                                           ("jsfiddle.net" . "jsfiddle")
                                           ("codesandbox.io" . "codesandbox")
                                           ("stackblitz.com" . "stackblitz")
                                           ("www.figma.com" . "figma")
                                           ("blueprintue.com" . "blueprintue"))
  "Alist of embed url list available in Zenn.dev.

The key is a string to host of url.  The value are a string to correspoinding
string.  An Empty string is specified, then it is represented as is."
  :group 'org-export-yazenn
  :type 'alist)
```

# ダイアグラム

`mermaid.js` によるダイアグラム表示を利用する場合、srcブロックで言語名を `mermaid` と記述します。

参照

- [Zenn公式 ダイアグラム](https://zenn.dev/zenn/articles/markdown-guide#%E3%83%80%E3%82%A4%E3%82%A2%E3%82%B0%E3%83%A9%E3%83%A0)

Org:

```
  #+begin_src mermaid
  graph TB
      A[Hard edge] -->|Link text| B(Round edge)
      B --> C{Decision}
      C -->|One| D[Result one]
      C -->|Two| E[Result two]
  #+end_src
```

Markdown:

```
  ​```mermaid
  graph TB
      A[Hard edge] -->|Link text| B(Round edge)
      B --> C{Decision}
      C -->|One| D[Result one]
      C -->|Two| E[Result two]
  ​```
```
