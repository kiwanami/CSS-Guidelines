# 一般 CSS 注釈、アドバイス、ガイドライン

---

* [Original](https://github.com/csswizardry/CSS-Guidelines/blob/master/README.md)

---

大きくて長期間に渡るプロジェクトに、多くの開発者が関わって一緒に働く場合、
他の約束事と同様に、以下のような目的のために統一された方法を用いることが重要です。

* スタイルシートをメンテナンス可能にし続ける
* コードを見通しよく読みやすいように維持する
* スタイルシートを拡張可能に保つ

これらのゴール達成のためは、いくつかの技術的手法があります。

この文書では、まず最初に文法やフォーマット、CSSの構成について述べて、
後半ではCSSを書いたり設計するにあたって、考え方や取り組み方について述べます。
どうですか？面白そうでしょう？

## Contents

* [CSSファイルの構成](#cssファイルの構成)
  * [一般](#一般)
  * [ひとつのファイル vs. 複数ファイル](#ひとつのファイル-vs-複数ファイル)
  * [目次](#目次)
  * [セクションタイトル](#セクションタイトル)
* [ソースファイルの順番](#ソースファイルの順番)
* [ルールセット構成](#ルールセット構成)
* [名前付け方法](#名前付け方法)
  * [JS連携](#js連携)
  * [国際化](#国際化)
* [コメント](#コメント)
  * [コメントでCSS強化](#コメントでcss強化)
    * [準限定セレクター](#準限定セレクター)
    * [コードへのタグ付け](#コードへのタグ付け)
    * [相互リンクポインター](#相互リンクポインター)
* [CSSの書き方理論](#cssの書き方理論)
* [新しいコンポーネントの作り方](#新しいコンポーネントの作り方)
* [オブジェクト指向CSS](#オブジェクト指向css)
* [レイアウト](#レイアウト)
* [UIのサイズ指定](#uiのサイズ指定)
  * [フォントサイズ](#フォントサイズ)
* [省略記法](#省略記法)
* [IDについて](#idについて)
* [セレクター](#セレクター)
  * [過剰限定セレクター](#過剰限定セレクター)
  * [セレクターのパフォーマンス](#セレクターのパフォーマンス)
* [CSSセレクターの意図](#cssセレクターの意図)
* [`!important`](#important)
* [マジックナンバーと絶対値](#マジックナンバーと絶対値)
* [条件付きスタイルシート](#条件付きスタイルシート)
* [デバッグ](#デバッグ)
* [プリプロセッサー](#プリプロセッサー)


---

## CSSファイルの構成

たとえ文書であっても、私達はいつも共通のフォーマットに合わせる努力を行い、
それを継続し続けるべきです。これは、コメントの一貫性や文法の一貫性、
そして名前付けについて一貫性があることを意味します。

### 一般

スタイルシートのファイルについて、できるだけ横幅80文字以内になるように制限しましょう。
例外としては、グラデーションを指定する文法や、コメント内のURLなどがありますが、これはどうしようも無いのでこのままで構いません。

私自身は、「タブ文字」よりも「4スペース」によるインデントが好きですし、
マルチラインCSS（CSSの指定を複数行に分ける書き方）を使って書きます。

### ひとつのファイル vs. 複数ファイル

ひとつの大きなファイルにまとめてしまう方法が好きな派閥があります。これから見ていくガイドラインに従う限り、特に問題有りません。
私はSassに移行したため、スタイルシートファイルを小さく分割してインクルードするようになりました。
こちらの方法でも良いです。
どちらを選んだとしても、これから説明するルールやガイドラインは適用できます。
ひとつ大きな違いとしては、目次やセクションタイトルの付け方です。下の説明を読んでみてください。

### 目次

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

目次があれば、このファイルの中に期待しているものがあるかどうかが、別の開発者にも正確に分かります。
目次の各項目は、各セクションのタイトルに対応させます。

もし、ひとつの巨大なスタイルシートで作業を行なっているなら、目次に書いてあるセクションは同じファイルの中にあるでしょう。
もし、あなたが複数のCSSファイルで作業をしているなら、目次の各項目はインクルードする先のファイルの中にあるでしょう。

### セクションタイトル

目次は対応するセクションタイトルが無ければ何の役にも立ちません。
セクションは以下のように書きます。

    /*------------------------------------*\
        $RESET
    \*------------------------------------*/

`$`記号でセクション名を始めることで、`$[セクション名]`のように検索してセクションタイトルだけを検索対象にすることができます。

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

この空行の固まりによって、巨大なファイルを素早くスクロールしていても、すぐにセクションの切れ目に気がつくことができます。

もし、あなたが複数のスタイルシートで作業している場合は、各ファイルの先頭にセクションタイトルを書くだけで良いです。空行は入れる必要はありません。

## ソースファイルの順番

スタイルシートを詳細度の低い順に書くようにしましょう。正しい順番で書くことで、継承とカスケードの利点を十分に活かすことができます。ちなみに、「カスケード」はCSSの最初の *C* を表します。

大抵の場合、うまく構成されたスタイルシートの順番は以下のようになっています。

1. **リセット** – 基準地点.
2. **エレメント（要素）** – クラス名のない `h1` や `ul` など.
3. **オブジェクト、抽象化** — 汎用的、全体的なデザインのパターン.
4. **コンポーネント** – オブジェクトで構成された部品（コンポーネント）やその拡張など.
5. **臨時スタイル** – エラー状態など.

これらの詳細は続きで説明します。
ここで重要な点は、各セクションは前に定義された内容を基準にしたり、継承して構築されるということです。
スタイルの破滅をできるだけ避け、クラスの再利用性を高めて、いろいろな問題に対応できるようなスタイルシートを設計しましょう。

もっと読みたい人には、Jonathan Snook氏の [SMACSS](http://smacss.com) を超お勧めします。

## ルールセット構成

    [selector]{
        [property]:[value];
        [<- Declaration ->]
    }

私はルールセットを組み立てるとき、いくつかの標準を使っています。

* クラス名の単語はハイフンで区切る（ただし、BEM記法についてはこの限りではない。[※後述](#naming-conventions)）
* 4スペースによるインデント
* マルチライン
* 適切な宣言順序（アルファベット順ではない）
* ベンダープレフィクスで揃うようにインデント
* DOMの構成と一致するようにルールセットをインデント
* セミコロンを必ず付ける

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

ここで、 `.widget-heading` が `.widget` の子要素であることを気が付かせるために、`.widget-heading` のルールセットを `.widget` よりも深くインデントしています。
これは、開発者にルールセットのインデントをちらっとみるだけで、ルールセット間の関係を分かってもらうのに便利です。

`.widget-heading`の各CSS宣言がある順番で並んでいることがわかります。
この場合、`.widget-heading`はテキストがメインの要素なので、まずテキストを
修飾する内容から始めて、続いてその他の要素を並べています。

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

この例の場合([inuit.css’s table grid system](https://github.com/csswizardry/inuit.css/blob/master/base/_tables.scss#L96) から引用)、このように一行にまとめてしまう方がより分かりやすくなります。

## 名前付け方法

ほとんどの場合、私はクラス名の単語はハイフンで区切っています。（例：`.foo-bar`、悪い例：`.foo_bar`, `.fooBar`）
しかしながら大体の場合において、私はBEM記法を使っています。（BEMは、Block、Element、Modifierの略です）

<abbr title="Block, Element, Modifier">BEM</abbr>は一種の命名法です。この方法を使うことで、CSSセレクターをより厳密に、見通しよく、また意味のあるものにします。

この命名法のパターンは以下のようです。

    .block{}
    .block__element{}
    .block--modifier{}

* `.block` は上位レベルの抽象名やコンポーネント名を表します。
* `.block__element` は `.block` の要素を全体的に補助するような子要素を表します。
* `.block--modifier` は `.block` 要素自体のある状態や別バージョンを表します。

BEMクラス名をどのように使うか、ひとつの **参考例** を示します。

    .person{}
    .person--woman{}
        .person__hand{}
        .person__hand--left{}
        .person__hand--right{}

この例ではまず、person（人）を表す基本的なオブジェクト（抽象的な概念）を定義しています。
このpersonにはwoman（女性）という別のタイプがあります。
さらに、personはhand（手）要素を子要素として持っていて、さらにhand要素にはleft（左）とright（右）という亜種があることが分かります。

この命名方法によって、セレクターは基本的なオブジェクトごとに名前空間を分けることができます。
さらに、子要素なのか(`__`)、亜種なのか(`--`)を使い分けることによって、このセレクターがどんな役割を持っているのかについても知ることができます。

`.page-wrapper` という独立したセレクター名について考えてみましょう。
このセレクターの名前は抽象要素やコンポーネントの一部ではなく、page wrapper（ページ囲み）という名前を綴ったものですので、これは正しく命名されているといえます。
それでは `.widget-heading` はどうでしょうか。これは何らかのコンポーネントと関係している（`.widget`の子要素だと思われます）ので、この場合は `.widget__heading` と改名するのが適切でしょう。

BEM法はちょっと見た目が悪くて冗長ではありますが、クラス名だけを見て要素間の関係や機能を知ることが出来るため、大変心強いものです。また、BEM記法は繰り返し同じ単語が頻出しますので、gzipなどの典型的な圧縮とも相性が良いです。

あなたがBEM法を使うかどうかにかかわらず、クラス名はいつでもよく考えて命名するべきです。
具体的には以下のようです。まず、短すぎず、長すぎず、必要最小限の長さにしてください。
つぎに、オブジェクトや抽象名は漠然とさせておいて（例えば `.ui-list` とか `.media` とか）、
なるべく広範囲で再利用できるようにしておき、具体的にオブジェクトを拡張する時に、ハッキリした名前付け（例えば `.user-avatar-link` など）にします。
クラス名の数やそれによるファイルサイズ増加については気にする必要はありません。gzip圧縮が信じられないくらい小さく圧縮してくれます。

### HTML内のクラス名

ちょっとしたことですが、読みやすくするために、HTML内のクラス名は2スペースあけておきます。
このようです。

    <div class="foo--bar  bar__baz">

このようにスペースを増やすことで、複数のクラス名を認識しやすくなったり、読みやすくなることが期待できます。

### JS連携

**CSSスタイルで使っているクラス名を、JavaScriptから使うのは絶対にやめましょう!!**
JSによる振る舞いの実装にスタイルのクラス名を使い始めると、もう今後そういうこと無しに
JSの実装が行えなくなって大変なことになります。

もしJSで使いたいクラス名をマークアップする必要があるなら、JS用と分かるCSSクラス名を使いましょう。
単純に `.js-` のようなものを付ける方法があります。例えば `.js-toggle` とか `.js-drag-and-drop` です。
この場合、JS用とCSS用のクラス名を同時にマークアップしてかまいません。重複して面倒なことになることはありません。

    <th class="is-sortable  js-is-sortable">
    </th>

上のマークアップ例では、2つのクラスを併記しています。ひとつはソート可能なテーブルのカラムに設定するスタイル名で、もうひとつはソート機能実装用のクラス名です。

### 国際化

イギリス人開発者（colorではなくcolourと綴ったりすることに時間を費やすような人）であるかわりに、私は一貫性を重視してアメリカ英語（US-English）でCSSを書く方が良いと思っています。
他の言語のコードと同様に、CSSを `color:red;` のようにアメリカ英語で書いておきながら、 `.colour-picker{}` のような（イギリス英語）クラス名を混ぜてしまうのは一貫性がありません。
以前、以下のように、バイリンガルなクラス名の書き方をアドバイスしたり提唱したことがありました。

    .color-picker,
    .colour-picker{
    }

しかし最近になって私は、とても大きなSassを使ったプロジェクトで、大量の「colour」を使った
変数（例えば `$brand-color` とか `$highlight-color` など）をメンテナンスするうちに、
これはとても大変なことに気が付きました。それぞれ2回検索したり、置換することになるからです。

一貫性が重要である場合、いつでもクラス名前や変数は、お使いのロケールに合わせて書くようにしましょう。

## コメント

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

皆さん、コメントやドキュメントはできるだけたくさん書きましょう。
あなたにとっては、コメントがなくてもコードの見通しが良く、十分な説明が出来ていると思うようなものであっても、他の開発者にとってはそうでもないかもしれません。
ひとかたまりのコードを書いたなら、都度コメントも書きましょう。

### コメントでCSS強化

ここでは、以下の3つの、コメントを使ったいくつかの先進的な技術を紹介します。

* 準限定セレクター
* コードへのタグ付け
* 相互リンクポインター

#### 準限定セレクター

セレクターを限定すべきではありません。つまり、`ul.nav{}`のように書くべきではなくて、
単純に `.nav` と書きます。セレクターを限定することは、セレクターのパフォーマンスを低下させます。
また、限定された要素以外に適用できなくなるため、クラス名の再利用性が低下します。
全くいいことはありませんので、とにかく避けるべきです。

しかしながら、すぐとなりの開発者にクラスの意図を伝えるときに、たまに便利な時があります。
例えば `.product-page` というクラス名について考えましょう。このクラス名は上位に来る
入れ物のような響きがしますので、もしかしたら `html` や `body` 要素に使うかもしれません。
でも、 `.product-page` というクラス名だけではどの要素に使うのかわかりません。

そこで、コメントを使って限定セレクターっぽく見せることで、このクラスをどの要素に使って欲しいのかを
別の開発者に教えることができます。以下のようです。

    /*html*/.product-page{}

これで、このクラスをどの要素に使ったらいいかを正確に知ることができます。
しかも、先程説明した詳細化や再利用性の問題もありません。

他の例をあげてみましょう。

    /*ol*/.breadcrumb{}
    /*p*/.intro{}
    /*ul*/.image-thumbs{}

このように、これらのクラスをどこに使うつもりなのかを、セレクターの動作を変えずに知らせることができます。

#### コードへのタグ付け

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

これらのタグによって、他の開発者が「機能」で検索した時に、目的のコードへ辿り着くことができます。
つまり、ある開発者がリストする機能について何か探していて、 `^lists` という単語を検索すると、
`.nav` や `.matrix` というクラス名を（もっとたくさん見つかるかもしれませんが）
見つけることが出来るというわけです。

#### 相互リンクポインター

オブジェクト指向な方法で開発を行なっていると、骨組みとなる基本的な枠組みのCSSと、
それに対するスキンのような拡張部分を分けて書くことが多いと思います。
その場合、基本部分と拡張部分は密接に関係しているのですが、ファイル上では離れた場所に
書かざるを得ないことがよくあります。
この基本部分と拡張部分に、それとよく分かるリンクを張るために、<i>相互リンクポインター</i>を
使うと良いです。これを行うためには、以下のようにコメント文を書きます。

まず、基本部分でこう書きます。

    /**
     * `.foo` を theme.css で継承すること
     */
     .foo{}

次に、テーマのような拡張部分でこう書きます。

    /**
     * base.css の `.foo` を継承している
     */
     .bar{}

これで、離れた地点のコードに強固な関係が築けました。

---

## CSSの書き方理論

これまでのセクションでは、どのようにCSSを構成して記述するかという事について扱いました。
つまり、とても形式的な内容でした。このセクションでは、もうすこし理論的な考え方や
取り組み方について説明します。

## 新しいコンポーネントの作り方

新しくコンポーネント（部品）を作る場合は、まずCSSよりもマークアップの方から書くようにしましょう。
この方法により、どのCSS属性が継承されていて何を書き加えるべきかを視覚的に確認でき、その結果冗長なCSS属性の定義を避けることができます。

マークアップを先に書くことで、表示されるべきデータや内容、またそれらの意味について集中することができます。
そうすれば、そのあとで適切なクラス名やCSSについて考えることができます。

## オブジェクト指向CSS

私はオブジェクト指向CSS方式で作業しています。つまり、コンポーネントの枠組みである基本部分と、
拡張部分であるスキンを分けて書いています。現実の例ではないですが、このような雰囲気です。

    .room{}

    .room--kitchen{}
    .room--bedroom{}
    .room--bathroom{}

家にいくつかのタイプの部屋があるとします。それらの部屋は似たような特徴を持ってます。
すなわち、どの部屋にも、床と天井と壁とドアがあります。
そこで、この共通な特徴の情報を、抽象化した `.room{}` クラスで共有することにします。
しかしながら、それぞれの部屋の詳細については他のものと違います。
キッチンはタイル張りの床で、ベッドルームはカーペットかもしれませんし、
バスルームには窓がなくてもベッドルームにはおそらく窓はあるでしょう。
また、それぞれの部屋の壁の色は多分ちがうでしょう。
オブジェクト指向CSSによると、基本オブジェクトで共通のスタイルを定義し、
具体的なクラスで基本オブジェクトを _拡張_ して、ユニークな特性を定義することになっています。

そこで、大量のコンポーネントをバラバラに作る代わりに、コンポーネント間で繰り返し現れるデザインのパターンを
見つけ出して、再利用可能なクラスとして抽象化してみましょう。
つまり、骨組みとなる基本「オブジェクト」を作り、その基本オブジェクトに抽象化したクラスを定義します。
そして、それらのクラスを拡張して、よりユニークな詳細をスタイルとして定義するのです。

もし新しいコンポーネントを作る場面になったら、そのコンポーネントを骨組みとスキンに分けてみましょう。
コンポーネントの骨組みは、構築時に再利用するためにとても汎用的なクラスを使います。
そして、より具体的なクラスでスキンを定義してデザイン要素を埋め込んでいきます。

## レイアウト

コンポーネントを新たに作る場合は、横幅に特に制限を設けるべきではありません。
つまり、フルードレイアウト出来るようにしておいて、コンポーネントを格納する側の
親要素やグリッドシステムが横幅を決められるようにしておきましょう。

高さは **絶対に** 設定すべきではありません。
高さは、（画像とかスプライトのような）サイトを _表示する前から_ サイズが決まっているものにのみ適用すべきです。
間違っても、`p` とか `ul` とか `div` とか、そういうものに高さを設定してはいけません。
高さを調整したい場合、 `line-height` を使うことでより柔軟にやりたいことを達成できるはずです。

グリッドシステムは棚のようなものだと考えるべきです。
何らかの中身を格納するものであり、グリッドシステム自体が主役にはなりません。
グリッドシステムを設置したら、次はその中に具材を詰めていきましょう。
グリッドとコンポーネントの配置の設定をお互いに独立にさせておくことで、
サイズを固定にしてしまうよりも、コンポーネントをグリッドの中でより自由に動かすことができます。
つまり、前面部のレイアウトを柔軟にして、スムーズにレイアウトが動作するようにしましょう。

グリッドの各要素に直接スタイルを設定すべきではありません。
先程も書きましたように、グリッドは入れ物であり、それら自身はコンテンツではありません。
スタイルはグリッドの _中の_ 要素に対して適用しましょう。
_どんな_ 状況においても、決して、ボックスモデルの属性をグリッドを構成する要素に適用してはダメです。

## UIのサイズ指定

私はUIのサイズ調整にいくつかの方法を組み合わせています。
パーセント(%)、ピクセル(pixel)、文字幅(em, rem)を使って、他は全く使いません。

グリッドシステムは理想的にはパーセントで設定すべきです。
グリッドシステムをカラムの幅やページレイアウトの制御に使うことで、
コンポーネントが自由にサイズを調整する余地を残すことができます。（先程議論しましたね）

フォントサイズについてはrem単位を使い、予備としてピクセル指定を併記します。
この方法は、em単位による柔軟性を確保すると同時に、ピクセル指定でバックアップすること(ピクセルフォールバック)で信頼感が得られます。
ここで、Sassを使ってremとピクセルフォールバックを行う例をお見せします。
（この例ではベースフォントサイズ($base-font-size)がどこかで設定されているとします）

    @mixin font-size($font-size){
        font-size:$font-size +px;
        font-size:$font-size / $base-font-size +rem;
    }

私は、予めサイズの決まっているのもの定義には、ピクセル単位しか使いません。
画像などの絶対的にピクセルでサイズが決まっているものも、ピクセル指定を使います。

### フォントサイズ

私はグリッドシステムにあわせて、文字のサイズに関する一連のクラスを定義します[※1]。
これらのクラスは Double stranded heading hierarchy 方式[※2]で
スタイルの適用を行なっています。この方式について詳しくは私の記事[Pragmatic, practical font-sizing in CSS](http://csswizardry.com/2012/02/pragmatic-practical-font-sizing-in-css)を参照してください。

[※1] ：ヘッダー、フッター、サイドバーなどで、それぞれ文字のサイズを調節するという事です。

[※2] ：論理的な見出し構造と物理的な見出しサイズを、複雑にせずに簡単に設定する方法です。詳細はリンク先を参照してください。

## 省略記法

**CSSの省略記法は注意して使うべきです。**

CSS属性の定義にて、 `background:red;` のように短く書いてしまう誘惑に駆られますが、
そのように書いてしまうと「私は背景として、画像なしだけど本文と同期スクロールで、
左上配置のX-Y方向のリピートで、背景色は赤でよろしく」と指定したことになります。
10回のうち9回は、それで特に問題はないかもしれませんが、残りの1回で大抵困ったことになり、
もう省略記法なんて使うもんかと腹が立つ事態になるはずです。
きちんと `background-color:red;` と書くようにしましょう。

同様に、 `margin:0;` は短くて便利ですが、もっと **明示的に書くべきです** 。
実は要素の下の余白だけに設定したかったのであれば、 `margin-bottom:0;` と書くほうがより適切です。

CSSの属性は明示的に書くようにして、省略記法によって意図せず属性が設定されないように気を付けるべきです。
つまり、要素の下側の余白だけ無くしたいのに、全ての四隅の余白を `margin:0;` で
無くしてしまうなんてことはナンセンスです。

省略記法はいいものですが、簡単に誤用してしまうものです。

## IDについて

まず、セレクターの一般論に入る前に、CSSのIDについてひとつだけ注意したいことがあります。

**絶対にCSSでIDセレクターは使ってはダメです！**

JSとの連携のためのマークアップや、要素の特定のためにIDを使うことはできますが、
スタイルの設定にはクラスを使ってください。
みなさんも、一箇所にしか適用しないようなものを、スタイルシートの中で見たくないですよね！

クラスには再利用性の利点が常にあり（私たちが望む望まないにかかわらず）、
クラスには優れた低詳細度（low specificity / 訳注：いろんな所に適用できる性質）があります。
低詳細度はプロジェクトの困難に立ち向かう手っ取り早い方法のひとつであり、
詳細度を低く保つことは絶対の義務です。
ひとつのIDは、ひとつのクラスよりも **255** 倍も狭い範囲しか特定できないので、
今後とも _永遠に_ CSSの中で使わないようにしてください。

## セレクター

セレクターは短く、効率的で、可搬性があるように気を使ってください。

重たい子孫セレクター（location-based selectors）はいくつかの理由から避けるべきものです。
例えば、 `.sidebar h3 span{}` というセレクターについて考えてみましょう。
このセレクターは子孫の指定が多いので、スタイルを維持したまま `span` 要素のスタイルを `.sidebar` の `h3` の中以外に移動することができません。

長すぎるセレクターは、パフォーマンスの問題も引き起こします。
つまり、セレクターのチェックの数が多くなります（例えば、`.sidebar h3 span` だと3回、
`.content ul p a` だと4回のチェックが発生します）ので、ブラウザーに負荷がかかります。

スタイルはできるだけ特定の子孫要素に依存しないようにしつつ、短く良いセレクターになるように気を付けましょう。

全体的に、セレクターは短くなるように意識するべきですが（例えばクラス階層はひとつにするなど）、
クラス名自体は必要であれば長くても構いません。 `.usr-avt` よりも `.user-avatar` の
方がはるかに良いです。

**覚えておいてください：** クラス名には意味も無意味もありません。機能的かそうでないかが重要です。
クラス名にコンテンツ上の意味を押し付けるのをやめて、機能性や将来性を考えて名前を付けましょう。

### 過剰限定セレクター

これまでの議論の通り、限定セレクターにいい話はありません。

`div.promo`のようなものは過剰に限定されたセレクターです。
`.promo`と書くだけで同じ効果が得られるでしょう。
もちろん、時々意図的に限定されたクラス名を使いたい時があるでしょうが
（つまり、`.error`という汎用的なクラスがあって、特定の要素の時にはもう少し修飾したいというような場合（こんな感じ `.error{ color:red; }` `div.error{ padding:14px; }`））、
一般的には避けるべきです。

別の過剰な限定セレクターの例として、`ul.nav li a{}` というものがあります。
これも純粋に `ul` を取り去ることができます。なぜなら、私たちは `.nav` は
リスト要素であることを知っていて、それゆえ `a` 要素は _必ず_ `li` の中にある
はずですので、最終的に `ul.nav li a{}` は `.nav a{}` のように書き直せます。

### セレクターのパフォーマンス

ブラウザーが常にCSSのレンダリング速度を向上させているのは事実ですが、
効率はあなたが気を付けることで何とか出来る部分です。
短く、入れ子になっていないセレクターを使いましょう。
ユニバーサルセレクター（`*{}`）をキーセレクター（※3）として使ってはダメです。
また、CSS3の複雑なセレクターを避けることも問題回避に役立つはずです。

[※3] セレクターのうち最も右に現れるセレクターをキーセレクタと言います。

## CSSセレクターの意図

DOMをたどって特定の要素を指定するようなセレクターを使う代わりに、
スタイルが必要な要素に直接クラスを書く方がしばしば最良の場合があります。
`.header ul{}` というセレクターの例を考えましょう。

問題の `ul` 要素は、今私達が考えているWebサイトにとって、メインのナビゲーションであるとしましょう。
`ul`要素がヘッダーの中にあって、そこに`ul`要素しか無いのであれば、先ほどのセレクターでもうまく行くでしょう。
しかし、それは理想的で賢明なセレクターであるとはいえません。
将来性がなく、きっとセレクターとして要素をうまく指定できない状況が来るでしょう。
この状況で別の`ul`要素をヘッダーに加えると、意図せずナビゲーション用のスタイルが適用されてしまいます。
こうなると、大量のコードのリファクタリングを行うか、`.header`内に追加した`ul`要素にナビゲーション用の
スタイルを打ち消すようなスタイリングを行う必要が出てきます。

セレクターの意図は、何かをスタイリングするという目的の意図と一致していなければなりません。
つまり、以下のように自問自答してみましょう：
**「私はこの要素を`.header`内の`ul`だから選択しているのだろうか？ それともWebサイトのメインナビゲーションだからだろうか？」**
この質問への答えによって、あなたの書くセレクターが自ずと決まるでしょう。

キーセレクターは、要素のタイプを指定したり、抽象オブジェクトのクラスを指定するような
ものにならないようにしましょう。 `.sidebar ul{}` や `.footer .media{}` のようなセレクターを
テーマ用のスタイルシートで、全くもって見たくないわけです。

とにかく明示的であるべきです。つまり、効果を及ぼしたい要素に照準を合わせるべきです。
その親要素ではありません。
また、マークアップが変わらないことを仮定するべきではありません。
**現在たまたまそうなっていることを狙うのではなく、必要な要素を狙ってセレクターを書きましょう。**

さらにまとまっている私の記事がありますので、ぜひ参照してください。
[Shoot to kill; CSS selector intent](http://csswizardry.com/2012/07/shoot-to-kill-css-selector-intent/)

## `!important`

`!important`をヘルパー的なクラスに使うのは良いです。
`!important`をつけたスタイルが優先されることを **常に** 自覚しているのなら、
天下り的に`!important`を付けても悪くありません。例えば `.error{ color:red!important }` のような
ものです。

`!important` を試行錯誤的に使うこと（つまりCSSがこんがらがっていて、それを何とかしたい時など）は、
勧められません。そのCSSを見なおして、セレクターをリファクタリングすることで、
こんがらがった問題を解決しようとしてください。
セレクターは短くしてIDを避けるとで、この場合ほとんどの問題は解決します。

## マジックナンバーと絶対値

マジックナンバーは「丁度それでうまくいく」という理由で使われている数字です。
なぜそれがうまく行くのか本当の理由について分かってないということと、
大抵将来性がないか柔軟性や寛容性がないため、良くないことです。
問題の解決ではなく、対処療法のようなものです。

例えば、ナビゲーション要素の下にドロップダウン要素を動かすために、
`.dropdown-nav li:hover ul{ top:37px; }`という記述を使うことは、
37pxというマジックナンバーが出てくるので良くないです。
この場合、`.dropdown-nav`の要素が37pxの高さの時だけしか、ちゃんと動きません。

代わりに、`.dropdown-nav li:hover ul{ top:100%; }`という記述を使えば、
`.dropdown-nav`がどんな高さであっても、ドロップダウン要素はナビゲーション要素の
上から高さ100%の位置に表示されます。

数値を直接コーディングしようとする場合は、毎回2回はよく考えてください。
つまり、キーワードや別名（例えば`top:100%`というのは「上から高さ100%の位置」という別名）が
使えないかどうか、全く数値を測定しないでやる方法が無いかどうか考えてください。

全てのハードコードされた数値は、必ずしも必要でなかった努力です。

## 条件付きスタイルシート

IE用のスタイルシートは、おおむね全体的に避けることができます。
IE用のスタイルシートが必要とされるのは、露骨な機能不足を避けるためだけです（PNG透過修正とか）。

一般的に、IE用スタイルシートが無くても、全てのレイアウトルールやボックスモデルは
CSSを見なおして修正するだけで動きます。これはつまり `<!--[if IE 7]> element{ margin-left:-9px; } < ![endif]-->` というものや、単に「動くようにする」ためだけのいろいろなスタイリングを見なくて済むということを意味します。

## デバッグ

もしCSSで問題にぶち当たったのなら、その問題を修正しようとして **何か追加したりする前に、コードを削除しましょう** 。
すでに書かれたCSSに問題があるのであれば、そのCSSに何かを追加することは正しい解決策ではありません。

問題が無くなるまでマークアップやCSSを削除することで、
どのコードに問題があるのかを特定できます。

変なレイアウトの動きを隠すために、どこかに`overflow:hidden;`を入れてしまおうという誘惑に
駆られるかもしれませんが、大抵の場合overflowは問題ではありません。
つまり、 **症状を改善するのではなくて、きちんと問題を解決しましょう。**

## プリプロセッサー

私はプリプロセッサーとしてSassを使っています。
**プリプロセッサーは賢く使いましょう。**
Sassは、CSSをよりパワフルに書くために使うのであって、ネストを伝染させる（子孫セレクターを濫用する）のは避けましょう！
ネストしていいのは、プリプロセッサーを使わずにCSSを書く場合でも問題ない場合のみです。つまり、

    .header{}
    .header .site-nav{}
    .header .site-nav li{}
    .header .site-nav li a{}

このような書き方は、これまでの議論から、通常のCSSとしては全く必要ないものですので、
Sassで以下のように書くのは **ダメ** です。

    .header{
        .site-nav{
            li{
                a{}
            }
        }
    }

もし、Sassでこの例を書くのであれば、以下のようにします。

    .header{}
    .site-nav{
        li{}
        a{}
    }
