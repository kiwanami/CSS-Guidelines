# General CSS notes, advice and guidelines
# 一般的CSS注釈、勧告、ガイドライン

---

* [Original](https://github.com/csswizardry/CSS-Guidelines/blob/master/README.md)

---

In working on large, long running projects with dozens of developers, it is
important that we all work in a unified way in order to, among other things:

大きくて長期間に渡るプロジェクトに、多くの開発者が関わって一緒に働く場合、
他の約束事と同様に、以下のような目的のために統一された方法を用いることが重要です。

* Keep stylesheets maintainable
* Keep code transparent and readable
* Keep stylesheets scalable

* スタイルシートをメンテナンス可能にし続ける
* コードを見通しよく読みやすいように維持する
* スタイルシートを拡張可能に保つ

There are a variety of techniques we must employ in order to satisfy these
goals.

これらのゴール達成のためは、いくつかの技術的手法があります。

The first part of this document will deal with syntax, formatting and CSS
anatomy, the second part will deal with approach, mindframe and attitude toward
writing and architecting CSS. Exciting, huh?

この文書では、まず最初に文法やフォーマット、CSSの構成について述べて、
後半ではCSSを書いたり設計するにあたって、考え方や取っ掛かりについて述べます。
どうですか？面白そうでしょう？

## Contents

* [CSS document anatomy](#css-document-anatomy)
* [CSSファイルの構成](#css-document-anatomy)
  * [General](#general)
  * [一般](#general)
  * [One file vs. many files](#one-file-vs-many-files)
  * [ひとつのファイル vs. 複数ファイル](#one-file-vs-many-files)
  * [Table of contents](#table-of-contents)
  * [目次](#table-of-contents)
  * [Section titles](#section-titles)
  * [セクションタイトル](#section-titles)
* [Source order](#source-order)
* [ソースファイルの順番](#source-order)
* [Anatomy of rulesets](#anatomy-of-rulesets)
* [ルールセット構成](#anatomy-of-rulesets)
* [Naming conventions](#naming-conventions)
* [名前付け方法](#naming-conventions)
  * [JS hooks](#js-hooks)
  * [JS連携](#js-hooks)
  * [Internationalisation](#internationalisation)
  * [国際化](#internationalisation)
* [Comments](#comments)
* [コメント](#comments)
  * [Comments on steroids](#comments-on-steroids)
  * [コメントでCSS強化](#comments-on-steroids)
    * [Quasi-qualified selectors](#quasi-qualified-selectors)
    * [準限定セレクター](#quasi-qualified-selectors)
    * [Tagging code](#tagging-code)
    * [コードへのタグ付け](#tagging-code)
    * [Object/extension pointers](#objectextension-pointers)
    * [相互リンクポインター](#objectextension-pointers)
* [Writing CSS](#writing-css)
* [CSSの書き方理論](#writing-css)
* [Building new components](#building-new-components)
* [新しいコンポーネントの作り方](#building-new-components)
* [OOCSS](#oocss)
* [オブジェクト指向CSS](#oocss)
* [Layout](#layout)
* [レイアウト](#layout)
* [Sizing UIs](#sizing-uis)
* [UIのサイズ指定](#sizing-uis)
  * [Font sizing](#font-sizing)
  * [フォントサイズ](#font-sizing)
* [Shorthand](#shorthand)
* [省略記法](#shorthand)
* [IDs](#ids)
* [IDについて](#ids)
* [Selectors](#selectors)
* [セレクター](#selectors)
  * [Over qualified selectors](#over-qualified-selectors)
  * [過剰限定セレクター](#over-qualified-selectors)
  * [Selector performance](#selector-performance)
  * [セレクターのパフォーマンス](#selector-performance)
* [CSS selector intent](#css-selector-intent)
* [CSSセレクターの意図](#css-selector-intent)
* [`!important`](#important)
* [`!important`](#important)
* [Magic numbers and absolutes](#magic-numbers-and-absolutes)
* [マジックナンバーと絶対値](#magic-numbers-and-absolutes)
* [Conditional stylesheets](#conditional-stylesheets)
* [条件付きスタイルシート](#conditional-stylesheets)
* [Debugging](#debugging)
* [デバッグ](#debugging)
* [Preprocessors](#preprocessors)
* [プリプロセッサー](#preprocessors)

---

## CSS Document Anatomy
## CSSファイルの構成

No matter the document, we must always try and keep a common formatting. This
means consistent commenting, consistent syntax and consistent naming.

たとえ文書であっても、私達はいつも共通のフォーマットに合わせる努力を行い、
それを継続し続けるべきです。これは、コメントの一貫性や文法の一貫性、
そして名前付けについて一貫性があることを意味します。

### General
### 一般

Limit your stylesheets to a maximum 80 character width where possible.
Exceptions may be gradient syntax and URLs in comments. That’s fine, there’s
nothing we can do about that.

スタイルシートのファイルについて、できるだけ横幅80文字以内になるように制限しましょう。
例外としては、グラデーションを指定する文法や、コメント内のURLなどがありますが、これはどうしようも無いのでこのままで構いません。

I prefer four (4) space indents over tabs and write multi-line CSS.

私自身は、「タブ文字」よりも「4スペース」によるインデントが好きですし、
マルチラインCSS（CSSの指定を複数行に分ける書き方）を使って書きます。

### One file vs. many files
### ひとつのファイル vs. 複数ファイル

Some people prefer to work with single, large files. This is fine, and by
sticking to the following guidelines you’ll encounter no problems. Since moving
to Sass I have started sharding my stylesheets out into lots of tiny includes.
This too is fine… Whichever method you choose, the following rules and
guidelines apply. The only notable difference is with regards our table of
contents and our section titles. Read on for further explanation…

ひとつの大きなファイルにまとめてしまう方法が好きな派閥があります。これから見ていくガイドラインに従う限り、特に問題有りません。
私はSassに移行したため、スタイルシートファイルを小さく分割してインクルードするようになりました。
こちらの方法でも良いです。
どちらを選んだとしても、これから説明するルールやガイドラインは適用できます。
ひとつ大きな違いとしては、目次やセクションタイトルの付け方です。下の説明を読んでみてください。

### Table of contents
### 目次

At the top of stylesheets, I maintain a table of contents which will detail the
sections contained in the document, for example:

    /*------------------------------------*\
        $CONTENTS
    \*------------------------------------*/
    /**
     * CONTENTS............You’re reading it!
     * RESET...............Set our reset defaults
     * FONT-FACE...........Import brand font files
     */

スタイルシートの先頭にて、私はスタイルシートの内容の目次を書くようにしています。
例えば以下のようです。

    /*------------------------------------*\
        $CONTENTS
    \*------------------------------------*/
    /**
     * CONTENTS............今ここ
     * RESET...............デフォルトの設定
     * FONT-FACE...........ブランドフォントの設定
     */

This will tell the next developer(s) exactly what they can expect to find in
this file. Each item in the table of contents maps directly to a section title.

目次があれば、このファイルの中に期待しているものがあるかどうかが、別の開発者にも正確に分かります。
目次の各項目は、各セクションのタイトルに対応させます。

If you are working in one big stylesheet, the corresponding section will also be
in that file. If you are working across multiple files then each item in the
table of contents will map to an include which pulls that section in.

もし、ひとつの巨大なスタイルシートで作業を行なっているなら、目次に書いてあるセクションは同じファイルの中にあるでしょう。
もし、あなたが複数のCSSファイルで作業をしているなら、目次の各項目はインクルードする先のファイルの中にあるでしょう。

### Section titles
### セクションタイトル

The table of contents would be of no use unless it had corresponding section
titles. Denote a section thus:

    /*------------------------------------*\
        $RESET
    \*------------------------------------*/

目次は対応するセクションタイトルが無ければ何の役にも立ちません。
セクションは以下のように書きます。

    /*------------------------------------*\
        $RESET
    \*------------------------------------*/

The `$` prefixing the name of the section allows us to run a find ([Cmd|Ctrl]+F)
for `$[SECTION-NAME]` and **limit our search scope to section titles only**.

`$`記号でセクション名を始めることで、`$[セクション名]`のように検索してセクションタイトルだけを検索対象にすることができます。

If you are working in one large stylesheet, you leave five (5) carriage returns
between each section, thus:

    /*------------------------------------*\
        $RESET
    \*------------------------------------*/
    [Our
    reset
    styles]





    /*------------------------------------*\
        $FONT-FACE
    \*------------------------------------*/

もし、ひとつのスタイルシートで作業している場合は、セクションの間に空行を５つ入れておきます。
以下例です。

    /*------------------------------------*\
        $RESET
    \*------------------------------------*/
    [ここに
    リセット
    スタイルを書く]





    /*------------------------------------*\
        $FONT-FACE
    \*------------------------------------*/

This large chunk of whitespace is quickly noticeable when scrolling quickly
through larger files.

この空行の固まりによって、巨大なファイルを素早くスクロールしていても、すぐにセクションの切れ目に気がつくことができます。

If you are working across multiple, included stylesheets, start each of those
files with a section title and there is no need for any carriage returns.

もし、あなたが複数のスタイルシートで作業している場合は、各ファイルの先頭にセクションタイトルを書くだけで良いです。空行は入れる必要はありません。

## Source order
## ソースファイルの順番

Try and write stylesheets in specificity order. This ensures that you take full
advantage of inheritance and CSS’ first <i>C</i>; the cascade.

スタイルシートを決まった順番で書くようにしましょう。正しい順番で書くことで、継承とカスケードの利点を十分に活かすことができます。ちなみに、「カスケード」はCSSの最初のCを表します。

A well ordered stylesheet will be ordered something like this:

1. **Reset** – ground zero.
2. **Elements** – unclassed `h1`, unclassed `ul` etc.
3. **Objects and abstractions** — generic, underlying design patterns.
4. **Components** – full components constructed from objects and their
   extensions.
5. **Style trumps** – error states etc.

大抵の場合、うまく構成されたスタイルシートの順番は以下のようになっています。

1. **リセット** – 基準地点.
2. **エレメント（要素）** – クラス名のない `h1` や `ul` など.
3. **オブジェクト、抽象化** — 汎用的、全体的なデザインのパターン.
4. **コンポーネント** – オブジェクトで構成された部品（コンポーネント）やその拡張など.
5. **臨時スタイル** – エラー状態など.

This means that—as you go down the document—each section builds upon and
inherits sensibly from the previous one(s). There should be less undoing of
styles, less specificity problems and all-round better architected stylesheets.

これらの詳細は続きで説明します。
ここで重要な点は、各セクションは前に定義された内容を基準にしたり、継承して構築されるということです。
スタイルの破滅をできるだけ避け、低特異性の問題（クラス名の適用範囲が狭くて再利用性に問題がある）も回避して、いろいろな問題に対応できるようなスタイルシートを設計しましょう。

For further reading I cannot recommend Jonathan Snook’s
[SMACSS](http://smacss.com) highly enough.

もっと読みたい人には、Jonathan Snook氏の [SMACSS](http://smacss.com) を超お勧めします。

## Anatomy of rulesets
## ルールセット構成

    [selector]{
        [property]:[value];
        [<- Declaration ->]
    }

I have a number of standards when structuring rulesets.

私はルールセットを組み立てるとき、いくつかの標準を使っています。

* Use hyphen delimited class names (except for BEM notation,
  [see below](#naming-conventions))
* 4 space indented
* Multi-line
* Declarations in relevance (NOT alphabetical) order
* Indent vendor prefixed declarations so that their values are aligned
* Indent our rulesets to mirror the DOM
* Always include the final semi-colon in a ruleset

* クラス名の単語はハイフンで区切る（ただし、BEM記法についてはこの限りではない。[※後述](#naming-conventions)）
* 4スペースによるインデント
* マルチライン
* 適切な宣言順序（アルファベット順ではない）
* ベンダープレフィクスで揃うようにインデント
* DOMの構成と一致するようにルールセットをインデント
* セミコロンを必ず付ける

A brief example:

以下例です。

    .widget{
        padding:10px;
        border:1px solid #BADA55;
        background-color:#C0FFEE;
        -webkit-border-radius:4px;
           -moz-border-radius:4px;
                border-radius:4px;
    }
        .widget-heading{
            font-size:1.5rem;
            line-height:1;
            font-weight:bold;
            color:#BADA55;
            margin-right:-10px;
            margin-left: -10px;
            padding:0.25em;
        }

Here we can see that `.widget-heading` must be a child of `.widget` as we have
indented the `.widget-heading` ruleset one level deeper than `.widget`. This is
useful information to developers that can now be gleaned just by a glance at the
indentation of our rulesets.

ここで、 `.widget-heading` が `.widget` の子要素であることを気が付かせるために、`.widget-heading` のルールセットを `.widget` よりも深くインデントしています。
これは、開発者にルールセットのインデントをちらっとみるだけで、ルールセット間の関係を分かってもらうのに便利です。

We can also see that `.widget-heading`’s declarations are ordered by their
relevance; `.widget-heading` must be a textual element so we begin with our
text rules, followed by everything else.

`.widget-heading`の各CSS宣言がある順番で並んでいることがわかります。
この場合、`.widget-heading`はテキストがメインの要素なので、まずテキストを
修飾する内容から始めて、続いてその他の要素を並べています。

One exception to our multi-line rule might be in cases of the following:

マルチラインのルールの例外は、次のようなものです。

    .t10    { width:10% }
    .t20    { width:20% }
    .t25    { width:25% }       /* 1/4 */
    .t30    { width:30% }
    .t33    { width:33.333% }   /* 1/3 */
    .t40    { width:40% }
    .t50    { width:50% }       /* 1/2 */
    .t60    { width:60% }
    .t66    { width:66.666% }   /* 2/3 */
    .t70    { width:70% }
    .t75    { width:75% }       /* 3/4*/
    .t80    { width:80% }
    .t90    { width:90% }

In this example (from [inuit.css’s table grid system](https://github.com/csswizardry/inuit.css/blob/master/inuit.css/partials/base/_tables.scss#L88))
it makes more sense to single-line our CSS.

この例の場合([inuit.css’s table grid system](https://github.com/csswizardry/inuit.css/blob/master/inuit.css/partials/base/_tables.scss#L88) から引用)、このように一行にまとめてしまう方がより分かりやすくなります。

## Naming conventions
## 名前付け方法

For the most part I simply use hyphen delimited classes (e.g. `.foo-bar`, not
`.foo_bar` or `.fooBar`), however in certain circumstances I use BEM (Block,
Element, Modifier) notation.

ほとんどの場合、私はクラス名の単語はハイフンで区切っています。（例：`.foo-bar`、悪い例：`.foo_bar`, `.fooBar`）
しかしながら大体の場合において、私はBEM記法を使っています。（BEMは、Block、Element、Modifierの略です）

<abbr title="Block, Element, Modifier">BEM</abbr> is a methodology for naming
and classifying CSS selectors in a way to make them a lot more strict,
transparent and informative.

<abbr title="Block, Element, Modifier">BEM</abbr>は一種の命名法です。この方法を使うことで、CSSセレクターをより厳密に、見通しよく、また意味のあるものにします。

The naming convention follows this pattern:

この命名法のパターンは以下のようです。

    .block{}
    .block__element{}
    .block--modifier{}

* `.block` represents the higher level of an abstraction or component.
* `.block__element` represents a descendent of `.block` that helps form `.block`
  as a whole.
* `.block--modifier` represents a different state or version of `.block`.

* `.block` は上位レベルの抽象名やコンポーネント名を表します。
* `.block__element` は `.block` の要素を全体的に補助するような子要素を表します。
* `.block--modifier` は `.block` 要素自体のある状態や別バージョンを表します。

An **analogy** of how BEM classes work might be:

BEMクラス名をどのように使うか、ひとつの**参考例**を示します。

    .person{}
    .person--woman{}
        .person__hand{}
        .person__hand--left{}
        .person__hand--right{}

Here we can see that the basic object we’re describing is a person, and that a
different type of person might be a woman. We can also see that people have
hands; these are sub-parts of people, and there are different variations,
like left and right.

この例ではまず、person（人）を表す基本的なオブジェクト（抽象的な概念）を定義しています。
このpersonにはwoman（女性）という別のタイプがあります。
さらに、personはhand（手）要素を子要素として持っていて、さらにhand要素にはleft（左）とright（右）という亜種があることが分かります。

We can now namespace our selectors based on their base objects and we can also
communicate what job the selector does; is it a sub-component (`__`) or a
variation (`--`)?

この命名方法によって、セレクターは基本的なオブジェクトごとに名前空間を分けることができます。
さらに、子要素なのか(`__`)、亜種なのか(`--`)を使い分けることによって、このセレクターがどんな役割を持っているのかについても知ることができます。

So, `.page-wrapper` is a standalone selector; it doesn’t form part of an
abstraction or a component and as such it named correctly. `.widget-heading`,
however, _is_ related to a component; it is a child of the `.widget` construct
so we would rename this class `.widget__heading`.

`.page-wrapper` という独立したセレクター名について考えてみましょう。
このセレクターの名前は抽象要素やコンポーネントの一部ではなく、page wrapper（ページ囲み）という名前を綴ったものですので、これは正しく命名されているといえます。
それでは `.widget-heading` はどうでしょうか。これは何らかのコンポーネントと関係している（`.widget`の子要素だと思われます）ので、この場合は `.widget__heading` と改名するのが適切でしょう。

BEM looks a little uglier, and is a lot more verbose, but it grants us a lot of
power in that we can glean the functions and relationships of elements from
their classes alone. Also, BEM syntax will typically compress (gzip) very well
as compression favours/works well with repetition.

BEM法はちょっと見た目が悪くて冗長ではありますが、クラス名だけを見て要素間の関係や機能を知ることが出来るため、大変心強いものです。また、BEM記法は繰り返し同じ単語が頻出しますので、gzipなどの典型的な圧縮とも相性が良いです。

Regardless of whether you need to use BEM or not, always ensure classes are
sensibly named; keep them as short as possible but as long as necessary. Ensure
any objects or abstractions are very vaguely named (e.g. `.ui-list`, `.media`)
to allow for greater reuse. Extensions of objects should be much more explicitly
named (e.g. `.user-avatar-link`). Don’t worry about the amount or length of
classes; gzip will compress well written code _incredibly_ well.

あなたがBEM法を使うかどうかにかかわらず、クラス名はいつでもよく考えて命名するべきです。
具体的には以下のようです。まず、短すぎず、長すぎず、必要最小限の長さにしてください。
つぎに、オブジェクトや抽象名は漠然とさせておいて（例えば `.ui-list` とか `.media` とか）、
なるべく広範囲で再利用できるようにしておき、具体的にオブジェクトを拡張する時に、ハッキリした名前付け（例えば `.user-avatar-link` など）にします。
クラス名の数やそれによるファイルサイズ増加については気にする必要はありません。gzip圧縮が信じられないくらい小さく圧縮してくれます。

### Classes in HTML
### HTML内のクラス名

In a bid to make things easier to read, separate classes in your HTML with two
(2) spaces, thus:

ちょっとしたことですが、読みやすくするために、HTML内のクラス名は2スペースあけておきます。
このようです。

    <div class="foo--bar  bar__baz">

This increased whitespace should hopefully allow for easier spotting and reading
of multiple classes.

このようにスペースを増やすことで、複数のクラス名を認識しやすくなったり、読みやすくなることが期待できます。

### JS hooks
### JS連携

**Never use a CSS _styling_ class as a JavaScript hook.** Attaching JS behaviour
to a styling class means that we can never have one without the other.

**CSSスタイルで使っているクラス名を、JavaScriptから使うのは絶対にやめましょう!!**
JSによる振る舞いの実装にスタイルのクラス名を使い始めると、もう今後そういうこと無しに
JSの実装が行えなくなって大変なことになります。

If you need to bind to some markup use a JS specific CSS class. This is simply a
class namespaced with `.js-`, e.g. `.js-toggle`, `.js-drag-and-drop`. This means
that we can attach both JS and CSS to classes in our markup but there will never
be any troublesome overlap.

もしJSで使いたいクラス名をマークアップする必要があるなら、JS用と分かるCSSクラス名を使いましょう。
単純に `.js-` のようなものを付ける方法があります。例えば `.js-toggle` とか `.js-drag-and-drop` です。
この場合、JS用とCSS用のクラス名を同時にマークアップしてかまいません。重複して面倒なことになることはありません。

    <th class="is-sortable  js-is-sortable">
    </th>

The above markup holds two classes; one to which you can attach some styling for
sortable table columns and another which allows you to add the sorting
functionality.

上のマークアップ例では、2つのクラスを併記しています。ひとつはソート可能なテーブルのカラムに設定するスタイル名で、もうひとつはソート機能実装用のクラス名です。

### Internationalisation
### 国際化

Despite being a British developer—and spending all my life writing <i>colour</i>
instead of <i>color</i>—I feel that, for the sake of consistency, it is better
to always use US-English in CSS. CSS, as with most (if not all) other languages,
is written in US-English, so to mix syntax like `color:red;` with classes like
`.colour-picker{}` lacks consistency. I have previously suggested and advocated
writing bilingual classes, for example:

イギリス人開発者（colorではなくcolourと綴ったりすることに時間を費やすような人）であるかわりに、私は一貫性を重視してアメリカ英語（US-English）でCSSを書く方が良いと思っています。
他の言語のコードと同様に、CSSを `color:red;` のようにアメリカ英語で書いておきながら、 `.colour-picker{}` のような（イギリス英語）クラス名を混ぜてしまうのは一貫性がありません。
以前、以下のように、バイリンガルなクラス名の書き方をアドバイスしたり提唱したことがありました。

    .color-picker,
    .colour-picker{
    }

However, having recently worked on a very large Sass project where there were
dozens of colour variables (e.g. `$brand-color`, `$highlight-color` etc.),
maintaining two versions of each variable soon became tiresome. It also means
twice as much work with things like find and replace.

しかし最近になって私は、とても大きなSassを使ったプロジェクトで、大量の「colour」を使った
変数（例えば `$brand-color` とか `$highlight-color` など）をメンテナンスするうちに、
これはとても大変なことに気が付きました。それぞれ2回検索したり、置換することになるからです。

In the interests of consistency, always name classes and variables in the locale
of the language you are working with.

一貫性が重要である場合、いつでもクラス名前や変数は、お使いのロケールに合わせて書くようにしましょう。

## Comments
## コメント

I use a docBlock-esque commenting style which I limit to 80 characters in length:

    /**
     * This is a docBlock style comment
     *
     * This is a longer description of the comment, describing the code in more
     * detail. We limit these lines to a maximum of 80 characters in length.
     *
     * We can have markup in the comments, and are encouraged to do so:
     *
       <div class=foo>
           <p>Lorem</p>
       </div>
     *
     * We do not prefix lines of code with an asterisk as to do so would inhibit
     * copy and paste.
     */

私は、docBlock風のコメントスタイルを、横幅80文字に制限して使っています。
以下のようです。

    /**
     * これはdocBlock風コメントスタイルです
     *
     * ここはとてもながいコメントの記述で、詳細な内容が書いてあります。
     * 長いですが、横幅を80文字に制限して折り返しています。
     *
     * コメントの中でマークアップを書く場合は、以下のように書きます：
     *
       <div class=foo>
           <p>Lorem</p>
       </div>
     *
     * コードを書くような場合は、コピペの邪魔にならないように、アスタリ
     * スクで行を始めないようにしましょう。
     */

You should document and comment our code as much as you possibly can, what may
seem or feel transparent and self explanatory to you may not be to another dev.
Write a chunk of code then write about it.

皆さん、コメントやドキュメントはできるだけたくさん書きましょう。
あなたにとっては、コメントがなくてもコードの見通しが良く、十分な説明が出来ていると思うようなものであっても、他の開発者にとってはそうでもないかもしれません。
ひとかたまりのコードを書いたなら、都度コメントも書きましょう。

### Comments on steroids
### コメントでCSS強化

There are a number of more advanced techniques you can employ with regards
comments, namely:

ここでは、以下の3つの、コメントを使ったいくつかの先進的な技術を紹介します。

* Quasi-qualified selectors
* Tagging code
* Object/extension pointers

* 準限定セレクター
* コードへのタグ付け
* 相互リンクポインター

#### Quasi-qualified selectors
#### 準限定セレクター

You should never qualify your selectors; that is to say, we should never write
`ul.nav{}` if you can just have `.nav`. Qualifying selectors decreases selector
performance, inhibits the potential for reusing a class on a different type of
element and it increases the selector’s specificity. These are all things that
should be avoided at all costs.

セレクターを限定すべきではありません。つまり、`ul.nav{}`のように書くべきではなくて、
単純に `.nav` と書きます。セレクターを限定することは、セレクターのパフォーマンスを低下させます。
また、限定された要素以外に適用できなくなるため、クラス名の再利用性が低下します。
全くいいことはありませんので、とにかく避けるべきです。

However, sometimes it is useful to communicate to the next developer(s) where
you intend a class to be used. Let’s take `.product-page` for example; this
class sounds as though it would be used on a high-level container, perhaps the
`html` or `body` element, but with `.product-page` alone it is impossible to
tell.

しかしながら、すぐとなりの開発者にクラスの意図を伝えるときに、たまに便利な時があります。
例えば `.product-page` というクラス名について考えましょう。このクラス名は上位に来る
入れ物のような響きがしますので、もしかしたら `html` や `body` 要素に使うかもしれません。
でも、 `.product-page` というクラス名だけではどの要素に使うのかわかりません。

By quasi-qualifying this selector (i.e. commenting out the leading type
selector) we can communicate where we wish to have this class applied, thus:

そこで、コメントを使って限定セレクターっぽく見せることで、このクラスをどの要素に使って欲しいのかを
別の開発者に教えることができます。以下のようです。

    /*html*/.product-page{}

We can now see exactly where to apply this class but with none of the
specificity or non-reusability drawbacks.

これで、このクラスをどの要素に使ったらいいかを正確に知ることができます。
しかも、先程説明した詳細化や再利用性の問題もありません。

Other examples might be:

他の例をあげてみましょう。

    /*ol*/.breadcrumb{}
    /*p*/.intro{}
    /*ul*/.image-thumbs{}

Here we can see where we intend each of these classes to be applied without
actually ever impacting the specificity of the selectors.

このように、これらのクラスをどこに使うつもりなのかを、セレクターの動作を変えずに知らせることができます。

#### Tagging code
#### コードへのタグ付け

If you write a new component then leave some tags pertaining to its function in
a comment above it, for example:

もし、あなたが新しいコンポーネントを書いたなら、いくつかのタグをコメントに書いておいて、
そのコンポーネントが持つ「機能」について関係付け行なっておきましょう。例えば以下のようです。

    /**
     * ^navigation ^lists
     */
    .nav{}

    /**
     * ^grids ^lists ^tables
     */
    .matrix{}

These tags allow other developers to find snippets of code by searching for
function; if a developer needs to work with lists they can run a find for
`^lists` and find the `.nav` and `.matrix` objects (and possibly more).

これらのタグによって、他の開発者が「機能」で検索した時に、目的のコードへ辿り着くことができます。
つまり、ある開発者がリストする機能について何か探していて、 `^lists` という単語を検索すると、
`.nav` や `.matrix` というクラス名を（もっとたくさん見つかるかもしれませんが）
見つけることが出来るというわけです。

#### Object/extension pointers
#### 相互リンクポインター

When working in an object oriented manner you will often have two chunks of CSS
(one being the skeleton (the object) and the other being the skin (the
extension)) that are very closely related, but that live in very different
places. In order to establish a concrete link between the object and its
extension with use <i>object/extension pointers</i>. These are simply comments
which work thus:

オブジェクト指向な方法で開発を行なっていると、骨組みとなる基本的な枠組みのCSSと、
それに対するスキンのような拡張部分を分けて書くことが多いと思います。
その場合、基本部分と拡張部分は密接に関係しているのですが、ファイル上では離れた場所に
書かざるを得ないことがよくあります。
この基本部分と拡張部分に、それとよく分かるリンクを張るために、<i>相互リンクポインター</i>を
使うと良いです。これを行うためには、以下のようにコメント文を書きます。

In your base stylesheet:

    /**
     * Extend `.foo` in theme.css
     */
     .foo{}

まず、基本部分でこう書きます。

    /**
     * `.foo` を theme.css で継承すること
     */
     .foo{}

In your theme stylesheet:

    /**
     * Extends `.foo` in base.css
     */
     .bar{}

次に、テーマのような拡張部分でこう書きます。

    /**
     * base.css の `.foo` を継承している
     */
     .bar{}

Here we have established a concrete relationship between two very separate
pieces of code.

これで、離れた地点のコードに強固な関係が築けました。

---

## Writing CSS
## CSSの書き方理論

The previous section dealt with how we structure and form our CSS; they were
very quantifiable rules. The next section is a little more theoretical and deals
with our attitude and approach.

これまでのセクションでは、どのようにCSSを構成して記述するかという事について扱いました。
つまり、とても形式的な内容でした。このセクションでは、もうすこし理論的な考え方や
取り組み方について説明します。

## Building new components
## 新しいコンポーネントの作り方

When building a new component write markup **before** CSS. This means you can
visually see which CSS properties are naturally inherited and thus avoid
reapplying redundant styles.

新しくコンポーネント（部品）を作る場合は、まずCSSよりもマークアップの方から書くようにしましょう。
この方法により、どのCSS属性が継承されていて何を書き加えるべきかを視覚的に確認でき、その結果冗長なCSS属性の定義を避けることができます。

By writing markup first you can focus on data, content and semantics and then
apply only the relevant classes and CSS _afterwards_.

マークアップを先に書くことで、表示されるべきデータや内容、またそれらの意味について集中することができます。
そうすれば、そのあとで適切なクラス名やCSSについて考えることができます。

## OOCSS
## オブジェクト指向CSS

I work in an OOCSS manner; I split components into structure (objects) and
skin (extensions). As an **analogy** (note, not example) take the following:

私はオブジェクト指向CSS方式で作業しています。つまり、コンポーネントの枠組みである基本部分と、
拡張部分であるスキンを分けて書いています。現実の例ではないですが、このような雰囲気です。

    .room{}

    .room--kitchen{}
    .room--bedroom{}
    .room--bathroom{}

We have several types of room in a house, but they all share similar traits;
they all have floors, ceilings, walls and doors. We can share this information
in an abstracted `.room{}` class. However we have specific types of room that
are different from the others; a kitchen might have a tiled floor and a bedroom
might have carpets, a bathroom might not have a window but a bedroom most likely
will, each room likely has different coloured walls. OOCSS teaches us to
abstract the shared styles out into a base object and then _extend_ this
information with more specific classes to add the unique treatment(s).

家にいくつかのタイプの部屋があるとします。それらの部屋は似たような特徴を持ってます。
すなわち、どの部屋にも、床と天井と壁とドアがあります。
そこで、この共通な特徴の情報を、抽象化した `.room{}` クラスで共有することにします。
しかしながら、それぞれの部屋の詳細については他のものと違います。
キッチンはタイル張りの床で、ベッドルームはカーペットかもしれませんし、
バスルームには窓がなくてもベッドルームにはおそらく窓はあるでしょう。
また、それぞれの部屋の壁の色は多分ちがうでしょう。
オブジェクト指向CSSによると、基本オブジェクトで共通のスタイルを定義し、
具体的なクラスで基本オブジェクトを _拡張_ して、ユニークな特性を定義することになっています。

So, instead of building dozens of unique components, try and spot repeated
design patterns across them all and abstract them out into reusable classes;
build these skeletons as base ‘objects’ and then peg classes onto these to
extend their styling for more unique circumstances.

そこで、大量のコンポーネントをバラバラに作る代わりに、コンポーネント間で繰り返し現れるデザインのパターンを
見つけ出して、再利用可能なクラスとして抽象化してみましょう。
つまり、骨組みとなる基本「オブジェクト」を作り、その基本オブジェクトに抽象化したクラスを定義します。
そして、それらのクラスを拡張して、よりユニークな詳細をスタイルとして定義するのです。

If you have to build a new component split it into structure and skin; build the
structure of the component using very generic classes so that we can reuse that
construct and then use more specific classes to skin it up and add design
treatments.

もし新しいコンポーネントを作る場面になったら、そのコンポーネントを骨組みとスキンに分けてみましょう。
コンポーネントの骨組みは、構築時に再利用するためにとても汎用的なクラスを使います。
そして、より具体的なクラスでスキンを定義してデザイン要素を埋め込んでいきます。

## Layout
## レイアウト

All components you build should be left totally free of widths; they should
always remain fluid and their widths should be governed by a parent/grid system.

コンポーネントを新たに作る場合は、横幅に特に制限を設けるべきではありません。
つまり、フルードレイアウト出来るようにしておいて、コンポーネントを格納する側の
親要素やグリッドシステムが横幅を決められるようにしておきましょう。

Heights should **never** be be applied to elements. Heights should only be
applied to things which had dimensions _before_ they entered the site (i.e.
images and sprites). Never ever set heights on `p`s, `ul`s, `div`s, anything.
You can often achieve the desired effect with `line-height` which is far more
flexible.

高さは**絶対に**設定すべきではありません。
高さは、（画像とかスプライトのような）サイトを _表示する前から_ サイズが決まっているものにのみ適用すべきです。
間違っても、`p` とか `ul` とか `div` とか、そういうものに高さを設定してはいけません。
高さを調整したい場合、 `line-height` を使うことでより柔軟にやりたいことを達成できるはずです。

Grid systems should be thought of as shelves. They contain content but are not
content in themselves. You put up your shelves then fill them with your stuff.
By setting up our grids separately to our components you can move components
around a lot more easily than if they had dimensions applied to them; this makes
our front-ends a lot more adaptable and quick to work with.

グリッドシステムは棚のようなものだと考えるべきです。
何らかの中身を格納するものであり、グリッドシステム自体が主役にはなりません。
グリッドシステムを設置したら、次はその中に具材を詰めていきましょう。
グリッドとコンポーネントの配置の設定をお互いに独立にさせておくことで、
サイズを固定にしてしまうよりも、コンポーネントをグリッドの中でより自由に動かすことができます。
つまり、前面部のレイアウトを柔軟にして、スムーズにレイアウトが動作するようにしましょう。

You should never apply any styles to a grid item, they are for layout purposes
only. Apply styling to content _inside_ a grid item. Never, under _any_
circumstances, apply box-model properties to a grid item.

グリッドの各要素に直接スタイルを設定すべきではありません。
先程も書きましたように、グリッドは入れ物であり、それら自身はコンテンツではありません。
スタイルはグリッドの _中の_ 要素に対して適用しましょう。
_どんな_ 状況においても、決して、ボックスモデルの属性をグリッドを構成する要素に適用してはダメです。

## Sizing UIs
## UIのサイズ指定

I use a combination of methods for sizing UIs. Percentages, pixels, ems, rems
and nothing at all.

私はUIのサイズ調整にいくつかの方法を組み合わせています。
パーセント(%)、ピクセル(pixel)、文字幅(em, rem)を使って、他は全く使いません。

Grid systems should, ideally, be set in percentages. Because I use grid systems
to govern widths of columns and pages, I can leave components totally free of
any dimensions (as discussed above).

グリッドシステムは理想的にはパーセントで設定すべきです。
グリッドシステムをカラムの幅やページレイアウトの制御に使うことで、
コンポーネントが自由にサイズを調整する余地を残すことができます。（先程議論しましたね）

Font sizes I set in rems with a pixel fallback. This gives the accessibility
benefits of ems with the confidence of pixels. Here is a handy Sass mixin to
work out a rem and pixel fallback for you (assuming you set your base font
size in a variable somewhere):

フォントサイズについてはrem単位を使い、予備としてピクセル指定を併記します。
この方法は、em単位による柔軟性を確保すると同時に、ピクセル指定でバックアップすること(ピクセルフォールバック)で信頼感が得られます。
ここで、Sassを使ってremとピクセルフォールバックを行う例をお見せします。
（この例ではベースフォントサイズ($base-font-size)がどこかで設定されているとします）

    @mixin font-size($font-size){
        font-size:$font-size +px;
        font-size:$font-size / $base-font-size +rem;
    }

I only use pixels for items whose dimensions were defined before the came into
the site. This includes things like images and sprites whose dimensions are
inherently set absolutely in pixels.

私は、予めサイズの決まっているのもの定義には、ピクセル単位しか使いません。
画像などの絶対的にピクセルでサイズが決まっているものも、ピクセル指定を使います。

### Font sizing
### フォントサイズ

I define a series of classes akin to a grid system for sizing fonts. These
classes can be used to style type in a double stranded heading hierarchy. For a
full explanation of how this works please refer to my article
[Pragmatic, practical font-sizing in CSS](http://csswizardry.com/2012/02/pragmatic-practical-font-sizing-in-css)

私はグリッドシステムにあわせて、文字のサイズに関する一連のクラスを定義します[※1]。
これらのクラスは Double stranded heading hierarchy 方式[※2]で
スタイルの適用を行なっています。この方式について詳しくは私の記事[Pragmatic, practical font-sizing in CSS](http://csswizardry.com/2012/02/pragmatic-practical-font-sizing-in-css)を参照してください。

[※1] ：ヘッダー、フッター、サイドバーなどで、それぞれ文字のサイズを調節するという事です。

[※2] ：論理的な見出し構造と物理的な見出しサイズを、複雑にせずに簡単に設定する方法です。詳細はリンク先を参照してください。

## Shorthand
## 省略記法

**Shorthand CSS needs to be used with caution.**

**CSSの省略記法は注意して使うべきです。**

It might be tempting to use declarations like `background:red;` but in doing so
what you are actually saying is ‘I want no image to scroll, aligned top-left,
repeating X and Y, and a background colour of red’. Nine times out of ten this
won’t cause any issues but that one time it does is annoying enough to warrant
not using such shorthand. Instead use `background-color:red;`.

CSS属性の定義にて、 `background:red;` のように短く書いてしまう誘惑に駆られますが、
そのように書いてしまうと「私は背景として、画像なしだけど本文と同期スクロールで、
左上配置のX-Y方向のリピートで、背景色は赤でよろしく」と指定したことになります。
10回のうち9回は、それで特に問題はないかもしれませんが、残りの1回で大抵困ったことになり、
もう省略記法なんて使うもんかと腹が立つ事態になるはずです。
きちんと `background-color:red;` と書くようにしましょう。

Similarly, declarations like `margin:0;` are nice and short, but
**be explicit**. If you actually only really want to affect the margin on
the bottom of an element then it is more appropriate to use `margin-bottom:0;`.

同様に、 `margin:0;` は短くて便利ですが、もっと**明示的に書くべきです**。
実は要素の下の余白だけに設定したかったのであれば、 `margin-bottom:0;` と書くほうがより適切です。

Be explicit in which properties you set and take care to not inadvertently unset
others with shorthand. E.g. if you only want to remove the bottom margin on an
element then there is no sense in setting all margins to zero with `margin:0;`.

CSSの属性は明示的に書くようにして、省略記法によって意図せず属性が設定されないように気を付けるべきです。
つまり、要素の下側の余白だけ無くしたいのに、全ての四隅の余白を `margin:0;` で
無くしてしまうなんてことはナンセンスです。

Shorthand is good, but easily misused.

省略記法はいいものですが、簡単に誤用してしまうものです。

## IDs
## IDについて

A quick note on IDs in CSS before we dive into selectors in general.

まず、セレクターの一般論に入る前に、CSSのIDについてひとつだけ注意したいことがあります。

**NEVER use IDs in CSS.**

**絶対にCSSでIDセレクターは使ってはダメです！**

They can be used in your markup for JS and fragment identifiers but use only
classes for styling. You don’t want to see a single ID in any stylesheets!

JSとの連携のためのマークアップや、要素の特定のためにIDを使うことはできますが、
スタイルの設定にはクラスを使ってください。
みなさんも、一箇所にしか適用しないようなものを、スタイルシートの中で見たくないですよね！

Classes come with the benefit of being reusable (even if we don’t want to, we
can) and they have a nice, low specificity. Specificity is one of the quickest
ways to run into difficulties in projects and keeping it low at all times is
imperative. An ID is **255** times more specific than a class, so never ever use
them in CSS _ever_.

クラスには再利用性の利点が常にあり（私たちが望む望まないにかかわらず）、
クラスには優れた低特異性（訳注：いろんな所に適用できる性質）があります。
低特異性はプロジェクトの困難に立ち向かう手っ取り早い方法のひとつであり、
特異性を低く保つことは絶対の義務です。
ひとつのIDは、ひとつのクラスよりも**255**倍も狭い範囲しか特定できないので、
今後とも _永遠に_ CSSの中で使わないようにしてください。

## Selectors
## セレクター

Keep selectors short, efficient and portable.

セレクターは短く、効率的で、可搬性があるように気を使ってください。

Heavily location-based selectors are bad for a number of reasons. For example,
take `.sidebar h3 span{}`. This selector is too location-based and thus we
cannot move that `span` outside of a `h3` outside of `.sidebar` and maintain
styling.

重たい子孫セレクター（location-based selectors）はいくつかの理由から避けるべきものです。
例えば、 `.sidebar h3 span{}` というセレクターについて考えてみましょう。
このセレクターは子孫の指定が多いので、スタイルを維持したまま `span` 要素のスタイルを `.sidebar` の `h3` の中以外に移動することができません。

Selectors which are too long also introduce performance issues; the more checks
in a selector (e.g. `.sidebar h3 span` has three checks, `.content ul p a` has
four), the more work the browser has to do.

長すぎるセレクターは、パフォーマンスの問題も引き起こします。
つまり、セレクターのチェックの数が多くなります（例えば、`.sidebar h3 span` だと3回、
`.content ul p a` だと4回のチェックが発生します）ので、ブラウザーに負荷がかかります。

Make sure styles aren’t dependent on location where possible, and make sure
selectors are nice and short.

スタイルはできるだけ特定の子孫要素に依存しないようにしつつ、短く良いセレクターになるように気を付けましょう。

Selectors as a whole should be kept short (e.g. one class deep) but the class
names themselves should be as long as they need to be. A class of `.user-avatar`
is far nicer than `.usr-avt`.

全体的に、セレクターは短くなるように意識するべきですが（例えばクラス階層はひとつにするなど）、
クラス名自体は必要であれば長くても構いません。 `.usr-avt` よりも `.user-avatar` の
方がはるかに良いです。

**Remember:** classes are neither semantic or insemantic; they are sensible or
insensible! Stop stressing about ‘semantic’ class names and pick something
sensible and futureproof.

**覚えておいてください：** クラス名には意味も無意味もありません。機能的かそうでないかが重要です。
クラス名にコンテンツ上の意味を押し付けるのをやめて、機能性や将来性を考えて名前を付けましょう。

### Over-qualified selectors
### 過剰限定セレクター

As discussed above, qualified selectors are bad news.

これまでの議論の通り、限定セレクターにいい話はありません。

An over-qualified selector is one like `div.promo`. You could probably get the
same effect from just using `.promo`. Of course sometimes you will _want_ to
qualify a class with an element (e.g. if you have a generic `.error` class that
needs to look different when applied to different elements (e.g.
`.error{ color:red; }` `div.error{ padding:14px; }`)), but generally avoid it
where possible.

`div.promo`のようなものは過剰に限定されたセレクターです。
`.promo`と書くだけで同じ効果が得られるでしょう。
もちろん、時々意図的に限定されたクラス名を使いたい時があるでしょうが
（つまり、`.error`という汎用的なクラスがあって、特定の要素の時にはもう少し修飾したいというような場合（こんな感じ `.error{ color:red; }` `div.error{ padding:14px; }`））、
一般的には避けるべきです。

Another example of an over-qualified selector might be `ul.nav li a{}`. As
above, we can instantly drop the `ul` and because we know `.nav` is a list, we
therefore know that any `a` _must_ be in an `li`, so we can get `ul.nav li a{}`
down to just `.nav a{}`.

別の過剰な限定セレクターの例として、`ul.nav li a{}` というものがあります。
これも純粋に `ul` を取り去ることができます。なぜなら、私たちは `.nav` は
リスト要素であることを知っていて、それゆえ `a` 要素は _必ず_ `li` の中にある
はずですので、最終的に `ul.nav li a{}` は `.nav a{}` のように書き直せます。

### Selector performance
### セレクターのパフォーマンス

Whilst it is true that browsers will only ever keep getting faster at rendering
CSS, efficiency is something you could do to keep an eye on. Short, unnested
selectors, not using the universal (`*{}`) selector as the key selector, and
avoiding more complex CSS3 selectors should help circumvent these problems.

ブラウザーが常にCSSのレンダリング速度を向上させているのは事実ですが、
効率はあなたが気を付けることで何とか出来る部分です。
短く、入れ子になっていないセレクターを使いましょう。
ユニバーサルセレクター（`*{}`）をキーセレクター（※3）として使ってはダメです。
また、CSS3の複雑なセレクターを避けることも問題回避に役立つはずです。

[※3] セレクターのうち最も右に現れるセレクターをキーセレクタと言います。

## CSS selector intent
## CSSセレクターの意図

Instead of using selectors to drill down the DOM to an element, it is often best
to put a class on the element you explicitly want to style. Let’s take a
specific example with a selector like `.header ul{}`…

DOMをたどって特定の要素を指定するようなセレクターを使う代わりに、
スタイルが必要な要素に直接クラスを書く方がしばしば最良の場合があります。
`.header ul{}` というセレクターの例を考えましょう。

Let’s imagine that `ul` is indeed the main navigation for our website. It lives
in the header as you might expect and is currently the only `ul` in there;
`.header ul{}` will work, but it’s not ideal or advisable. It’s not very future
proof and certainly not explicit enough. As soon as we add another `ul` to that
header it will adopt the styling of our main nav and the the chances are it
won’t want to. This means we either have to refactor a lot of code _or_ undo a
lot of styling on subsequent `ul`s in that `.header` to remove the effects of
the far reaching selector.

問題の `ul` 要素は、今私達が考えているWebサイトにとって、メインのナビゲーションであるとしましょう。
`ul`要素がヘッダーの中にあって、そこに`ul`要素しか無いのであれば、先ほどのセレクターでもうまく行くでしょう。
しかし、それは理想的で賢明なセレクターであるとはいえません。
将来性がなく、きっとセレクターとして要素をうまく指定できない状況が来るでしょう。
この状況で別の`ul`要素をヘッダーに加えると、意図せずナビゲーション用のスタイルが適用されてしまいます。
こうなると、大量のコードのリファクタリングを行うか、`.header`内に追加した`ul`要素にナビゲーション用の
スタイルを打ち消すようなスタイリングを行う必要が出てきます。

Your selector’s intent must match that of your reason for styling something;
ask yourself **‘am I selecting this because it’s a `ul` inside of `.header` or
because it is my site’s main nav?’**. The answer to this will determine your
selector.

セレクターの意図は、何かをスタイリングするという目的の意図と一致していなければなりません。
つまり、以下のように自問自答してみましょう：
**「私はこの要素を`.header`内の`ul`だから選択しているのだろうか？ それともWebサイトのメインナビゲーションだからだろうか？」**
この質問への答えによって、あなたの書くセレクターが自ずと決まるでしょう。

Make sure your key selector is never an element/type selector or
object/abstraction class. You never really want to see selectors like
`.sidebar ul{}` or `.footer .media{}` in our theme stylesheets.

キーセレクターは、要素のタイプを指定したり、抽象オブジェクトのクラスを指定するような
ものにならないようにしましょう。 `.sidebar ul{}` や `.footer .media{}` のようなセレクターを
テーマ用のスタイルシートで、全くもって見たくないわけです。

Be explicit; target the element you want to affect, not its parent. Never assume
that markup won’t change. **Write selectors that target what you want, not what
happens to be there already.**

とにかく明示的であるべきです。つまり、効果を及ぼしたい要素に照準を合わせるべきです。
その親要素ではありません。
また、マークアップが変わらないことを仮定するべきではありません。
**現在たまたまそうなっていることを狙うのではなく、必要な要素を狙ってセレクターを書きましょう。**

For a full write up please see my article
[Shoot to kill; CSS selector intent](http://csswizardry.com/2012/07/shoot-to-kill-css-selector-intent/)

さらにまとまっている私の記事がありますので、ぜひ参照してください。
[Shoot to kill; CSS selector intent](http://csswizardry.com/2012/07/shoot-to-kill-css-selector-intent/)

## `!important`

It is okay to use `!important` on helper classes only. To add `!important`
preemptively is fine, e.g. `.error{ color:red!important }`, as you know you will
**always** want this rule to take precedence.

`!important`をヘルパー的なクラスに使うのは良いです。
`!important`をつけたスタイルが優先されることを**常に**自覚しているのなら、
天下り的に`!important`を付けても悪くありません。例えば `.error{ color:red!important }` のような
ものです。

Using `!important` reactively, e.g. to get yourself out of nasty specificity
situations, is not advised. Rework your CSS and try to combat these issues by
refactoring your selectors. Keeping your selectors short and avoiding IDs will
help out here massively.

`!important` を試行錯誤的に使うこと（つまりCSSがこんがらがっていて、それを何とかしたい時など）は、
勧められません。そのCSSを見なおして、セレクターをリファクタリングすることで、
こんがらがった問題を解決しようとしてください。
セレクターは短くしてIDを避けるとで、この場合ほとんどの問題は解決します。

## Magic numbers and absolutes
## マジックナンバーと絶対値

A magic number is a number which is used because ‘it just works’. These are bad
because they rarely work for any real reason and are not usually very
futureproof or flexible/forgiving. They tend to fix symptoms and not problems.

マジックナンバーは「丁度それでうまくいく」という理由で使われている数字です。
なぜそれがうまく行くのか本当の理由について分かってないということと、
大抵将来性がないか柔軟性や寛容性がないため、良くないことです。
問題の解決ではなく、対処療法のようなものです。

For example, using `.dropdown-nav li:hover ul{ top:37px; }` to move a dropdown
to the bottom of the nav on hover is bad, as 37px is a magic number. 37px only
works here because in this particular scenario the `.dropdown-nav` happens to be
37px tall.

例えば、ナビゲーション要素の下にドロップダウン要素を動かすために、
`.dropdown-nav li:hover ul{ top:37px; }`という記述を使うことは、
37pxというマジックナンバーが出てくるので良くないです。
この場合、`.dropdown-nav`の要素が37pxの高さの時だけしか、ちゃんと動きません。

Instead you should use `.dropdown-nav li:hover ul{ top:100%; }` which means no
matter how tall the `.dropdown-nav` gets, the dropdown will always sit 100% from
the top.

代わりに、`.dropdown-nav li:hover ul{ top:100%; }`という記述を使えば、
`.dropdown-nav`がどんな高さであっても、ドロップダウン要素はナビゲーション要素の
上から高さ100%の位置に表示されます。

Every time you hard code a number think twice; if you can avoid it by using
keywords or ‘aliases’ (i.e. `top:100%` to mean ‘all the way from the top’)
or&mdash;even better&mdash;no measurements at all then you probably should.

数値を直接コーディングしようとする場合は、毎回2回はよく考えてください。
つまり、キーワードや別名（例えば`top:100%`というのは「上から高さ100%の位置」という別名）が
使えないかどうか、全く数値を測定しないでやる方法が無いかどうか考えてください。

Every hard-coded measurement you set is a commitment you might not necessarily
want to keep.

全てのハードコードされた数値は、必ずしも必要でなかった努力です。

## Conditional stylesheets
## 条件付きスタイルシート

IE stylesheets can, by and large, be totally avoided. The only time an IE
stylesheet may be required is to circumvent blatant lack of support (e.g. PNG
fixes).

IE用のスタイルシートは、おおむね全体的に避けることができます。
IE用のスタイルシートが必要とされるのは、露骨な機能不足を避けるためだけです（PNG透過修正とか）。

As a general rule, all layout and box-model rules can and _will_ work without an
IE stylesheet if you refactor and rework your CSS. This means you never want to
see `<!--[if IE 7]> element{ margin-left:-9px; } < ![endif]-->` or other such
CSS that is clearly using arbitrary styling to just ‘make stuff work’.

一般的に、IE用スタイルシートが無くても、全てのレイアウトルールやボックスモデルは
CSSを見なおして修正するだけで動きます。これはつまり `<!--[if IE 7]> element{ margin-left:-9px; } < ![endif]-->` というものや、単に「動くようにする」ためだけのいろいろなスタイリングを見なくて済むということを意味します。

## Debugging
## デバッグ

If you run into a CSS problem **take code away before you start adding more** in
a bid to fix it. The problem exists in CSS that is already written, more CSS
isn’t the right answer!

もしCSSで問題にぶち当たったのなら、その問題を修正しようとして**何か追加したりする前に、コードを削除しましょう**。
すでに書かれたCSSに問題があるのであれば、そのCSSに何かを追加することは正しい解決策ではありません。

Delete chunks of markup and CSS until your problem goes away, then you can
determine which part of the code the problem lies in.

問題が無くなるまでマークアップやCSSを削除することで、
どのコードに問題があるのかを特定できます。

It can be tempting to put an `overflow:hidden;` on something to hide the effects
of a layout quirk, but overflow was probably never the problem; **fix the
problem, not its symptoms.**

変なレイアウトの動きを隠すために、どこかに`overflow:hidden;`を入れてしまおうという誘惑に
駆られるかもしれませんが、大抵の場合overflowは問題ではありません。
つまり、**症状を改善するのではなくて、きちんと問題を解決しましょう。**

## Preprocessors
## プリプロセッサー

Sass is my preprocessor of choice. **Use it wisely.** Use Sass to make your CSS
more powerful but avoid nesting like the plague! Nest only when it would
actually be necessary in vanilla CSS, e.g.

私はプリプロセッサーとしてSassを使っています。
**プリプロセッサーは賢く使いましょう。**
Sassは、CSSをよりパワフルに書くために使うのであって、ネストを伝染させる（子孫セレクターを濫用する）のは避けましょう！
ネストしていいのは、プリプロセッサーを使わずにCSSを書く場合でも問題ない場合のみです。つまり、

    .header{}
    .header .site-nav{}
    .header .site-nav li{}
    .header .site-nav li a{}

Would be wholly unnecessary in normal CSS, so the following would be **bad**
Sass:

このような書き方は、これまでの議論から、通常のCSSとしては全く必要ないものですので、
Sassで以下のように書くのは**ダメ**です。

    .header{
        .site-nav{
            li{
                a{}
            }
        }
    }

If you were to Sass this up you’d write it as:

もし、Sassでこの例を書くのであれば、以下のようにします。

    .header{}
    .site-nav{
        li{}
        a{}
    }
