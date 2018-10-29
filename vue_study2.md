# 第2回vue勉強会

## 今日の目標  「コンポーネント指向について学ぼう」

---
###### 項目
1.  基本文法の続き
2. 「コンポーネント」とは
3.  コンポーネント間での通信
---

#### はじめに

今日は前回の続きの文法編とコンポーネントについて学習します。    
文法編では新たなプロパティが出てきたり、コンポーネント指向、コンポーネント間での通信の仕方などを学んでいきます。ちょっと今日の学習するものを少しハードルが高いかもしれませんが、「こんなこともできるんだ」と軽く理解する程度で大丈夫です。  
ここで教えている僕もちょっと理解が難しく、先週自分で「Vue.jsはわかりやすい！」的なことを言っていたのを思い出して、土日すごく焦りながら学習していました。    
それでは基本文法の続きをを軽く紹介していきます。

#### 基本文法

今回もオンラインエディタを用意しましたのでお使いください。   
先週作っていただいた、自分のテンプレートを使っていただいても構いません。

今日の新たに紹介するプロパティは「算出プロパティ」です。  
算出プロパティとは、任意の処理を使ったデータです。
算出プロパティは任意のデータを返す関数を`computed`に登録し使います。   

例として、次のコードをみていきましょう。
`data`プロパティに登録されている`width`,`computed`プロパティに登録している`halfWidth`が使われています。

⇩オンラインエディタを利用する人はこちら⇩
>[「jsfiddleで実行」](https://jsfiddle.net/kusaoisii/og1ajsLr/1/)

```html
<p>{{ width }} の半分は {{ halfWidth }}</p>
```
算出プロパティは関数の戻り値をデータとして持っています。

```js
new Vue({
  el: '#app',
  data: {
    width: 800
  },
  computed: {
    // 算出プロパティhalfWidthを定義
    halfWidth: function() {
      return this.width / 2
    }
  }
})
```
実行結果

![実行結果](https://i.imgur.com/l1a6Pbk.png)




算出プロパティを組み合わて新たな算出プロパティを作ることもできます。

⇩オンラインエディタを利用する人はこちら⇩
>[「jsfiddleで実行」](https://jsfiddle.net/kusaoisii/p3atznuw/1/)


```html
<p>X: {{ halfPoint.x }}</p>
<p>Y: {{ halfPoint.y }}</p>
```
次の算出プロパティ`halPoint`は、別に定義した算出プロパティ`halfWidth`と`halfHeight`を使い、`width`と`height`の中心ポイントを値にします。
```js
new Vue({
  el: '#app',
  data: {
    width: 800,
    height: 600
  },
  computed: {
    halfWidth: function() {
      return this.width / 2
    },
    halfHeight: function() {
      return this.height / 2
    },
    // 「width × height」の中心座標をオブジェクトで返す
    halfPoint: function() {
      return {
        x: this.halfWidth,
        y: this.halfHeight
      }
    }
  }
})
```
実行結果

![実行結果](https://i.imgur.com/mBgDcjw.png)

算出プロパティはキャッシュ機能を持っている

算出プロパティとメソッドは用途が似ているように見えます。  
例でみてみましょう。

[「jsfiddleで実行」](https://jsfiddle.net/kusaoisii/08j5kub7/3/)

```html
<p>算出プロパティ</p>
<ol>
  <li>{{ computedData }}</li>
  <li>{{ computedData }}</li>
</ol>
<p>メソッド</p>
<ol>
  <li>{{ methodsData() }}</li>
  <li>{{ methodsData() }}</li>
</ol>
```
実行してみると、`Math.random()`は何度使用しても同じ数字が返されます。
このキャッシュ機能によって、算出プロパティは元のデータに変更があるまで、何度使用しても関数内の処理は一度しか行われません。   
そのため、複雑な処理に使われていることが多いです。
```js
new Vue({
  el: '#app',
  computed: {
    computedData: function() { return Math.random() }
  },
  methods: {
    methodsData: function() { return Math.random() }
  }
})
```
実行結果

![実行結果](https://i.imgur.com/JYwid2u.png)

フィルタでテキストの変換を行う    

『フィルタ』とは、数字にカンマを入れると言った、テキストの変換処理に特化した機能です。

フィルタの使い方

登録したフィルタは、Mustacheにバイブ「｜」でフィルタ名を繋げることで呼び出します。
`filter`メソッドの第１引数としてデータが渡されます.

```html
{{ 対象のデータ | フィルタの名前 }}
```

実際に使っている例をみてみましょう。

[jsfiddleで実行」](https://jsfiddle.net/kusaoisii/zL0hxto1/4/)

```html
<!-- priceはconverNumの第一引数 -->
<p>{{ price | converNum }}</p>
```

```js
new Vue({
  el: '#app',
  data: {
    price: 19800
  },
  filters: {
    localeNum: function (val) {
      return val.toLocaleString()
    }
  }
})
```
実行結果

![実行結果](https://i.imgur.com/zCzDE6e.png)

フィルタに第2〜引数を持たせることもできます。  
下のの例では第一引数として`message`,第二引数として`foo`が受け取れます。


```html
<p>{{ message | filter(foo)}}</p>
```

複数のフィルタをチェーンのように繋げてて、扱うこともでいます。  
この例では、`radian`フィルターの結果の結果を引数とし`round`を呼び出します。  

[「jsfiddleで実行」](https://jsfiddle.net/kusaoisii/gc0w49qe/2/)
```html
<p>180 度は {{ 180 | radian | round }} ラジアンだよ</p>
```
```js
new Vue({
  el: '#app',
  filters: {
    // 小数点以下を第2位に丸めるフィルタ
    round: function (val) {
      return Math.round(val * 100) / 100
    },
    // 度からラジアンに変換するフィルタ
    radian: function (val) {
      return val * Math.PI / 180
    }
  }
})
```
![実行結果](https://i.imgur.com/VrPTUOC.png)

他にもたくさんの機能をvue.jsは持っています。   
気になった方は技術書の購入をお勧めします。    

次は今日の本題「コンポーネント」のお話をします。

#### 「コンポーネントとは」

みなさんがいつも使っている、ウェブサイト、アプリケーションの画面は、いろんな機能を持った部品でできていると思います。実際に今みなさんが使っているGitHubのページでは自分のアカウント情報がみれる機能をもつ部品、検索ができる部品など、様々な機能を持った部品でページができていると思います。大規模なアプリケーションではソースコードが複雑になりやすく、後からメンテナンスをしようとした時、メンテナンスをしたいソースコードを探すのは大変そうですよね。  

![汚い部屋](https://lohas.nicoseiga.jp/thumb/5359231i?https://lohas.nicoseiga.jp/thumb/5359231i?)   
↑いろんなものが散らかってる部屋より、本は本棚、服は服用のタンスにあったほうが、あとあと探しやすいよねってことを伝えたかった画像。

そこで、Vue.jsの大きな機能の一つである「コンポーネント」は、機能を持つUI部品ごとに管理し、より開発を捗らせようという仕組みです。

コンポーネントはUI部品(機能を持つHTML)の設計図です。そして、この設計図から作成された実態のことを「インスタンス」と言います。
それぞれのインスタンスは元の設計図が一緒であれば基本的な動作は同じですが、プロパティに変化をつけることで、それぞれ違った細かな動作を指定できます。

コンポーネントの再利用

仕様が同じにも関わらず別個のコンポーネントとして作ってしまうと、機能の追加や仕様の変更があった時全てのコンポーネントを修正することになります。そうすると、作業コストが発生してしまう可能性がでできますし、一部だけ更新をし忘れるなど無駄なことが増えてしまいます。    
そこで、コンポーネントを部品ごとに切り離して管理することで「再利用」が容易にできるというのもメリットの一つです。    

それでは、実際に簡単なコンポーネントを作りましょう。  