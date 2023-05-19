---
title: "ChatGPTで日本人開発者の英語の悩みを減らす"
emoji: "🤖"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["chatgpt", "ai", "gpt4", "vscode", "typescript"]
published: false
---

# はじめに

開発者は大変です。開発者は常にコードを書くことが仕事だと思われがちです。

開発者は知っています。開発者はコードを書くことだけが仕事ではないことを。

# TL;DR

日本人開発者の英語の悩みを解決するために、VSCode拡張機能を作りました。ご活用いただけると幸いです。

https://marketplace.visualstudio.com/items?itemName=takagimeow.name-genie

リポジトリはこちらです。スターお待ちしております。

https://github.com/takagimeow/name-genie

# 開発者の負担

特に私が頭を悩ませるのが変数と関数の命名です。日本語で変数を定義できれば良いのですが、一般的なプログラミング言語では英語で変数を定義します。

そしてもう一つ大変なのがブランチ名やコミットメーセージの作成です。日本語話者しかいないチームであれば、日本語でコミットメッセージを作成して良いかもしれません。しかし、OSSに関わる場合や、国際的なチームを組む場合は英語でコミットメッセージを作成するはずです。

本来はコードに集中したいのに、英作文に集中しなければならないのは本当に大変です。

## 変数の命名

変数の命名を考えてみましょう。

ユーザー名を表す変数程度あれば簡単だと思います。

```js
const userName = "Tanaka Taro"
```

野球のストライクゾーンを表す変数はどうでしょうか？少し悩みませんか？

次のような変数名が考えられます。

- `baseballStrikeZone`
- `strikeZone`
- `pitchingZone`

それでは1日の摂取カロリーを表す変数はどうでしょうか？先ほどより悩みませんか？

次のような変数名が考えられるでしょう。

- `dailyCalorieIntake`
- `oneDayCalorieIntake`
- `caloriesPerDay`

このように、少しの変数名を考えるだけであればそれほど大変ではありません。しかし、これが数百、数千といった数になると一気に大変さが増します。それはやがて、ストレスに変わるでしょう。

## コミットメッセージとブランチ名の作成

先ほどの変数を追加した時のコミットメッセージを考えてみましょう。

できる限りコミットメッセージ規約に則りたいです。次のようなフォーマットが考えられます。

```txt
<type>[optional scope]: <subject>
```

いかがでしょうか？例えば次のようなコミットメッセージが考えられます。

```txt
feat: add daily calorie intake variable
```

それではブランチ名はどうでしょうか？次のようなブランチ名が考えられます。

```txt
add-daily-calorie-variable
```

もしくは次のようなブランチ名でしょうか？

```txt
feature/add-daily-calorie-variable
```

このように、普段慣れない言語を使ったブランチ名やコミットメッセージを考えるのは難易度が高いです。みんながみんな英語を使いこなせるわけではありません。英語への苦手意識のせいでプログラミングの楽しさを感じられない人もいるかもしれません。これは世界にとって本当にもったいないです。

# ChatGPTの活用

ここで質問です。みなさんはChatGPTをどのように活用していますか？検索エンジンの代わりに使っていませんか？それはChatGPTのおすすめされた使い方ではないと私は考えます。検索エンジンで事足りるのであれば、それは検索エンジンを使うべきです。

ChatGPTは文章生成が得意です。文脈を与えれば、それに沿った文章を生成してくれます。つまり創作が得意なのです。ご存知の通り、文章だけでなくコードの生成も得意です。

## ChatGPTを使って変数を命名する

このChatGPTの特徴を活かして、日本人開発者の負担を減らすことができないかと考えてました。

試しに、GPT-4モデルを使って変数名を考えてもらいます。

例えば、1万円から3万円の間の値を表す変数を考えてもらいましょう。

プロンプトは次のようにします。

```md
1万円から3万円の間の値を表す変数名を作成してください。
```

ChatGPTは次のような変数名を考えてくれました。

```md
以下のように変数名を作成することができます：

1. `amount_in_yen`
2. `jpy_range_10000_to_30000`
3. `price_between_10k_and_30k_yen`
4. `jpy_value`
5. `yen_amount_in_range`

ただし、この変数名はあくまで一例であり、具体的なコンテキストやプログラミング言語により、もっと適切な変数名が存在するかもしれません。
```

スネークケースの変数名が返ってきました。私はTypeScriptが好きなので、キャメルケースで生成してもらいます。

```md
1万円から3万円の間の値を表す変数名を作成してください。
言語はTypeScriptです。
```

TypeScriptだと、次のような変数名を提案してくれました。

```md
TypeScriptはJavaScriptのスーパーセットで、JavaScriptに静的型機能を追加したものです。そのため、JavaScriptと同様に変数名を作るときは特定の規則を守る必要があります。 

変数名は通常、その変数が何を表しているかを簡単に理解できるように作成します。1万円から3万円の範囲の価値を示す変数名を作るときは、その価値が何を表すかを考慮すると良いでしょう。以下に例を示します：

```typescript
let priceInRange10000To30000: number;
```

この変数名は直訳すれば「10000から30000の範囲内の価格」を意味します。他の例としては:

```typescript
let valueBetween10kAnd30k: number;
```

こちらの変数名は「10kと30kの間の価値」を意味します。この変数名は少し短く、読みやすさを重視したものです。ただし、「k」が千を意味することを知らない人には理解しづらい可能性もあります。

変数名は、それが何を表すかを一目で理解できるように、具体的で意味のある名前をつけることが重要です。
```

`10k`や`30k`という表現を私たちはとっさに思いつけるでしょうか？ChatGPTはコードの読みやすさも考慮してくれているようです。

## ChatGPTを使ってコミットメッセージを考える

今度はコミットメッセージを考えてもらいます。

消費税率を計算する関数を作成した場合を考えてみましょう。

次のようなプロンプトを与えました。

```md
消費税率を計算する関数を作成した場合のコミットメッセージを作成してください。
```

ChatGPTは次のようなレスポンスを返してくれました。

```md
コミットメッセージは、行った変更を他の開発者や将来のあなたが理解できるように、明瞭かつ具体的に記述することが重要です。以下に、消費税率を計算する関数を作成した場合のコミットメッセージの例を示します。

1. "Add function to calculate sales tax rate"
   - 「消費税率を計算する関数を追加」: 関数の追加だけを伝えています。

2. "Implement calculateSalesTax function"
   - 「calculateSalesTax関数を実装」: 関数の具体的な名前を明記しています。

3. "Create function for computing consumption tax"
   - 「消費税を計算する関数を作成」: 関数の役割を具体的に説明しています。

4. "Add taxCalculation function for computing sales tax rate"
   - 「sales tax rateを計算するためのtaxCalculation関数を追加」: 関数の名前と役割の両方を明示しています。

これらのメッセージは、何が行われたか（関数の追加）とその目的（消費税率の計算）を明示しています。なお、具体的な関数名を含めるかどうかは、その関数がプロジェクト内でどれほど重要か、または特定の規模のコミットでどれだけの変更が行われたかによります。
```

ご覧の通り、複数候補を提示してくれます。これにより0から1を考えるのではなく、1から10を考えることができます。この二つのストレスの差は大きいと思います。

# Name Genieで負担を軽減

しかし、変数を定義するたびにブラウザを開いてChatGPTに質問するのも面倒です。

そこで、「Name Genie」というVSCodeの拡張機能を作成しました。

https://marketplace.visualstudio.com/items?itemName=takagimeow.name-genie

Name Genieを使えば、VSCodeから離れることなく命名してもらうことが可能になります。

2023年5月19日現在、バージョン1.1.0では次の名前を生成することができます。

- 変数名
- 関数名
- クラス名
- インターフェース名
- ブランチ名
- コミットメッセージ
- プルリクエストタイトル
- ファイル名

そして、お好みに合わせて次の設定を行えます。

- ハイフン区切り
- 小文字で始める
- キャメルケース
- ケバブケース
- スネークケース
- パスカルケース

これらのオプションを組み合わせた上に、最大3つまで説明を与えることができます。もちろんこの説明は日本語で大丈夫です。

## 使う前の注意点

Name Genieを使うには、APIキーの設定が必要です。APIキーは[こちらのページ](https://platform.openai.com/account/api-keys)から発行してください。

https://platform.openai.com/account/api-keys

取得したキーは、VSCodeの設定画面から設定してください。

![VSCodeの設定](https://storage.googleapis.com/zenn-user-upload/4ea08d348747-20230519.png)

:::details Name Genieの設定を開く方法
VSCodeの設定を開いてください。検索用の入力フィールドに`name-genie`と入力すると、Name Genieの設定が表示されます。
:::

:::details モデルの設定方法
Name Genieでは、GPT-4とGPT-3のどちらのモデルでも選択できます。
上記の設定画面にて、Modelsと書かれたドロップダウンから、お好みのモデルを選択してください。

次のモデルを選択できます。

```md
- gpt-4
- gpt-3.5-turbo-0301
- gpt-3.5-turbo
```
:::

## 変数名を生成する

それでは変数名を生成してみましょう。今回は「飲んだ缶コーヒーの数」を表す変数を考えてみましょう。

1. 「作成したい名前」と書かれたドロップダウンから「**変数名**」を選択します。
2. 「プリセット」として「**キャメルケース**」を選択します。
3. そして「説明1」に「**飲んだ缶コーヒーの数**」を入力します。
4. 最後に「**Ask Genie**」をクリックします。

次のような結果が生成されました。

```md
- drankCanCoffeeCount
- consumedCanCoffeeNumber
- finishedCanCoffeeAmount
```

![Name Genieを使って変数名を生成](https://storage.googleapis.com/zenn-user-upload/60b627b357aa-20230519.png)

:::details 3つではなく、5つの候補を生成する方法
「説明2」の入力フィールドに「5つ候補を生成してください」と入力してください。

結果、5つの候補が生成されます。

```md
- drankCanCoffeeCount
- consumedCanCoffeeNumber
- finishedCanCoffeeAmount
- canCoffeeDrankTotal
- canCoffeeConsumedQuantity
```
:::

## 関数名を生成する

それでは関数名を生成してみましょう。今回は「１年間の投資の利益の計算」を表す関数名を考えてみましょう。

1. 「作成したい名前」と書かれたドロップダウンから「**関数名**」を選択します。
2. 「プリセット」として「**キャメルケース**」を選択します。
3. そして「説明1」に「**１年間の投資の利益の計算**」を入力します。
4. 最後に「**Ask Genie**」をクリックします。

次のような結果が生成されました。

```md
- calculateAnnualInvestmentProfit
- computeYearlyInvestmentGain
- getOneYearInvestmentEarnings
```

![Name Genieを使って関数名を生成](https://storage.googleapis.com/zenn-user-upload/191909dc687d-20230519.png)

## ブランチ名を生成する

今度はブランチ名を生成してみましょう。「アラートが表示されないバグを修正」を表すブランチ名を考えてみましょう。

1. 「作成したい名前」と書かれたドロップダウンから「**ブランチ名**」を選択します。
2. 「プリセット」として「**ハイフン区切り**」を選択します。
3. 「説明1」に「**アラートが表示されないバグを修正**」を入力します。
4. 「説明2」に「**次のフォーマットに従ってください。**」と入力します。
5. 「説明3」に「**`<type>/<subject>`**」を入力します。
6. 最後に「**Ask Genie**」をクリックします。

次のような結果が生成されました。

```md
- bugfix/alert-not-displaying
- fix/alert-display-issue
- bugfix/fix-alert-display
```

![Name Genieを使ってブランチ名を生成する](https://storage.googleapis.com/zenn-user-upload/144df8a8a5c5-20230519.png)

## コミットメッセージを生成する

最後にコミットメッセージを生成してみましょう。「アラートが表示されないバグを修正」を表すコミットメッセージを考えてみましょう。

1. 「作成したい名前」と書かれたドロップダウンから「**コミットメッセージ**」を選択します。
2. 「プリセット」として「**小文字で始める**」を選択します。
3. 「説明1」に「**アラートが表示されないバグを修正**」を入力します。
4. 「説明2」に「**次のフォーマットに従ってください。**」と入力します。
5. 「説明3」に「**`<type>[optional scope]: <subject>`**」を入力します。
6. 最後に「**Ask Genie**」をクリックします。

次のような結果が生成されました。

```md
fix(alert): no-alert-bug
fix(alert): alert-not-showing
fix(alert): alert-display-bug
```

![Name Genieを使ってコミットメッセージを生成](https://storage.googleapis.com/zenn-user-upload/08b29a20c4ee-20230519.png)

# まとめ

開発者は自分の得意なことに集中すべきです。これもしなきゃ、あれもしなきゃと考えることで脳のメモリを圧迫してしまいます。英語のせいで開発者体験が悪くなるのは絶対に避けるべきだと思います。

ChatGPT、そしてName Genieは開発者にとってのパワードスーツとして働いてくれます。苦手な英語はAIに任せて、開発者はコーディングに集中しましょう。

これこそがまさに「**AIによる開発者体験の向上**」だと私は思います。

# 参考

https://marketplace.visualstudio.com/items?itemName=takagimeow.name-genie

https://github.com/takagimeow/name-genie

https://gist.github.com/minop1205/5fc4f6ef0ec89fb1738833ba25ae00a0

https://newspicks.com/movie-series/14?movieId=2603