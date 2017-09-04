## JS勉強会 1日目
### 2017/09/05

---

### 今日やること

- nodeのインストール
- JSの基本
  - データ型
  - 演算子
  - 変数
- 宿題

---

### 1. nodeのインストール

```bash
$ brew update
$ brew install n
$ n 8.2.1

     install : node-v8.2.1
       mkdir : /usr/local/n/versions/node/8.2.1
       fetch : https://nodejs.org/dist/v8.2.1/node-v8.2.1-darwin-x64.tar.gz
######################################################################## 100.0%
   installed : v8.2.1
```

---

## 2. JSの基本

---

### 2.1. データ型について

JSのデータ型は `プリミティブ` と `オブジェクト` の二種類に大別できます。

---

### 2.2. プリミティブ

JSには<span style="color: orange">6種類</span>のプリミティブ型が存在します。

- 文字列
- 数値
- 真偽値
- シンボル ※ES2015以降
- null
- undefined

全てのプリミティブは Immutable

---

### 2.3. オブジェクト

プリミティブ以外のデータ型は全てObjectを継承しています。

- 連想配列 Objectクラス
- 配列 Arrayクラス
- 関数 Functionクラス
- プリミティブラッパーオブジェクト
- その他

---

### 2.4. プリミティブラッパーオブジェクト

null と undefined 以外のプリミティブ型にはプリミティブラッパーオブジェクトが存在します。

- 文字列 -> String
- 数値 -> Number
- 真偽値 -> Boolean
- シンボル -> Symbol

なお自ら使うことはありません(ES2015から非推奨)

---

### 2.5. プリミティブラッパーオブジェクト

```javascript
const primitive1 = 'this is primitive.';
const primitive2 = String('this is primitive.');
const notPrimitive = new String('this is object.');

// primitive1 === primitive2 -> true
// primitive1 === notPrimitive -> false
// primitive1 === notPrimitive.toString() -> true

// typeof primitive1 === 'string' -> true
// typeof primitive2 === 'string' -> true
// typeof notPrimitive === 'object' -> true
```
@[1-2](primitive)
@[3](object)
@[5-7](プリミティブとオブジェクトの比較は必ず失敗)
@[9-11](typeof演算子を使うと基本のデータ型を判定できる)

---

### 2.6. プリミティブラッパーオブジェクト

#### primitive1 + notPrimitive = ?

- 答えは 'this is primitive.this is object.' |

---

### 2.7. プリミティブ型のオブジェクトみたいな挙動

```javascript
'foo'.repeat(2);
// -> 'foofoo'

(0).toString();
// -> '0'

0.toString();
// -> Uncaught SyntaxError: Invalid or unexpected token
```

- JavaScriptでは、必要に応じてプリミティブの値がオブジェクトに変換されます。 |

---

### 2.8. 演算子について: !

JSでは式の頭に `!` (論理否定演算子) をつけることでboolean型を返却します。booleanが必要な場合によく利用します。

```javascript
!true
// -> false

!1
// -> false

!!true
// -> true

!!1
// -> true
```

---

### 2.9. 演算子について: +

```javascript
'foo' + 'bar'
// -> 'foobar'

'123' + '123'
// -> '123123'

'123' + 123
// -> '123123'

123 + '123'
// -> '123123'

'123' + true
// -> '123true'

123 + 123
// -> 246
```
@[1-14](文字列 + 何か は基本的に文字列に変換して連結する)
@[16-17](数値同士はもちろん数値)

---

### 2.10. 演算子について: -, \*, /

```javascript
'foo' - '123'
// -> NaN

'123' - '1'
// -> 122

'123' - 1
// -> 122

'123' - true
// -> 122
```
@[1-8](文字列 - 何か はそれぞれ数値に変換できる場合は数値として扱われる)
@[10-11](trueは数値にすると1になる)
@[1-11](掛け算や割算も同様である)

---

### 2.11. 演算子について: ==, ===, !=, !==

`==, !=` は型を変換して比較するため危険であり、非推奨です。例えば以下は全てtrueが返却されます。

```javascript
1 == true
'1' == 1
'' == false
0 == false
null == undefined
```

---

### 2.12. 演算子について: ==, ===, !=, !==

代わりに厳密な `===, !==` を使用します。

```javascript
1 === true
// -> false

'1' === 1
// -> false

!1 === false
// -> true

1 === 1
// -> true
```

---

### 2.13. オブジェクトの比較

オブジェクトの中身の比較に `===`, `==` 演算子は利用できません。  
JSでは内部参照を比較するので、全く同一のオブジェクトの場合のみtrueが返却されます。

```javascript
const object1 = { key: 'value' };
const object2 = { key: 'value' };

object1 == object2
// -> false

object1 === object2
// -> false

const object3 = object1;

object1 === object3
// -> true
```

---

### 2.14. 変数の宣言

変数の宣言には4種類の方法があります。

- 明示的に宣言せずに代入する -> グローバル変数として宣言されます。書き換え可能。 ※非推奨

```javascript
function foo () {
  bar = true;

  if (bar) {
    baz = 'baz';
  }

  bar = false;

  console.log(bar, baz);
  // -> false, 'baz'
}

console.log(bar, baz);
// -> undefined, undefined

foo();

console.log(bar, baz);
// -> false, 'baz'

```

---

### 2.15. 変数の宣言: var

変数の宣言には4種類の方法があります。

- var -> 関数スコープで宣言されます。書き換え可能

```javascript
function foo () {
  var bar = true;

  if (bar) {
    var baz = 'baz';
  }

  bar = false;

  console.log(bar, baz);
  // -> false, 'baz'
}
```

---

### 2.16. 変数の宣言: let

変数の宣言には4種類の方法があります。

- let -> ブロックスコープで宣言されます。書き換え可能

```javascript
function foo () {
  let bar = true;

  if (bar) {
    let baz = 'baz';
  }

  bar = false;

  console.log(bar, baz);
  // -> false, undefined
}
```

---

### 2.17. 変数の宣言: const

変数の宣言には4種類の方法があります。

- const -> ブロックスコープで宣言されます。書き換え不可

```javascript
function foo () {
  const bar = true;

  if (bar) {
    const baz = 'baz';
  }


  console.log(bar, baz);
  // -> true, undefined

  bar = false;
  // -> Uncaught TypeError: Assignment to constant variable.
}
```

---

### 2.18. 変数の基本

- 変数の実態は参照(アドレス) |
- なので const で定義した変数も参照先のObjectは操作できる(Writableな場合のみ) |
- プリミティブ型はImmutableなので、参照が入れ替わることになる |
- 関数に引数を渡す場合は「参照の値渡し」と表現される |

---

### 2.19. 参照の値渡し その1

- プリミティブ型を渡した場合、値渡しのような挙動になる

```javascript
function foo (bar) {
  bar = 'foo' + bar;
  console.log(bar);
}

let baz = 'baz';
foo(baz);
// -> 'foobaz'

console.log(baz);
// -> 'baz'
```

---

### 2.20. 参照の値渡し その2

- オブジェクト型を渡した場合、参照渡しのような挙動になる ※これはよくある勘違い

```javascript
function foo (bar) {
  bar.baz = 'foo' + bar.baz;
  console.log(bar);
}

const obj = { baz: 'baz' };
foo(obj);
// -> { baz: 'foobaz' }

console.log(obj);
// -> { baz: 'foobaz' }
```

---

### 2.21. 参照の値渡し その3

- 参照の値渡しとは、変数を渡す(参照渡し)わけではなく、参照(アドレス)を渡す

```javascript
function foo (bar) {
  bar = { baz: 'foo' + bar.baz };
  console.log(bar);
}

const obj = { baz: 'baz' };
foo(obj);
// -> { baz: 'foobaz' }

console.log(obj);
// -> { baz: 'baz' }
```

---

### 3. 宿題

---

##### 3. 宿題

- ごめんなさい、後ほどアナウンスします＞＜ |
