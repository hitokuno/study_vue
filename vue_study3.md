# 第3回Vue.js勉強会

## 今日の目標「.Vueファイルに触れてみよう」

---
項目
1. トランジションに触れてみる

---

#### トランジションとは

Vue.jsの「トランジション」はcssトランジション/アニメーションをより簡単に使いやすくサポートする機能です。要素がDOMに追加されたり消去、更新されたとき、Vue.jsは自動的にCSSトランジション/アニメーションのためのクラスを付与して、スタイルを適用します。

実際に動かした方がわかりやすく、楽しいと思うので動かしてみましょう.

トランジション効果を適用したい要素を`<trransition>`タグで囲むだけでトランジション用のクラスが使用できるようになります。
```html
<div id="app">
  <p><button v-on:click="show=!show">切り替え</button></p>
  <transition>
    <div v-if="show">
      トランジションさせたい要素
    </div>
  </transition>
</div>
```

```js
new Vue({
  el: '#app',
  data: {
    show: true
  }
})
```
cssファイルではトランジションクラスにスタイルを定義すると、Vue.jsが`<div>`要素の追加と消去のタイミングでクラスを動的に操作し、トランジションが適用されます。
下のcssファイルのコードの`v-`以下については詳しく説明します。
`
```css
/* 1秒かけて透明度を遷移 */
.v-enter-active, .v-leave-active {
  transition: opacity 1s;
}
/* 見えなくなるときの透明度 */
.v-enter, .v-leave-to {
  opacity: 0;
}
```

ここで先ほどの謎のコード`.v-enter`や` .v-leave`などついて説明していきます。   
動くものを定義するには最初の状態,そして動いた後の状態を定義すれば良さそうですよね。  
先ほど動かしたコードを見ていきながら解説していきます。

```html
<div id="app">
  <p><button v-on:click="show=!show">切り替え</button></p>
  <transition>
    <div v-if="show">
      トランジションさせたい要素
    </div>
  </transition>
</div>
```
```js
new Vue({
  el: '#app',
  data: {
    show: true
  }
})
```

```css
.v-enter-active, .v-leave-active{
  transition: opacity 1s;
}
.v-enter, .v-leave-to{
  opacity:0;
}
```

このcssコードはちょっと省略があるので省略なしのコードが以下のようになります。  

```css
.v-enter-active, .v-leave-active{
  /*変化の間隔を定義*/
  transition: opacity 1s;
}

/*表示されるときは0から１*/

.v-enter{
  /*透明度を指定(無指定は1)*/
  opacity:0;
}
.v-enter-to{
  opacity:1;
}

/*消えるときは1から０*/

.v-leave{
  opacity:1;
}
.v-leave-to{
  opacity:0;
}

```
実行してみればわかると思うのですが、`button`で`show`が入れ替わり、`v-if`で条件分岐されていて`show`が`true`の時は`enter`の`opacity`が0から１に変化して、`false`の時は`leave`の1から０に変化しています。

ちょっと細かく説明するとEnter系は対象要素(ここでは`<div>`)がDOMに挿入されるときのフェーズ、Learve系のクラスは逆の対象要素がDOMから消去されるときのフェーズです。  
詳しく知りたい人は読んでみてください。  
以下は各フェーズの説明です。([公式から引用](https://jp.vuejs.org/v2/guide/transitions.html#%E3%83%88%E3%83%A9%E3%83%B3%E3%82%B8%E3%82%B7%E3%83%A7%E3%83%B3%E3%82%AF%E3%83%A9%E3%82%B9))

---
1. v-enter: enter の開始状態。要素が挿入される前に適用され、要素が挿入された 1 フレーム後に削除されます。
2. v-enter-active: enter の活性状態。トランジションに入るフェーズ中に適用されます。要素が挿入される前に追加され、トランジション/アニメーションが終了すると削除されます。このクラスは、トランジションの開始に対して、期間、遅延、およびイージングカーブを定義するために使用できます。
3. v-enter-to: バージョン 2.1.8 以降でのみ利用可能です。 enter の終了状態です。要素が挿入後 (同時に v-enter が削除されます) 1 フレームが追加されます。トランジション/アニメーションが終了すると削除されます。
4. v-leave: leave の開始状態。トランジションの終了がトリガされるとき、直ちに追加され、1フレーム後に削除されます。
5. v-leave-active: leave の活性状態。トランジションが終わるフェーズ中に適用されます。leave トランジションがトリガされるとき、直ちに追加され、トランジション/アニメーションが終了すると削除されます。このクラスは、トランジションの終了に対して、期間、遅延、およびイージングカーブを定義するために使用できます。
6. v-leave-to: バージョン 2.1.8 以降でのみ利用可能です。 leave の終了状態です。トランジションの終了がトリガされた後 (同時に v-leave が削除されます) 1 フレームが追加されます。トランジション/アニメーションが終了すると削除されます
---

簡単に説明すると、要素が追加されたときは`v-enter`から`v-enter-to`、消去されたときは`v-leave`から`v-leave-to`に動きます。
`v-enter-active`と`v-leave-activve`には全体を通して必要な`transition`や`position`を定義されます。  

図をみてみると分かりやすいですね。


![図](https://i.imgur.com/rD2jlKL.png)

以上のように、要素に付与される各トランジション名は、デフォルトで`v-`と言うプレフィックスがつきます。このプレフィックスは、`<transition>`タグに`name`属性で宣言することで任意に変更できます。

```html
<!-- トランジションタグにname属性で`demo`をつける -->
<transition name="demo">
  <div v-if="show">トランジションさせたい要素</div>
</transition>
```
```css
/*クラスのプレフィックスが[demo-]になる*/
.demo-enter-active, .demo-leave-active{
  transition: opacity 1s;
}
.demo-enter, .demo-leave-to{
  opacity:0;
}
```

初期描画時にもトランジションを行うには
`<transition>`タグに`appear`属性をつけることで、初期描写時にもトランジションが行えます。   
[「jsfiddleで実行」](https://jsfiddle.net/kusaoisii/kz9op2cd/5/)

```html
<transition appear>
  <div v-if="show">example</div>
</transition>
```
