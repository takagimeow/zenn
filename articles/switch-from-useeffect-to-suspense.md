---
title: "useEffectをやめて、Suspenseを使おう"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["React", "Suspense", "useEffect"]
published: false
---

Reactコンポーネントの開発時、データフェッチは欠かせません。

SPAで開発を行う時、あなたも含めて`useEffect()`を使ったことがあるはずです。

あなたがSWRやReact Queryの代わりに`useEffect()`を使う理由は、いくつかあるのでしょう。

そんな方のために、Reactが提供する`<Suspense>`を使ってデータフェッチを行う方法を紹介します。

:::message
Suspenseを実際のアプリケーションで使う場合、Next.jsなどのメタフレームワークを使用してください。もしくは、Suspenseに対応したデータフェッチライブラリを使いましょう。単体での実装方法は、公式でもドキュメント化されていない内容です。あらかじめご了承ください。
:::

# useEffectとは

`useEffect()`は外部システムとの同期のために使われます。

外部システムとの同期は**side-effects**です。

**side-effects**のある関数は、外側にあるものを変更します。

そして、与えられた入力に対して同じ値を返すことが保証されません。

なぜなら、外部システムの状況によっては失敗や拒否が発生するからです。

具体的な処理は次の内容です。

- APIからデータを取得する
- データベースと通信を行う
- ローカルストレージからデータを取得する
- イベントリスナーの登録と解除を行う

# useEffectの実装

一般的な実装を見てみましょう。実際のデータを取得せず、`setTimeout()`を使って模擬的なデータフェッチを行います。

```tsx
type Article =  {
  id: string;
  title: string;
  content: string;
}

function fetchData(): Promise<Article[]> {
  return new Promise((resolve) => {
    setTimeout(() => {
      const articles: Article[] = [
        {
          id: "1",
          title: "Article 1",
          content: "Content 1"
        },
        {
          id: "2",
          title: "Article 2",
          content: "Content 2"
        },
        {
          id: "3",
          title: "Article 3",
          content: "Content 3"
        },
      ];
      resolve(articles);  
    }, 1000);
  });
}

export function Articles() {
  const [loading, setLoading] = useState(true);
  const [articles, setArticles] = useState<Article[]>([])
  useEffect(() => {
    (async () => {
      const data = await fetchData2();
      setArticles(data);
      setLoading(false);
    })();
  }, []);

  if (loading) {
    return <h2>Loading...</h2>
  }

  return (
    <div>
      {
        articles.map((article) => (
          <div key={article.id}>
            <h2>{article.title}</h2>
            <p>{article.content}</p>
          </div>
        ))
      }
    </div>
  )
}
```

`loading`をコンポーネント内で管理しています。

`true`の間のみ、`Loading...`を表示します。そして、`false`になったら、`articles`を表示します。

`useEffect()`を使って、データフェッチが必要なコンポーネントを実装するたびに、このようなボイラープレートを書くべきなのでしょうか？

# useEffectを使う場合に開発者が意識すること

`useEffect()`でデータフェッチを実装する場合、開発者が意識しないといけないことは多いです。

- 読み込み状態の管理
- 読み込みに失敗した時のエラーの管理
- アンマウント時のリクエストのキャンセル
- 状態の更新
- キャッシュの操作
- 楽観的UIの提供

キャッシュは本当に複雑です。設計を自分で行う場合、次の問題が発生します。

- コードの複雑化
- 複雑化に伴うエラー
- 不必要なAPIの呼び出し
- ネットワークが詰まった場合の不具合

# UXの観点から見たuseEffectの問題

`useEffect()`を使ったデータフェッチは、**Fetch-on-render**と呼ばれます。

このデータフェッチはコンポーネントのレンダリング後に実行されます。

つまり、ウォーターフォール問題を引き起こします。

次の例を考えましょう。

1. `<Articles>`に`useEffect()`を実装します。
2. この`useEffect()`では、データフェッチが行われます。
3. `<Articles>`は`<Article>`をレンダリングします。
4. `<Article>`には`useEffect()`が実装されています。
5. この`useEffect()`では、同じようにデータフェッチが行われます。

レンダリングが始まるまで、データフェッチが行われないことはアプリケーションが低速化する原因になり得ます。

この問題を解決するためには、レンダリングを実行する前にデータフェッチを行わないといけません。

# Suspenseとは

Suspenseは、非同期処理を管理するためのAPIです。そして、コンポーネントがデータを保持していることをReactに伝えるための仕組みです。

データフェッチが完了するよりも前に、コンポーネントをレンダリングしません。

完了したタイミングで、即座にレンダリングを開始します。

これを、**Render-as-you-fetch**と呼びます。

# Suspenseの実装

データフェッチが完了するまでは読み込み状態を表示し、データフェッチの完了後にレンダリングを行いたい場合は、対象のコンポーネントを`<Suspense>`で囲みます。

コンポーネントがレンダリング中にサスペンドしたタイミングで、最も近い親要素であるSuspenseのフォールバックにレンダリングが切り替わります。

```tsx
<Suspense fallback={<Loading />}>
  <Articles />
</Suspense>
```

Suspenseは、`throw`された内容を補足します。

そして、それが本当のエラーかPromiseかを確認します。

エラーの場合は最も近くの`Error Boundary`までバブルアップさせます。

Promiseの場合は、フォールバックをレンダリングします。

:::message
フォールバックは、スピナーやスケルトンなどのプレースホルダーコンポーネントです。
:::

ネットワークリクエストを実行後、すぐにレンダリングを行うため、レンダリングまでにレスポンスを待つ必要はありません。

このサンプルでは、`<Articles>`を実装します。この中で、データフェッチを行うため`<Suspense>`で囲みます。

```tsx
import React, { Suspense } from "react";
import { Articles } from "./Articles";
export function Home() {
  return (
    <>
      <Suspense fallback={<h2>Loading...</h2>}>
        <Articles />
      </Suspense>
    </>
  )
}
```

データフェッチの間は`Promise`を`throw`します。

そこで、`Promise`の状態に応じて`throw`を行い、解決後にデータを返す`use()`を実装します。

ここでは、`value`や`status`を参照しています。そのままでは型エラーが出るので定義します。

```ts
declare global {
  interface Promise<T> {
    status: 'pending' | 'fulfilled' | 'rejected';
    value: T;
    reason: any;
  }
}

export function use<T>(promise: Promise<T>) {
  if (promise.status === 'fulfilled') {
    return promise.value;
  } else if (promise.status === 'rejected') {
    throw promise.reason;
  } else if (promise.status === 'pending') {
    throw promise;
  } else {
    promise.status = 'pending';
    promise.then(
      result => {
        promise.status = 'fulfilled';
        promise.value = result;
      },
      reason => {
        promise.status = 'rejected';
        promise.reason = reason;
      },      
    );
    throw promise;
  }
}
```

再レンダリングのたびに`Promise`が再生成されます。その結果、どんなに`status`を変更しても、`undefined`になってしまいます。

これでは、ずっとフォールバックが表示されます。この問題を解決するため、対象の`Promise`をキャッシュします。

ここでは簡易的な実装に留めるため、データフェッチ関数の呼び出し時にキーを渡しています。

```ts
type Article =  {
  id: string;
  title: string;
  content: string;
}

const cache = new Map();

function fetchData(key: string): Promise<Article[]> {
  const promise =  new Promise((resolve) => {
    setTimeout(() => {
      const articles: Article[] = [
        {
          id: "1",
          title: "Article 1",
          content: "Content 1"
        },
        {
          id: "2",
          title: "Article 2",
          content: "Content 2"
        },
        {
          id: "3",
          title: "Article 3",
          content: "Content 3"
        },
      ];
      resolve(articles);  
    }, 1000);
  });

  if (!cache.has(key)) {
    cache.set(key, promise);
  }
  return cache.get(key);
}
```

作成した`use()`と`fetchData()`を使って、`<Articles>`を実装します。

```tsx
export function Articles() {
  const articles = use<Awaited<ReturnType<typeof fetchData>>>(fetchData("key1"))
  return (
    <div>
      {
        articles.map((article) => (
          <div key={article.id}>
            <h2>{article.title}</h2>
            <p>{article.content}</p>
          </div>
        ))
      }
    </div>
  )
}
```

最初に、フォールバックが表示されます。

![](https://storage.googleapis.com/zenn-user-upload/69d425f6e8aa-20230504.png)

その1秒後に、データが表示されます。

![](https://storage.googleapis.com/zenn-user-upload/0315c1bbf1a8-20230504.png)

# Suspenseを使うメリット

Suspenseを使うと、読み込み中であることを示す内容をコンポーネント内に配置する必要がありません。これにより、定型的なコードの削除につながります。

このデータフェッチの体験は、同期的なデータの取得体験に近いものがあり可読性の向上につながると考えられています。

ネットワークリクエストは並列に実行されます。そのため、ウォーターフォール問題が発生しません。

これらのメリットは、アプリケーションの高速化に大きく寄与し、UXとDXの向上に影響を与えます。

上記の`<Articles>`の子コンポーネントとして、`<Article>`を実装しました。その中でリクエストを行い、その際のパフォーマンスを測定しました。

`useEffect()`を使った場合がこちらです。

![](https://storage.googleapis.com/zenn-user-upload/cf99f3356069-20230505.png)

`<Suspense>`を使った場合がこちらです。

![](https://storage.googleapis.com/zenn-user-upload/683fd47000b9-20230505.png)

ご覧の通り、`useEffect()`ではウォーターフォール問題が発生しているのに対し、`<Suspense>`では並列にリクエストが行われています。

:::message
再レンダリング時のキャッシュに関する問題は、Suspense単体では解決できません。ドキュメントでもSuspenseに対応したライブラリやフレームワークの使用を推奨しています。RelayやNext.jsなどのSuspenseに対応したフレームワークを使用しましょう。
:::

# まとめ

外部システムとの同期には、データフェッチも含まれています。そして良いUXのために、データフェッチは非同期な処理であるべきです。

しかし、これを実現するために`useEffect()`を実装するには、開発者に求められることはあまりに膨大です。

DXはやがてUXに影響を与えます。その逆も然りです。

ウォーターフォール問題の解決も含めて、Suspenseの利用を一度考えてみませんか？

# 参考

https://www.makeuseof.com/tanstack-query-vs-useeffect-hook-in-react/

https://dev.to/smitterhane/swap-out-useeffect-with-suspense-for-data-fetching-in-react-2leb

https://kentcdodds.com/blog/remix-the-yang-to-react-s-yin

https://www.telerik.com/blogs/suspense-server-react-18

https://www.telerik.com/blogs/concurrent-rendering-react-18

https://blog.logrocket.com/react-suspense-data-fetching/

https://react.dev/reference/react/Suspense

https://zenn.dev/uhyo/books/react-concurrent-handson/viewer/render-as-you-fetch