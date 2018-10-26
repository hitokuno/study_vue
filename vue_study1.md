# 第一回vue勉強会

## 今日の目標  「Vue.jsを知って、触れてみよう」


-----------
###### 項目
1. [vue.jsの紹介](https://gist.github.com/kusaoisii/af5395041d7508152a2053099446e6ac#vuejs%E3%81%A8%E3%81%AF)

2. [環境について](https://gist.github.com/kusaoisii/af5395041d7508152a2053099446e6ac#%E7%92%B0%E5%A2%83%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6)

3. [基本文法、機能](https://gist.github.com/kusaoisii/af5395041d7508152a2053099446e6ac#%E5%9F%BA%E6%9C%AC%E6%96%87%E6%B3%95%E6%A9%9F%E8%83%BD)
-----------

##### はじめに...

フロントの知識がほぼ0に近い状態で、Vue.jsに触れて勉強している人間の観点からこの発表を進められたらなと思います！  
1年生でも学習しやすいように細かな説明、引用,コード説明など多めに載せました。      
この場が僕と皆のスキルアップの場になるよう頑張ります！！  
今の所、4~5回に別けてこの勉強会を開く予定です！

#### Vue.jsとは
フロントで使われているJavascirpt(js)のフレームワークです。
フレームワークはあらかじめ機能を持ったコードを書くことで効率的に開発を進めることを可能にします。    
まず始めに、各言語ごとのフロントエンドの役割を確認しましょう。   
htmlが基盤となる構造を作り、cssで装飾,Javascirptで動きがつけられます。

![フロントエンドの役割](https://fastcoding.jp/blog/wp-content/uploads/2017/03/html_css_js_08.png)

そして今日は、この「動き」の役割をもつjsのフレームワーク、Vue.js
について学んで行きたいと思います。     
主なVue.jsの特徴として次のようなものがあげられます。

- コードのシンプルさ
- 公式ドキュメント(日本語)の充実性
- 情報量の多さ    

まず、Vue.jsの特徴として,コードがとてもシンプルに作られていているため、学習コストを抑えられます。実際にあまり経験のない僕も触れやすいなと感じました。さらにVue.jsは日本語の[公式ドキュメント](https://jp.vuejs.org/v2/guide/)を公開しており、学習のしやすさもメリットとして挙げられます。また、Vue.jsは人気なフレームワークなだけに,情報(記事、ブログ)が多いです。これらの点から、初心者の方の入口としてはとても良いフレームワークだと感じました。   
他の特徴として、再利用しやすいコンポーネントによるUIの設計、レンダリングの速さがあげられます。この速さの一つの理由として、いちいちDOMを一から生成することなく、仮装DOMと元のDOMの差によって新しいDOMを生成することにより、高速なレンダリングを可能としています。

DOMはWebページの情報を受け渡ししてくれる役割を持ち、ページを見るたびに生成されます(以下の図のように)。そして,このDOMを生成する速さがレンダリング(webページの表示)の速さに繋がっています。

![](https://camo.qiitausercontent.com/08797ec19dd8dee3649e27b18b1556c645fd7a7b/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f3134313135302f34343334313537332d653738342d666232392d636239312d6538336365333263613230342e706e67)

なぜ仮想DOMを利用すると早いのか...   
仮想DOMは作るのに時間がDOMほどかからないのではやい。   
次に行われる新しいDOMの再構築も、一部分だけなのではやい。(差分を取り、そこだけ成形)   
そのふたつの処理がはやく終わるおかげで表示速度がはやいというわけです。


![](https://camo.qiitausercontent.com/2c6e354b462444336edbc3c5838fdab6a9788eef/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f3134313135302f30633866663935312d363730322d306537392d663432302d3462323430633061323335362e706e67)

>([参考記事](https://qiita.com/risagon/items/019942c60e9c3e6c05a5))

このようにVue.jsの利用には様々なメリットがあるので、気になった方は触れてみてください！


次に学習するための環境について説明します...！

#### 環境について

ブラウザサポートについて   
Vue.jsはモダンブラウザ(IE9以降)のみのサポートとなっており,レガシーブラウザはサポートを行なっておりません。
モダンブラウザのみサポートすることで、最新のjsの機能をフルに活用することで高速な動作を実現しています。

また、エディタの指定などはありません。

スクリプトタグ`<script>`で読み込むだけの、スタンドアロン版 の Vue.js を使用します。
今日学習予定の範囲では特に環境構築の設定はいりません。  
まずはVue勉強用のディレクトリを作ります。  
そしてファイルの構成ですが、今日使うファイルは  

htmlファイル       -index.html     
Javascirptファイル  -main.js    

のみとなります。
実行の仕方はターミナルで'open index.html'です。   
また「そんなの作るのめんどくさい！」と言う方にオンラインエディタ[「jsfiddle」](https://jsfiddle.net/kusaoisii/8f6yk23h/12/)を用意しました！
を用意しました！

それでは早速、文法の学習を進めて行きましょう！
#### 基本文法、機能

まず、templateとして次のファイルを作ってください。
オンラインエディタを利用する場合はurlを載せてときましたので、そちらから飛んで利用してください。(多分、jsfiddleサイトの登録は不要です。)

まず、htmlのテンプレートからファイルを作ります。
以下のコードをコピーして使ってください。

```html
<!--htmlファイルのテンプレ-->
<!--index.html-->
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="utf-8">
  <title>Vue.js App</title>
  <link href="main.css" rel="stylesheet">
</head>
<body>
  <div id="app">
    <!-- この#appの内側にテンプレートを書き込んでいく -->
  </div>
  <!--CDNを利用してVue.jsを読み込む-->
  <script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
  <!--main.jsを読み込む-->
  <script src="main.js"></script>
</body>
</html>
```
ここでは初期設定をし、2つの`<script>`では1つ目はCDNによってインターネットが繋がっていれば,Vue.jsを利用でき、2つ目では次に作成してもらう`main.js`ファイルを読み込んでいます。  
(`main.css`ファイルはまだ使わないので作らなくても大丈夫です。)

次に`main.js`のテンプレートをコピーしファイルを作ってください。

ここではコンストラクタ関数`Vue`を使ってルートとなるVueインスタンスを作成します(`new Vue`のところ)。


```js
//main.jsのテンプレ
var app = new Vue({
  //ここに記入して行きます。
})
```

では,実際にVue.jsを体験して行きましょう！


###### データのバインディング

⇩オンラインエディタを利用する人はこちら⇩
>[「jsfiddleで実行」](https://jsfiddle.net/kusaoisii/8f6yk23h/12/)

以下のコードをhtmlの`<div id="app">`と`</div>`の間に追加してください。  
`<p>`要素はテキストの段落を表します。   
この、要素では`main.js`の`data`プロパティで登録されている`message`を{{}}で囲むことで表示しています。

```html
<p>{{ message }}</p>
```

`el`プロパティは、Vueアプリーケーションの範囲、Vueの管轄領域を表します。  
今回の例では、`<div id="app">` タグ内部がVueアプリケーションとして認識されるということです。
また,`data`プロパティには使用するデータを登録できます。

```js
var app = new Vue({
  el: '#app',   //elオプション には、id、もしくはひとつしかないclassを指定しないといけない。
  data: {
    message: 'Hello Vue.js!'
  }
})
```
実行結果
![実行結果](https://i.imgur.com/m13WuPY.png)

###### 繰り返し、リスト

`v-for`をを回してリストを表示できます。

>[「jsfiddleで実行」](https://jsfiddle.net/kusaoisii/7w3ouex8/)

まず,順序をつけてリストを表示するのに`ol`要素を作ります。        
`<li v-for="item in list">`ではループ処理が行われています.    
`v-for`はループ処理に使われ,dataプロパティで指定された`list`を回しています。


```html
<ol>
  <li v-for="item in list">{{ item }}</li>
</ol>
```
`v-for`の部分は以下のように構成されています   
`<li v-for="各要素を代入する変数名 in 繰り返したい配列">`

配列インデックスを任意で受ける書き方は以下のようになります。    
`<li v-for="(item,index) in list">...</li>`

以上のhtmlを通常のhtmlで書くと...こんな感じ
```html
<ol>
  <li>りんご</li>
  <li>バナナ</li>
  <li>いちご</li>
</ol>
```

このようにVue.jsはhtmlに直接コードを書き込むので,親しみやすい！

また、dataプロパティにリストを登録するときは以下のようにやります。

```js
var app = new Vue({
  el: '#app',
  data: {
    list: ['りんご', 'ばなな', 'いちご']
  }
})
```
実行結果
![実行結果](https://i.imgur.com/IQAW5LI.png)

また、インスタンスを変数化することでコンソールからアクセスが可能になり、リストの追加、削除などができます！詳しくは以下のサイトで確認してください。  
[「リストの変形の仕方はこちら」](https://v1-jp.vuejs.org/guide/list.html#%E9%85%8D%E5%88%97%E3%81%AE%E5%A4%89%E5%8C%96%E3%82%92%E6%A4%9C%E5%87%BA)



###### イベント、条件分岐
イベントをバインドするには`v-on`ディレクティブを利用します。  
ここではバインドされたイベントをクリックすると発火させています。    
また`v-if`で条件分岐ができます。`v-else`,`v-else-if`の使用も可能です。

以下のコードでは、`v-on:click="show=!show`でクリックするたびに`show`が`true`,`false`が入れ替わり代入されています。      
次に`v-if="show"`で条件分岐をしています。              

>[「jsfiddleで実行](https://jsfiddle.net/kusaoisii/rz2fac6y/)

```html
<button v-on:click="show=!show">切り替え</button>
<p v-if="show">Hello Vue.js!</p>
```
```js
var app = new Vue({
  el: '#app',
  data: {
    show: true
  }
})
```
実行結果
![実行結果](https://i.imgur.com/o89wF6e.png)

###### オブジェクトや配列要素の表示
指定されたオブジェクトや配列要素の表示の仕方について

>[「jsfiddleで実行」](https://jsfiddle.net/kusaoisii/zh965mfo/1/)

以下の`main.js`ではオブジェクトやリストが登録されています。  
あるオブジェクトのプロパティを表示したい場合は
`[オブジェクト名].[プロパティ名]`でプロパティを表せます。   

```html
<!-- 1 オブジェクトのプロパティを表示 -->
<p>{{ message.value }}</p>
<!-- 2 文字列の長さを表示 -->
<p>{{ message.value.length }}</p>
<!-- 3 リストのインデックス2を表示 -->
<p>{{ list[2] }}</p>
<!-- 4 プロパティを組み合わせて使用 -->
<p>{{ list[num] }}</p>
```
```js
new Vue({
  el: '#app',
  data: {
    // オブジェクトデータ
    message: {
      value: 'Hello Vue.js!'
    },
    // 配列データ 3 と 4 で使用
    list: ['りんご', 'ばなな', 'いちご'],
    // 別のデータを使用してlistから取り出す要素を動的に 4 で使用
    num: 1
  }
})
```
実行結果
![実行結果](https://i.imgur.com/rm0gEoS.png)

###### クリックイベント
先ほど少し触れましたが、`v-on`でmethodをバインドされています。
methodの宣言はVueオブジェクトのmethods{}でします。  

>[「jsfiddleで実行」](https://jsfiddle.net/kusaoisii/f3x4u5L0/4/)

```html
<div id="app">
  <!-- countプロパティを表示する -->
  <p>{{ count }}回クリックしたよ！ </p>
  <!-- このボタンをクリックするとincrementメソッドが呼び出される -->
  <button v-on:click="increment">カウントを増やす</button>
  <!-- v-onは"@"と書きえ、省略することができます。 -->
</div>
```
```js
new Vue({
  el: '#app',
  data: {
    count: 0
  },
  methods: {
    // ボタンをクリックしたときのハンドラ
    increment: function () {
      this.count += 1 // 処理は再代入するだけでOK！
    }
  }
})
```
実行結果
![実行結果](https://i.imgur.com/4Vd5sHe.png)

また、`v-on`は省略が可能で`@`で省略が可能です。他にも省略できるディレクティブがあるらしいですが、ここでは割愛させていただきます。きになる人は下のリンクから調べてみてください。
>[省略記法についてはこちら](https://v1-jp.vuejs.org/guide/syntax.html#%E7%9C%81%E7%95%A5%E8%A8%98%E6%B3%95)
##### フォーム入力バインディング

###### フォーム入力の取得

`<input>`はテキスト入力欄によく使われえる要素です。    


>[「jsfiddleで実行」](https://jsfiddle.net/kusaoisii/ca0v2e67/3/)  

```html
<input v-bind:value="message" v-on:change="handleInput">
<pre>{{ message }}</pre>
```
以上で`v-bind`が使われていまね   
`v-bind:value="message"`ではDOM要素(value)に`massage`をデータバインディングすることで、データの変更をするたびに、バインドされたDOM要素が更新されます。

```js
new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue.js',
  },
  methods: {
    handleInput: function (event) {
      // 代入前に何か処理を行う…
      this.message = event.target.value
    }
  }
})
```
`handleInput`では入力データを`message`に代入しています。

実行結果
![実行結果](https://i.imgur.com/rU3sZzN.png)

この二つの機能を合わせもつ`v-model`を次紹介します。
>[「v-bind公式ドキュメント」](https://jp.vuejs.org/v2/api/#v-bind)

###### 複数行テキスト

こちらでは、htmlの`<textarea>`要素を利用します。
フォームの入力値と選択値をデータと同期させる、「双方向データバインディング」を行うには、`v-model`ディレクティブを使用します。
`v-model`は先ほど説明したDOM要素へのデータバインディングと、要素から取得したデータをリアクティブデータに反映させるという２つの処理を自動化する構文です。

>[「jsfiddleで実行」](https://jsfiddle.net/kusaoisii/tpemgz7d/)

```html
<textarea v-model="message"></textarea>
<pre>{{ message }}</pre>
```

```js
new Vue({
  el: '#app',
  data: {
    message: 'Hello!'
  }
})
```
実行結果
![実行結果](https://i.imgur.com/NkYSR2P.png)
