---
title: "emacsの環境をどこでもシュッと復元する"
emoji: "🔖"
type: "tech"
topics: ["emacs"]
published: true
---

# はじめに

emacsに慣れていたにも関わらず、emacsから離れてしまった人の話をときどき見かけます。「設定が面倒になった」「起動時間が長くなった、反応が鈍くなった」と感じることがあるのではないかと予想します。emacsを使い続けている私も、少し前までemacsやめたいって気持ちがありました。ところが、非同期パッケージマネージャ[Elpaca](https://github.com/progfolio/elpaca)と[SetupEl](https://www.emacswiki.org/emacs/SetupEl)を使い出してから、まだしばらくはemacsを使う環境を維持しようという気分でいます。

この文書ではemacsの設定に関して以下のことを実現する内容を記します。

- 設定をゼロから自動で復元
- 非同期処理による起動時間短縮
- 利用中のパッケージ更新を素早く行う

なお、私が[Elpaca](https://github.com/progfolio/elpaca)と[SetupEl](https://www.emacswiki.org/emacs/SetupEl)を使い始めたのは、こちらの[ブログ Apribase](https://apribase.net/2024/05/29/emacs-elpaca-setup-el/)を読んだのがきっかけです。非常に参考になりますので併せて読むことをおすすめします。

# ElpacaとSetupElの概要

[Elpaca](https://github.com/progfolio/elpaca)を利用すると、非同期にパッケージをダウンロードしたり、elispの評価をキューに入れて `init.el` の後に実行する機能がemacsに備わります。ダウンロード済みのパッケージをキャッシュしたり、パッケージを個別または一括で更新する機能があります。
パッケージの取得元は、ELPAやMELPA、githubやcodebergのgitリポジトリなど多岐に対応しています。

[SetupEl](https://www.emacswiki.org/emacs/SetupEl)は `use-package` マクロと同様の役割を担います。パッケージ間の依存関係や評価順、ロード前後の処理を定義できます。 `use-package` 同様にパッケージ単位で設定を記述するため、可読性が上がる効果があります。

# ElpacaとSetupElを使用した設定例

以下は[SetupEl](https://www.emacswiki.org/emacs/SetupEl)に[Elpaca](https://github.com/progfolio/elpaca)を組み合わせた設定例です。

```lisp
(setup foo-package
  (:elpaca t)  ;; foo-packageをelpaca経由で取得
  (:load-after bar-package)  ;; bar-package がロードされた後に foo-package を評価
  (:when-loaded  ;; foo-package がロードされた後に評価
    (
     ...
     ))
  (:with-mode (rust-ts-mode)  ;; rust-ts-mode で foo-hook をフック関数として登録
    (:hook foo-hook))
  (:with-map foo-map
    (:bind
     "C-j" foo-one
     "C-k" foo-two)))
```

# Elpacaを使う設定

`elpaca` 関数で各種パッケージがインストールできるようになります。

```lisp
(elpaca パッケージ名 nil)
```

## (Windows)シンボリックを作成できるようにする

管理者権限がある場合は、以下の手順でシンボリックリンクを作成できるようにします。

1. secpol.mscを起動
2. 左のペインで[ローカルポリシー]「ユーザー権利の割り当て」を選択
3. 右のペインで「シンボリック リンクの作成」をダブルクリック
4. 「ユーザーまだはグループの追加(U)...」ボタンを押す
5. ユーザー名やUsersグループを追加

管理者権限がない場合は、elpacaが準備する回避策[(Windows)シンボリックリンクが作れないときの代替手段](#%28windows%29%E3%82%B7%E3%83%B3%E3%83%9C%E3%83%AA%E3%83%83%E3%82%AF%E3%83%AA%E3%83%B3%E3%82%AF%E3%81%8C%E4%BD%9C%E3%82%8C%E3%81%AA%E3%81%84%E3%81%A8%E3%81%8D%E3%81%AE%E4%BB%A3%E6%9B%BF%E6%89%8B%E6%AE%B5)を利用します。(筆者はこの方法でうまくいったことはないですけども)

## emacs標準のpackage.elを無効にする

`early-init.el` に以下の設定をします。elpacaはpackage.elの機能を包含しています。

```lisp
(setq package-enable-at-startup nil)
```

## Elpacaをインストールするコード

[Elpaca](https://github.com/progfolio/elpaca)の[Installer](https://github.com/progfolio/elpaca#installer)にあるコードを `init.el` にコピーペーストします。

## (Windows)シンボリックリンクが作れないときの代替手段

管理者権限がなくシンボリックリンクを作れない場合は、以下を実行します。

```lisp
(elpaca-no-symlink-mode)
```

# setup.el と Elpaca を組み合わせる


## setup.el のインストール

[SetupEl](https://www.emacswiki.org/emacs/SetupEl)の **Using Elpaca** の項目にあるように、elpaca関数を利用して setup.el をインストールします。 

```lisp
(elpaca setup (require 'setup))
(elpaca-wait)
```

## setup.el の書式で elpaca を呼び出せるようにする

setup の書式を使って elpaca 関数に展開されるマクロを準備します。
[SetupEl](https://www.emacswiki.org/emacs/SetupEl)の **Using Elpaca** の項目の2つ目のコードです。

```lisp
(defun setup-wrap-to-install-package (body _name)
"Wrap BODY in an `elpaca' block if necessary.
The body is wrapped in an `elpaca' block if `setup-attributes'
contains an alist with the key `elpaca'."
(if (assq 'elpaca setup-attributes)
    `(elpaca ,(cdr (assq 'elpaca setup-attributes)) ,@(macroexp-unprogn body))
  body))
;; Add the wrapper function
(add-to-list 'setup-modifier-list #'setup-wrap-to-install-package)
(setup-define :elpaca
  (lambda (order &rest recipe)
    (push (cond
       ((eq order t) `(elpaca . ,(setup-get 'feature)))
       ((eq order nil) '(elpaca . nil))
       (`(elpaca . (,order ,@recipe))))
      setup-attributes)
    ;; If the macro wouldn't return nil, it would try to insert the result of
    ;; `push' which is the new value of the modified list. As this value usually
    ;; cannot be evaluated, it is better to return nil which the byte compiler
    ;; would optimize away anyway.
    nil)
  :documentation "Install ORDER with `elpaca'.
The ORDER can be used to deduce the feature context."
  :shorthand #'cadr)
```

これで、 以下のようなコードが利用可能になります。 `:elpaca` キーワードの体裁で elpaca 関数を呼び出せます。

```lisp
(setup rust-mode
  (:elpaca t)
  (:option rust-format-on-save t
           rust-mode-treesitter-derive t)
  (:hook eglot-ensure))
```

`:elpaca` の設定例をいくつか記します。

```lisp
;; ELPAやMELPAにあるパッケージの場合
(setup f
  (:elpaca t))

;; githubのリポジトリの場合
(setup eglot
  (:elpaca eglot :host github :repo "joaotavora/eglot"))

;; githubのリポジトリ、elファイルがあるディレクトリを指定
(setup corfu
  (:elpaca corfu :host github :repo "minad/corfu" :files (:defaults "extensions/*")))

;; codebergのリポジトリ
(setup corfu-terminal 
  (:elpaca corfu-terminal :host "codeberg.org" :repo "https://codeberg.org/akib/emacs-corfu-terminal.git"))
```

setup.el を使った設定方法に関しては、[ブログ Apribase](https://apribase.net/2024/05/29/emacs-elpaca-setup-el/)が詳しいです。

あとは、好みの設定をsetupマクロ使って `init.el` に書きます。

# 設定をゼロから自動で復元

elpaca と setup を使用して自分好みの `early-init.el` や `init.el` を準備してしまえば、後はemacsを利用したい環境にそれらをコピーしてemacsを起動するだけで、サードパーティのパッケージが自動でインストールされ、好みの環境が復元されます。
パッケージがインストールされてく様子は視覚的に表示されます。fetch処理に失敗したり依存関係の設定ミスがあると、初期化処理は停止します。その場合は、再度emacsを起動しなおしたり、依存関係の見直しを行えば解決します。

# 非同期処理による起動時間短縮

elpaca はパッケージを非同期にダウンロードします。さらにelispの評価をキューに入れて `init.el` の後に実行します。elpaca を利用するだけで、起動時間短縮効果が期待できます。

# 利用中のパッケージ更新を素早く行う

`M-x elpaca-update-all` を実行すると、インストール済みのパッケージの更新が行われます。 `elpaca-update` でパッケージ単位で個別にアップデートを行うことも可能です。インストール済みのパッケージのリストは `M-x elpaca-manager` で得られます。パッケージの詳細情報は、 `elpaca-info` で確認できます。

# elpacaのキャッシュの位置

elpacaのキャッシュの位置は、 `elpaca-directory` 変数の値の場所です。
elpacaのインストール用コードに以下のような記述があります。

```lisp
(defvar elpaca-directory (expand-file-name "elpaca/" user-emacs-directory))
```

これにより、 `~/.emacs.d/elpaca` がキャシュに使用されるディレクトリです。
ソースなどもここに置かれます。
例えばこのディレクトリを削除してemacsを再起動すると、パッケージのfetch処理から再実行されます。

# elpaca自体の更新

elpaca自体も更新されることがあります。[Installer](https://github.com/progfolio/elpaca#installer)のコードを確認して、 `elpaca-installer-version` 変数の値を現在利用中のものと比較して確認します。
もし更新されているようでしたら、コピー・ペーストし直してemacsを再起動します。

# おわりに

設定作業の煩わしさ軽減と起動時間短縮を実現すると、emacsを使う楽しさを思い出すこ
と間違いなしです。
