# Vivliostyle Viewer

```js {mixin: true}
Vue.component("Link", {
  props: { path: String },
  template: `<a v-bind:href="url" target="_blank"><slot>{{url}}</slot></a>`,
  computed: {
    url() {
      const iLocalDocs = location.href.indexOf("/docs/#/");
      const isLocal = iLocalDocs !== -1;
      let path = this.path;
      if (!isLocal) {
        path = path.replace("../samples/", "https://vivliostyle.github.io/vivliostyle_doc/samples/");
      }
      let prefix = "";
      if (path.startsWith("/viewer/")) {
        if (isLocal) {
          prefix = location.href.slice(0, iLocalDocs);
        } else {
          prefix = "https://vivliostyle.org";
        }
      }
      return prefix + path;
    }
  }
});
```

Vivliostyle Viewer は、HTML+CSS文書を組版・表示するWebアプリケーションです。

## 使い方

### Vivliostyle Viewer をローカル環境で利用するには

Vivliostyle Viewer をローカル環境で利用するには、Vivliostyle CLI の preview コマンドを使うのが便利です。Vivliostyle CLI のインストール方法と使い方については、[Vivliostyle CLI ユーザーガイド](./vivliostyle-cli.md)、preview コマンドについては [組版結果のプレビュー](./vivliostyle-cli#組版結果のプレビュー) をご覧ください。

それ以外の方法で Vivliostyle Viewer をローカル環境で利用する場合は、配布パッケージに同梱の [README](https://github.com/vivliostyle/vivliostyle.js/blob/master/packages/viewer/README.ja.md) に記載の「使い方」の手順にしたがってWebサーバーを起動してください。

### オンライン公開 Vivliostyle Viewer 

オンラインで公開されている [vivliostyle.org/viewer/](https://vivliostyle.org/viewer/) も利用でき、これは常に最新のリリース版に更新されています。いち早く最新のバージョンを試したい場合は、[vivliostyle.vercel.app](https://vivliostyle.vercel.app)を利用してください。

### Vivliostyle Viewer を自分のサイトに配置

Vivliostyle Viewer の配布パッケージ（https://vivliostyle.org/ja/download/ からダウンロード）を解凍したものを自分のサイトに配置すれば、自分のサイトで Vivliostyle Viewer で閲覧する出版物を公開することができます。

Node.js のアプリケーションで利用するには、npmパッケージ版の [@vivliostyle/viewer](https://www.npmjs.com/package/@vivliostyle/viewer) を使用してください。

### Vivliostyle Viewer のスタートページ

パラメータを指定しないで <Link path="/viewer/">Vivliostyle Viewer</Link> を開くと、文書URLの入力欄 (“Input a document URL”)、 **Book Mode** と **Render All Pages** のチェックボックス、および使い方のヘルプが表示されます。

試してみる: <Link path="/viewer/" />

## サポートされている文書の種類

- HTML文書＋ページメディア用のCSS
- 本のような出版物（目次付き） (**Book Mode**: オン)
  - Web出版物（複数のHTML文書からなるコレクション）: 最初のHTMLまたはマニフェストファイルのURLを指定します。
  - ZIP解凍済みのEPUB: OPFファイルのURLまたは解凍されたEPUBファイルのトップディレクトリを指定します。

### メモ

- GitHubとGistのURLを直接指定することができます。そのようなURLが指定された場合、Vivliostyle は github/gist の raw コンテンツをロードします。
- ⚠️Mixedコンテンツ（“http:” のURLが “https:” の Vivliostyle Viewer に指定された場合）は通常ブラウザによってブロックされます。
- ⚠️Cross-Origin（異なるドメインへのリクエスト）は、サーバーが Cross-Origin Resource Sharing (CORS) を許可するように設定されていない限り、通常はブラウザによってブロックされます。

## URLパラメータのオプション

- #**src**=&lt;document URL>
- &amp;**bookMode**=[**true** | **false**] (**Book Mode**)
  - **true** (デフォルト): 本のような出版物（目次付き）用
    - HTML文書のURLが指定された場合、その出版物マニフェストまたは目次（<nav role="doc-toc"> などでマークアップ）からリンクされた一連のHTML文書が自動的に読み込まれます。
  - **false**: 単体のHTML文書用
- &amp;**renderAllPages**=[**true** | **false**] (**Render All Pages**)
  - **true** (デフォルト): 印刷用（すべてのページが印刷可能で、ページ番号は期待されるとおりに機能します）
  - **false**: 閲覧用（おおまかなページ番号を使って、クイックロード）
- &amp;**spread**=[**true** | **false** | **auto**] (**Page Spread View**)
  - **true**: 見開き表示
  - **false**: 単一ページ表示
  - **auto** (デフォルト): 自動見開き表示
- &amp;**style**=&lt;追加の（カスタム）スタイルシートのURL>
- &amp;**userStyle**=&lt;ユーザー・スタイルシートのURL>

オプションは設定パネル（<img src="images/vivliostyle-icon.png" width="16" height="16" alt="[Vivliostyle]" />**Settings**）でも設定できます。

**NOTE:** Book Modeのデフォルトは以前はオフでしたが、Vivliostyle Viewer v2.18.0 からオンに変更されました。デフォルトで有効なのでパラメータ `&bookMode=true` の指定は省略することができます。HTMLファイル内の目次のリンク先のHTML文書を連結しないで最初のHTMLファイルだけを表示するには、`&bookMode=false` を指定してください。

## 表示するHTML文書の指定

HTMLファイルを Vivliostyle Viewer で表示するには、Webサーバーが読み込める場所（上記手順にしたがってWebサーバーを起動している場合は、配布パッケージを解凍してできたフォルダ内）にそのファイル（およびそのファイルから読み込まれるCSSや画像ファイル）を置いた上で、次のようにパラメータを付加したURLをブラウザで開きます:

```
⟨Vivliostyle ViewerのURL⟩#src=⟨表示するファイルのURL (Vivliostyle Viewerからの相対)⟩
```

注: Vivliostyle Viewer 本体とは別ドメインの文書を読み込もうとする場合、文書があるWebサーバーの設定によって、文書が読み込めない場合があります。文書を読み込ませるためには、サーバー側で CORS (Cross-Origin Resource Sharing)の設定が必要です。

注: 数式の表示（MathMLおよびTeX形式に対応）にインターネット上のJavaScriptライブラリー([MathJax](https://www.mathjax.org))を使用するため、文書に数式が含まれる場合はインターネット接続が必要です。

例: HTMLファイル <Link path="../samples/gon/index.html" /> を表示する場合:

<Link path="/viewer/#src=../samples/gon/index.html" />

## EPUB

Vivliostyle ViewerではZIP解凍済みのEPUBファイルを表示することができます。この場合、次のようにパラメータを指定します:

```
#src=⟨表示する解凍済みEPUBフォルダのURL⟩&bookMode=true
```

GitHub上に公開されているZIP解凍済みのEPUBファイルを表示する例:

- [IDPF/epub3-samples](https://github.com/IDPF/epub3-samples/)の 『[Accessible EPUB 3](https://github.com/IDPF/epub3-samples/tree/master/30/accessible_epub_3/)』

  <Link path="/viewer/#src=https://github.com/IDPF/epub3-samples/tree/master/30/accessible_epub_3/&bookMode=true" />

## Web出版物（複数HTML文書）

Vivliostyle ViewerではWeb出版物（複数のHTML文書からなるコレクション）を表示することができます。この場合、次のようにパラメータを指定します:

```
#src=⟨最初のHTML文書またはマニフェストファイルのURL⟩&bookMode=true
```

Web出版物のマニフェストの形式については、W3Cドラフトの [Publication Manifest](https://www.w3.org/TR/pub-manifest/) および [Readium Web Publication Manifest](https://github.com/readium/webpub-manifest/) をサポートしています。

### HTMLの目次

Web出版物のマニフェストが存在しなくても、指定したHTML文書内の目次要素内に他のHTML文書へのリンクがある場合は、それらの文書が自動的にロードされます。Vivliostyle はHTML文書内の次のCSSセレクタにマッチする要素を目次要素として扱います:
`[role=doc-toc], [role=directory], nav li, .toc, #toc`

Web上に公開されている複数のHTML文書からなる出版物を表示する例:

- [Cascading Style Sheets Level 2 Revision 1 (CSS 2.1) Specification](https://www.w3.org/TR/CSS2/)

  <Link path="/viewer/#src=https://www.w3.org/TR/CSS2/&bookMode=true" />

## 目次パネル

`&bookMode=true`（あるいは **Book Mode** チェックボックス On）を指定すると、単一のHTMLファイルでも[Web出版物](#Web出版物（複数HTML文書）)と同様に、HTML内に目次要素があれば目次パネル（画面左上の目次アイコン <img src="images/toc-icon.png" width="16" height="16" alt="[Table of Contents]" /> をクリックして開く）が有効になります。

## 見開きビューモード

Vivliostyle Viewer は、表示領域の横幅が大きいとき（高さの1.45倍以上）、自動的に見開きページ表示にします。これを変えるには次の指定をURLに追加します:

```
&spread=true（常に見開きページ表示）
```

```
&spread=false（常に単ページ表示）
```

```
&spread=auto（自動切り替え＝デフォルト）
```

Vivliostyle Viewer の設定パネル（画面右上のアイコン <img src="images/vivliostyle-icon.png" width="16" height="16" alt="[Settings]" /> をクリックして開く）でも、ページ表示モードの変更ができます。

## スタイルシートの追加（カスタム・スタイルシート）

HTMLファイルに指定されているスタイルシートに加えて、追加のスタイルシート（CSSファイル）を指定することができます。これにより文書のスタイルをカスタマイズするためのカスタム・スタイルシートを指定することができます。追加のスタイルシート（カスタム・スタイルシート）を使うには、次の指定をURLに追加します:

```
&style=⟨スタイルシートのURL (Vivliostyle Viewer からの相対)⟩
```

この方法で指定したスタイルシートは、HTMLファイルで指定されているスタイルシートと同様（制作者スタイルシート）の扱いで、よりあとに指定されたことになるので、CSSのカスケーディング規則により、HTMLファイルからのスタイルの指定を上書きすることになります。

### ユーザー・スタイルシート

これに対して、次のようにしてユーザー・スタイルシート（スタイル指定に `!important` を付けないかぎり、制作者スタイルシートのスタイル指定を上書きしない）の指定もできます:

```
&userStyle=⟨ユーザー・スタイルシートのURL (Vivliostyle Viewer からの相対)⟩
```

複数個の `&style=` あるいは `&userStyle=` を使うことで、複数個のスタイルシートを指定できます。

データURLも利用できます。例えば:

```
&style=data:,html{writing-mode:vertical-rl}
```

### 設定パネルでのカスタム・スタイル設定

カスタム・スタイルの設定は、設定パネルの **Custom Style Settings** からも行うことができます。設定内容のCSSは設定パネルの **Edit CSS** で確認することができます（CSSコードをそこで追加することもできます）。

Web上に公開されている文書に、設定パネルからカスタム・スタイルの設定を加えた例:

- [Cascading Style Sheets Level 2 Revision 1 (CSS 2.1) Specification](https://www.w3.org/TR/CSS2/)

  <Link path="/viewer/#src=https://www.w3.org/TR/CSS2/&bookMode=true&style=data:,/*%3Cviewer%3E*/%0A@page%20%7B%20size:%20A4;%20%7D%0A/*%3C/viewer%3E*/%0A%0A@page%20:first%20%7B%0A%20%20@top-left%20%7B%0A%20%20%20%20content:%20none;%0A%20%20%7D%0A%20%20@top-right%20%7B%0A%20%20%20%20content:%20none;%0A%20%20%7D%0A%20%20@bottom-center%20%7B%0A%20%20%20%20content:%20none;%0A%20%20%7D%0A%7D%0A%0A@page%20:left%20%7B%0A%20%20font-size:%200.8rem;%0A%20%20@top-left%20%7B%0A%20%20%20%20content:%20env(pub-title);%0A%20%20%7D%0A%20%20@bottom-center%20%7B%0A%20%20%20%20content:%20counter(page);%0A%20%20%7D%0A%7D%0A%0A@page%20:right%20%7B%0A%20%20font-size:%200.8rem;%0A%20%20@top-right%20%7B%0A%20%20%20%20content:%20env(doc-title);%0A%20%20%7D%0A%20%20@bottom-center%20%7B%0A%20%20%20%20content:%20counter(page);%0A%20%20%7D%0A%7D">
    …#src=https://www.w3.org/TR/CSS2/&bookMode=true&style=data:,…
  </Link>

```css
/*<viewer>*/
@page { size: A4; }
/*</viewer>*/

@page :first {
  @top-left {
    content: none;
  }
  @top-right {
    content: none;
  }
  @bottom-center {
    content: none;
  }
}

@page :left {
  font-size: 0.8rem;
  @top-left {
    content: env(pub-title);
  }
  @bottom-center {
    content: counter(page);
  }
}

@page :right {
  font-size: 0.8rem;
  @top-right {
    content: env(doc-title);
  }
  @bottom-center {
    content: counter(page);
  }
}
```

この例のように、設定パネルからスタイルの設定を行うと、カスタム・スタイルのCSS内のコメント `/*<viewer>*/` と `/*</viewer>*/` で囲んだ部分に、設定パネルの項目から設定したスタイルのCSSコードが生成され、設定パネルの **Edit CSS** に表示されます。ユーザー独自のCSSコードをそれに追加することができます。この例ではページヘッダーとページフッターを追加しています。

## 印刷／PDFとして出力

ブラウザの印刷・PDF保存機能を利用して、レンダリングされたドキュメントをPDFに変換できます。

例えば、Google Chrome で PDF に出力するには、メニューから「印刷」を開き、出力先として「PDFに保存」、背景のグラフィック ON と指定して「保存」します。

注: 文書の全ページを印刷する場合、設定パネルで **Render All Pages** が On になっていることを確認してください。これが Off の場合、すでに表示されたページしか印刷できず、またページ番号が正しく出力されません。

### Vivliostyle CLI

[Vivliostyle CLI](https://github.com/vivliostyle/vivliostyle-cli) を使うとコマンドラインからPDFに出力できます。印刷に適したPDF/X-1a形式への変換も可能です。

## サポートされているCSSの機能

VivliostyleがサポートするCSSの機能については [サポートする CSS 機能](./supported-css-features.md) を参照してください。
