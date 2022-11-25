
# コード分割


## バンドル {#bundling}

多くの React アプリケーションは、[Webpack](https://webpack.js.org/)、[Rollup](https://rollupjs.org/) や [Browserify](http://browserify.org/) などのツールを使ってファイルを「バンドル」しています。バンドルはインポートされたファイルをたどって、それらを 1 つのファイルにまとめるプロセスです。このバンドルされたファイルを Web ページ内に置くことによって、アプリ全体を一度に読み込むことができます。

#### 例 {#example}

**App:**

```js
// app.js
import { add } from './math.js';

console.log(add(16, 26)); // 42
```

```js
// math.js
export function add(a, b) {
  return a + b;
}
```

**Bundle:**

```js
function add(a, b) {
  return a + b;
}

console.log(add(16, 26)); // 42
```

> 補足：
>
> 実際のバンドルはこれとは大幅に異なった見た目になります。

もし [Create React App](https://create-react-app.dev/)、[Next.js](https://nextjs.org/)、[Gatsby](https://www.gatsbyjs.org/) やこれらに類するツールを使用している場合、アプリケーションをバンドルするための Webpack のセットアップが最初から含まれています。

そうでない場合は、自分でバンドルを設定する必要があります。設定方法に関しては、Webpack のドキュメントにある [Installation](https://webpack.js.org/guides/installation/) や [Getting Started](https://webpack.js.org/guides/getting-started/) などを参照してみてください。

## コード分割 {#code-splitting}

バンドルは確かに素晴らしいですが、アプリが大きくなるにつれて、バンドルのサイズも大きくなります。特にサイズの大きなサードパーティ製のライブラリを含む場合は顕著にサイズが増大します。不用意に大きなバンドルを作成してしまいアプリの読み込みに多くの時間がかかってしまうという事態にならないためにも、常に注意を払い続けなければなりません。

大きなバンドルを不注意に生成してしまわないように、あらかじめコードを「分割」して問題を回避しましょう。
Code-Splitting は、[Webpack](https://webpack.js.org/guides/code-splitting/)、[Rollup](https://rollupjs.org/guide/en/#code-splitting) や Browserify（[factor-bundle](https://github.com/browserify/factor-bundle) を使用）などのバンドラによってサポートされている機能であり、実行時に動的にロードされる複数のバンドルを生成することができます。

コード分割は、ユーザが必要とするコードだけを「遅延読み込み」する手助けとなり、アプリのパフォーマンスを劇的に向上させることができます。アプリの全体的なコード量を減らすことはできませんが、ユーザが必要としないコードを読み込まなくて済むため、初期ロードの際に読む込むコード量を削減できます。

## `import()` {#import}

コード分割をアプリに導入する最も良い手段は動的な `import()` 構文を使用することです。

**Before:**

```js
import { add } from './math';

console.log(add(16, 26));
```

**After:**

```js
import("./math").then(math => {
  console.log(math.add(16, 26));
});
```

Webpack がこの構文を見つけると、自動的にアプリのコードを分割します。Create React App を使用している場合はすでに設定がされているため、[すぐに使用を開始することができます](https://facebook.github.io/create-react-app/docs/code-splitting)。[Next.js](https://nextjs.org/docs/advanced-features/dynamic-import) も同様です。

もし Webpack を自分でセットアップしていた場合には、Webpack の[コード分割に関するガイド](https://webpack.js.org/guides/code-splitting/)を読むと良いでしょう。あなたの Webpack の設定はだいたい[このように](https://gist.github.com/gaearon/ca6e803f5c604d37468b0091d9959269)なると思います。

もし [Babel](https://babeljs.io/) を使用している場合は、Babel が動的インポート構文をパースできても変換してしまわないようにする必要があります。そのためには  [babel-plugin-syntax-dynamic-import](https://yarnpkg.com/en/package/babel-plugin-syntax-dynamic-import) を利用すると良いでしょう。

## `React.lazy` {#reactlazy}

`React.lazy` 関数を使用すると、動的インポートを通常のコンポーネントとしてレンダーすることができます。

**Before:**

```js
import OtherComponent from './OtherComponent';
```

**After:**

```js
const OtherComponent = React.lazy(() => import('./OtherComponent'));
```

このコンポーネントがはじめてレンダーされる時、`OtherComponent` を含むバンドルを自動的にロードしてくれます。

`React.lazy` は動的インポート構文 `import()` を呼び出す関数を引数として取ります。この関数は React コンポーネントを含む `default` export を持つモジュールに解決される `Promise` を返さなければなりません。

遅延コンポーネントは、`Suspense` コンポーネント内でレンダーされる必要があります。これによって、遅延コンポーネントのローディングの待機中にフォールバック用のコンテンツ（ローディングインジケータなど）を表示できます。

```js
import React, { Suspense } from 'react';

const OtherComponent = React.lazy(() => import('./OtherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <OtherComponent />
      </Suspense>
    </div>
  );
}
```

`fallback` プロパティはコンポーネントがロードされるのを待っている間に表示したいあらゆる React 要素を受け取ります。`Suspense` コンポーネントは遅延コンポーネントより上位のどこにでも配置することができます。また、複数の遅延コンポーネントを単一の `Suspense` コンポーネントでラップすることもできます。

```js
import React, { Suspense } from 'react';

const OtherComponent = React.lazy(() => import('./OtherComponent'));
const AnotherComponent = React.lazy(() => import('./AnotherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <section>
          <OtherComponent />
          <AnotherComponent />
        </section>
      </Suspense>
    </div>
  );
}
```

### フォールバックを避ける {#avoiding-fallbacks}
既にユーザに表示されているものも含むあらゆるコンポーネントは、レンダーの結果としてサスペンドする可能性があります。画面に表示される内容の一貫性を保つため、既に表示されているコンポーネントがサスペンドした場合、React はツリーを直近の `<Suspense>` バウンダリまで非表示にする必要があります。しかしユーザの観点からはこれは不親切です。

このタブ切り替えの例で考えてみましょう：

```js
import React, { Suspense } from 'react';
import Tabs from './Tabs';
import Glimmer from './Glimmer';

const Comments = React.lazy(() => import('./Comments'));
const Photos = React.lazy(() => import('./Photos'));

function MyComponent() {
  const [tab, setTab] = React.useState('photos');
  
  function handleTabSelect(tab) {
    setTab(tab);
  };

  return (
    <div>
      <Tabs onTabSelect={handleTabSelect} />
      <Suspense fallback={<Glimmer />}>
        {tab === 'photos' ? <Photos /> : <Comments />}
      </Suspense>
    </div>
  );
}

```

この例では、タブが `'photos'` から `'comments'` に切り替わると `Comments` がサスペンドするため、ユーザは Glimmer（点滅）を見てしまうことになります。これは当然で、ユーザはもう `Photos` を見たいわけではないし `Comments` コンポーネントはまだ何もレンダーできないのですから、React はユーザ体験を一貫させるために `Glimmer` を表示するしかないわけです。

しかしこのようなユーザ体験は望ましくないことがあります。具体的には、新しい UI の準備を行っている間は「古い」UI を表示し続けるほうが望ましい場合があります。新たに導入された [`startTransition`](./react-api.html#starttransition) API を使うことで、React にこれをさせることが可能です。

```js
function handleTabSelect(tab) {
  startTransition(() => {
    setTab(tab);
  });
}
```

このコードは React に、タブを `'comments'` に切り替えるのは緊急性の高い更新ではなく、少し時間のかかる[トランジション](./react-api.html#transitions)である、と伝えています。これにより React は、既存の UI をその場でインタラクティブに保ち、`<Comments />` の準備ができたところでそちらを表示するよう切り替えるようになります。詳細は[トランジション](./react-api.html#transitions)を参照してください。

### Error Boundary {#error-boundaries}

もし他のモジュールがロードに失敗した場合（例えば、ネットワークの障害など）、エラーが発生します。その際には [error boundary](./error-boundaries.html) を使用することによってこれらのエラーをハンドリングし、エラーの回復やユーザ体験の向上に繋げることができます。error boundary を作成したら、遅延コンポーネントより上位のあらゆる場所で使用でき、ネットワークエラーが発生した際にエラー内容を表示することができます。

```js
import React, { Suspense } from 'react';
import MyErrorBoundary from './MyErrorBoundary';

const OtherComponent = React.lazy(() => import('./OtherComponent'));
const AnotherComponent = React.lazy(() => import('./AnotherComponent'));

const MyComponent = () => (
  <div>
    <MyErrorBoundary>
      <Suspense fallback={<div>Loading...</div>}>
        <section>
          <OtherComponent />
          <AnotherComponent />
        </section>
      </Suspense>
    </MyErrorBoundary>
  </div>
);
```

## ルーティング単位でのコード分割 {#route-based-code-splitting}

アプリ内のどこにコード分割を導入するかを決めるのは少し面倒です。バンドルを均等に分割する場所を確実に選択したいところですが、ユーザ体験を妨げてはなりません。

コード分割を導入するにあたって適している場所はルーティングです。Web を使用するほとんどの人は、多少のロード時間がかかるページ遷移に慣れています。また、ユーザがページ上の他の要素を同時に操作する可能性を減らすよう、ページ全体を一度に再レンダーすることが多いでしょう。

これは [React Router](https://reactrouter.com/) のようなライブラリを使ったアプリに `React.lazy` を使用することでルーティングベースのコード分割を導入する方法の例です。

```js
import React, { Suspense, lazy } from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';

const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </Suspense>
  </Router>
);
```

## 名前付きエクスポート {#named-exports}

`React.lazy` は現在デフォルトエクスポートのみサポートしています。インポートしたいモジュールが名前付きエクスポートを使用している場合、それをデフォルトとして再エクスポートする中間モジュールを作成できます。これにより、tree shaking が機能し未使用のコンポーネントを取り込まず済むようにできます。

```js
// ManyComponents.js
export const MyComponent = /* ... */;
export const MyUnusedComponent = /* ... */;
```

```js
// MyComponent.js
export { MyComponent as default } from "./ManyComponents.js";
```

```js
// MyApp.js
import React, { lazy } from 'react';
const MyComponent = lazy(() => import("./MyComponent.js"));
```
<span style="float: footnote;"><a href="./index.html#toc">Go to TOC</a></span>