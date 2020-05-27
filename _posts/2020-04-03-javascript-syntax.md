---
title: "JavaScript勉強の記録"
excerpt: "個人でJavaScript勉強したときに理解したことを書き足すポスト"
toc: true
toc_sticky: true
toc_label: "JavaScript"

categories:
  - エンジニア勉強
tags:
  - javascript
  - 基礎
  - 初心者
  - ES6
last_modified_at: 2020-04-30T17:23:00
---

# attributeとpropertyの違い
※2つとも属性の意味

## `attribute`
- HTML Documentのelementsに属性（追加の情報）を与えるために使われる。

- 最初から与えられる値

- class、data-*(dataset)など

## `property`
- HTML DOMの中でattributeを操作するために使われる

- key:value式で扱える

- DOMツリー内で動的に変えられる

## 違いについて
- `attribute`

  - HTML Documentの中で存在する

  - 静的で変化しない属性、値

  - HTMLのドキュメント自体が持つ属性

- `property`

  - HTML DOMの中に存在する（DOM APIで扱う）

  - 動的に変化し得る属性、値

  - DOMで動的に扱える属性

[attribute 와 property 의 차이](https://medium.com/hexlant/attribute-%EC%99%80-property-%EC%9D%98-%EC%B0%A8%EC%9D%B4-c6f1c91ba91)

[Property,Attribute 프로퍼티,어트리뷰트, 속성](http://www.ktword.co.kr/abbr_view.php?nav=2&m_temp1=3967&id=877)

[HTML 특성 참고서](https://developer.mozilla.org/ko/docs/Web/HTML/Attributes)

[HTML 속성](https://ko.wikipedia.org/wiki/HTML_%EC%86%8D%EC%84%B1)

[HTML Attributes](https://www.w3schools.com/html/html_attributes.asp)

# HTML,DOMの違い
ブラウザでレンダーリングされる順番

```markdown
                           DOM
                            ↓
HTML  →  HTMLパーサー  →   DOMツリー       配置
                            ↓           ↑
                       アタッチメント  →  renderツリー  → render  → 表示
                            ↑
StyleSheets→CSSパーサー→スタイル規則
```

1. ユーザがリクエストを送る

2. レスポンスで受け取ったHTMLファイルをパーシングしてDOMツリーを作る

3. CSSファイルをパーシングしてDOMツリーと結合する

4. レンダーリングする

5. ブラウザの画面に表示する

## HTML
- 文章の内容、構造、書式をmarkup情報として表現する言語

- 人が理解できて、区別できるように作られた

- 機械は理解できないため、ブラウザが解釈できる言語や構造に変換するパーシング作業が必要

- 各ブラウザごとにパーシングを行うパーサーが異なるため、同じHTMLでも違う結果値になる可能性がある

[HTML](https://namu.wiki/w/HTML)

[HTML - MDN](https://developer.mozilla.org/ko/docs/Web/HTML)

[HTML - wikipedia](https://ko.wikipedia.org/wiki/HTML)

[HTML 기초](http://tcpschool.com/html/html_intro_basic)

## Document
- ブラウザに読み込まれたウェブページ

- DOMツリー（ページコンテンツ）のエンドポイント（진입점）の役割をする

- ページのURL取得やドキュメント内に新しい要素を生成するなどの機能をグローバルに提供する

- あらゆるコンテンツタイプの共通プロパティやメソッドを提供する

[Document - MDN](https://developer.mozilla.org/ko/docs/Web/API/Document)

## DOM
- パーシングされたHTMLのツリー構造のオブジェクト

- ドキュメントにアクセスできるAPI

- node,property,methodを持つオブジェクト

- ブラウザに見えるものとは限らない

  - ブラウザに見えるものはrenderツリー（DOMとCSSの組み合わせ）であり、スクリーンに描画されるものだけに構成されるため、DOMとは違う

- Devtoolsに見えるとは限らない

  - 疑似要素は含まれない

  - ::beforeとか::afterのようなもの

[HTML,DOM간단정리](https://velog.io/@ddinggu/HTML-DOM-%EA%B0%84%EB%8B%A8-%EC%A0%95%EB%A6%AC)

[(번역)DOM은 정확히 무엇일까?](https://wit.nts-corp.com/2019/02/14/5522)

[DOM소개 - MDN](https://developer.mozilla.org/ko/docs/Web/API/Document_Object_Model/%EC%86%8C%EA%B0%9C)

[8/ DOM(1) - 노드의 계층 구조(1)](https://feel5ny.github.io/2017/12/26/JS_08_1/)

# Parsing
[Parsing(파싱)이란? Parser(파서)란?](https://na27.tistory.com/230)

[구문 분석 - Wikipedia](https://ko.wikipedia.org/wiki/%EA%B5%AC%EB%AC%B8_%EB%B6%84%EC%84%9D)

# ES5、ES6での違い

## テンプレートリテラル

### ES5
- 特殊文字のエスケープや連結演算子の記述が必要
```javascript
var lastName = "hoge";
var firstName = 'fuga';
console.log("My name is" + lastName + " " + firstName);
```

### ES6
- \`hoge\` で囲むことでテンプレートリテラルが使える

- テンプレートリテラル内で`${variable}`を使うことで変数をそのまま使える
```javascript
var lastName = "hoge";
var firstName = 'fuga';
console.log(`My name is ${lastName} ${firstName}`);
```

## アロー関数

### ES5
関数宣言に3種類がある

#### function命令
```javascript
function str(arg1, arg2) {
  console.log(arg1);
}
```

#### Functionコンストラクター
```javascript
var str = new Function("arg1", "arg2", "console.log(arg1)")
```

#### 関数リテラル（匿名関数を利用）
```javascript
var str = function(arg1, arg2) {
  console.log(arg1));
}
```

### ES6
アロー関数を使うことで関数リテラルをシンプルに書ける
```javascript
//引数が1つの場合
var str = arg1 => console.log(arg1);

// 引数が2つ以上の場合
var str = (arg1, arg2) => {
  console.log(arg1);
}

//オブジェクトリテラルを返す場合
var str = func => ({ id: '8' });
```

アロー関数を2回以上書く場合はカリー化して書いていることと同じ
```javascript
//ES6
const foo = x => y => x + y;

//↓

//ES5
var foo = function con(x) {
  return function (y) {
    return x + y;
  };
};

// 実行時
foo(1)(2); // 3
```

[JavaScript - ES6の文法でわからないところが｜teratail](https://teratail.com/questions/71006)

[[JavaScript] Vuex の中での foo => bar => foo + bar みたいなアロー関数2回続く場合の表記に慣れる。 « きんくまデザイン](http://www.kuma-de.com/blog/2017-07-06/7329)

## thisの挙動

### ES5

#### メソッド呼び出し
オブジェクトのメソッドで`this`は呼び出し元となるオブジェクトを指す
```javascript
var obj = {
  value: 10,
  show: function() {
    console.log(this.value); // 10
  }
}
obj.show();
```

#### 関数呼び出し
関数で呼び出すと`this`はグローバルを指す
```javascript
function func() {
  var value = 2;
  console.log(this.value); // undefined
}
func();
```
何か参照させたい場合は、関数外で`var value = 1;`などのように書く

#### コンストラクター呼び出し
コンストラクターでインスタンス化した場合、`this`は生成されたインスタンスを指す
```javascript
function Obj(val) {
  this.value = val;
}
var obj = Obj(0);
console.log(obj.value); // 0
```

#### apply,call,bind
`call`、`apply`メソッドを使うと第1引数の値を`this`にすることができる

※他のオブジェクトが持つメソッドをさぞ自分のメソッドかのように呼び出すメソッド
```javascript
var obj = {
  value: 1,
  show: function() {
    console.log(this.value);
  }
};
var newObj = {
  value: 5
};
obj.show(); // 1
obj.show.call(newObj); // 5
obj.show.apply(newObj); // 5
```
第2引数以降は関数に渡される引数

`call`は引数の順番通りに連続した引数のリストを渡す

`apply`は引数の配列を1つだけ渡す

[JavaScript の call( ) メソッドを雑に説明 - Qiita](https://qiita.com/Chrowa3/items/b3e2961c4930abc1369b)

[Function.prototype.call() - JavaScript \| MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Function/call)

[Function.prototype.apply() - JavaScript \| MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)

`bind`の場合、新しい関数を生成し、呼び出されたときの引数を`this`に設定した状態で処理をする
```javascript
var newFunc = obj.show.bind(newObj);
newFunc();
```

[Function.prototype.bind() - JavaScript \| MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)

※どんなときに使う関数なのかまだピンとこない

### ES6

#### アロー関数
関数が宣言された時のスコープに`this`が自動的に当たる
```javascript
var obj = {
  value: 10,

  //メソッド呼び出し
  show: function() {
    console.log(this.value); // 10

    // 関数呼び出し
    function show_01() {
        console.log(this.value); // undefined
    }
    show_01();

    // アロー関数
    var show_02 = () => {
        console.log(this.value); // 10 thisはobjを参照
    }
    show_02();
  }
}
obj.show();
```

## 変数の宣言

### ES5
`var`で宣言していた

そして、グローバル汚染問題も多々起きていた

### ES6
`var`,`let`,`const`の3つのシンタックス

`let`:変数の再宣言ができない、再代入はできる

`const`:変数の再宣言/再代入ができない

※`const`の場合、オブジェクトや配列を宣言した場合、**参照した物自体**は差し替えできないが、**参照の中身**は差し替えられる
```javascript
var array1 = () => {
  const arrayFalse = [1,2,3];
  arrayFalse = [4,5,6]; //Error
  console.log(arrayFalse);
}
array1(); //Error

var array2 = () => {
  const arrayTrue = [1,2,3];
  arrayTrue[1] = 10;
  console.log(arrayTrue);
}
array2(); //[1,10,3]
```

## 変数のブロックスコープ

### ES6
`let`, `const`をifやforのような{}に囲ったブロック内で宣言した場合、**ブロックの外側で参照はできない**
```javascript
if(true) {
  var i = 0;
}
console.log(i); //0

if(true) {
  let j = 10;
}
console.log(j); //ReferenceError

if(true) {
  var k = 100;
}
console.log(k); //ReferenceError
```

逆にブロック外で宣言した場合はブロック内で参照することはできる
```javascript
const i = 5;
if(true) {
  console.log(i); //5
}
```

## module

### ES5
名前空間パターンを使って使えた
```javascript
var obj = obj || {};
```
[JavaScript モジュール・クラス総括（2015年7月） - Qiita](https://qiita.com/nowri/items/3ebadf71bbdfacf186aa)

#### require
ファイル自体を読み込むことで使う方法
- CommonJSの仕様として存在
- ただし、`browserify`のような変換処理なしではまだ未対応

```javascript
js --- a.js
    |
    --- b.js
このような構成があるとする

//index.html
<script src="a.js"></script>
<script src="b.js"></script>

//b.js
var a = require(./a.js);

このように書くことでb.jsの中でa.jsをインポートすることができる
```

### ES6

#### import/export
Moduleとは
- ある機能を実現するためのプログラムのかたまり
- 別ファイルに渡す変数/関数などのまとまり

##### Moduleを1つだけ受け渡す場合
`export default 'Module名'`

`import 'import先で使う名前' from 'ファイルパス'`

##### Moduleを複数受け渡す場合
exportする側では受け渡しするものに`export`をつける

import側は`import {受け取り1,受け取り2, ...} from 'ファイルパス'`

複数のmoduleをすべてimportする場合は`import * as 'オブジェクト名' from 'ファイルパス'`

[ES5とES6の違いをまとめてみた(メモ) - Qiita](https://qiita.com/rifutan/items/a55f132d4dae7e2f1941)

[【JavaScript・export・import】モジュールについて理解をまとめる \| とものブログ](https://se-tomo.com/2019/02/13/%E3%80%90javascript%E3%83%BBexport%E3%83%BBimport%E3%80%91%E3%83%A2%E3%82%B8%E3%83%A5%E3%83%BC%E3%83%AB%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6%E7%90%86%E8%A7%A3%E3%82%92%E3%81%BE%E3%81%A8%E3%82%81/)

[ES2015(ES6)な時代だからこそ、ES5を改めて調べたJavaScript初級者のメモ - Qiita](https://qiita.com/zaru/items/d833dca52962c3f7770f)

[ES6以降の構文とES5の構文の比較 - Qiita](https://qiita.com/ut0n/items/dee70188a46028fa3a1f)

[JavaScriptの仕様ES5以前とES2015(ES6)以降の違いの抑えるべき項目 – CodeAid（コードエイド）](https://codeaid.jp/js-es2015/)

[JavaScript入門 - とほほのWWW入門](http://www.tohoho-web.com/js/what.htm)

# DOMセレクターについて

## 速度面

### 要素取得
- `getElementById / getElementsByClassName`のほうが、`querySelector / querySelectorAll`より早い
[【脱jQuery】JavaScriptをネイティブで書くときのあれこれTips | Will Style Inc.｜神戸にあるウェブ制作会社](https://www.willstyle.co.jp/blog/1025/)

# mustache技法
[{{ mustache }}](https://mustache.github.io/)
[mustache記法について簡単にまとめてみた - Qiita](https://qiita.com/sengok/items/1d958348215647a5eaf0)

# constructor
- インスタンスが生成される度に呼ばれる関数

# コールバック関数
- 他の関数に引数として渡される関数

- コールバック関数は外側の関数で何らかの処理やアクションを実行する

- 同期型コールバックの場合、呼び出した順番で処理される

- 非同期型コールバックの場合、非同期命令が完了した後に続いてコードが実行される

  - そのため、同期型コールバックのように**上からの順で呼び出される**とは限らない

  - コールしてリターンされるまでコールバック関数を使っている処理より下の処理が先に動くこともある

[Callback function (コールバック関数) - MDN Web Docs 用語集: ウェブ関連用語の定義 \| MDN](https://developer.mozilla.org/ja/docs/Glossary/Callback_function)

[JavaScriptのコールバック関数の使い方を現役エンジニアが解説【初心者向け】 \| TechAcademyマガジン](https://techacademy.jp/magazine/21421)


# ES5で関数に`this`を指定する方法
- 基本的に関数単体では「これ」と指差すオブジェクトが存在しない

  - `new`でオブジェクトとして生成する必要がある

- `constructor`を使って、関数に`this`を`bind`する
  ```javascript
  class A {
    constructor() {
      // オブジェクト内の関数に直接オブジェクトを指定する
      this.verb = this.verb.bind(this);
    }

    this.word = 'do something!';

    verb() {
      return this.word;
    }
  }

  const a = new A();

  const b = a.verb; // do something!
  ```

- ES6以降はアロー関数を使うことで、自動的に`this`でそのオブジェクトを指すことができる

# メソッド
- オブジェクトのプロパティとなった関数のこと

## メソッドの宣言
```javascript
let obj = {
  a: function(x) {return typeof x} // 従来の書き方
  b: y => typeof y // ES2015から
  c:(z) {return typeof z} // ES2015から
}
```

[【Javascript】関数とメソッドの違い - 弥生研究所](https://yayoi-tech.hatenablog.com/entry/differences_between_functions_and_methods_in_javascript)

[ES2015新機能: JavaScriptのclassとmethod - Qiita](https://qiita.com/niisan-tokyo/items/83582bc0646239cf6cb8)

# `async/await`

## `async`
- 非同期関数

- アロー関数で使うなら以下のように。
  ```javascript
  const a = async () => {
    ︙
    処理
    ︙
  }
  ```

## `await`
- 非同期関数の中でのみ有効

[async function - JavaScript \| MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/async_function)

[await - JavaScript \| MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/await)


# axios
- インスタンスを作ってからリクエストを投げる
  ```javascript
  import axios from 'axios';
  const axiosBase = axios.create({
    baseURL: 'urlを記載',
    headers: {
      必要なヘッダーを記載
    }
  })

  const result = axiosBase.get('/', {
    params: {キー:バリュー}
  })
  ```

[axios/axios: Promise based HTTP client for the browser and node.js](https://github.com/axios/axios)

[[axios] axios の導入と簡単な使い方 - Qiita](https://qiita.com/ksh-fthr/items/2daaaf3a15c4c11956e9)

# Map
- 要素の順番で反復処理を行う

  - `for .. of`、`forEach`のような処理の代わりに使える
    ```javascript
    const numbers = [0,1,2,3,4];
    let newNumbersForSyntax = [];
    let newNumbersMapSyntax = [];
    // for文の場合
    for (let i = 0; i < numbers.length; i++) {
      newNumbersForSyntax.push(numbers[i] * 10);
    }

    // Mapの場合
    newNumbersMapSyntax = numbers.map(num => num * 10);
    ```
[Map - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Map)

# レンダリングされた時点で要素取得したいが、うまくいかない時
- ある要素で表現するリソースがロードされていないのに取得しようとする可能性がある

  - `addEventListener`を使って、ロードしたら取得するという書き方にする
    ```javascript
    document.getElementById('要素ID').addEventListener('load', Elementattribute)
    ```
[이벤트 참조 | MDN](https://developer.mozilla.org/ko/docs/Web/Events)
