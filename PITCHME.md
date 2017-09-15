## JS勉強会 2日目
### 2017/09/15

---

### 前回やったこと

- nodeのインストール
- JSの基本
  - データ型
  - 演算子
  - 変数

---

### 反省したこと

- ロードマップがなかった
- たんたんと進行だけになっていた

---

# 反省

![hansei](https://pbs.twimg.com/media/BpwrwC-CUAAiPUu.jpg)

---

### 今日やること

- ロードマップ
- what is <span style="color: #e49436;">this</span>?
- クラスを書いてみよう

---

### nodeのインストール

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

### ロードマップ

#### 👇

+++

#### the 1st day

- JSのドハマリポイントや基礎を学んだ

+++

#### the 2nd day 👈 まだここ

- ES2015でクラスが書けるようになる

+++

#### the 3rd day

- CSSが書けるようになる

+++

#### the 4th day

- React / Reduxを理解する

+++

#### the 5th day

- ReactJSがバリバリ書ける

---

## What is <span style="color: #e49436;">this</span>?

---

### そもそもthisって何？

- swift/kotlinの self のようなもの |

---

### thisの怖い話

#### 👇

+++

```javascript
window.foo = 'bar';
console.log(window.foo);
console.log(this.foo);
console.log(window === this);
```
@[2]()
@[2](bar)
@[3]()
@[3](bar)
@[4]()
@[4](true)

+++

```javascript
const foo = {
  bar: 'bar',
  foobar: function () {
    return 'foo' + this.bar;
  }
};

console.log(foo.bar);
console.log(foo.foobar());
```
@[1-6]()
@[8]()
@[8](bar)
@[9]()
@[9](foobar)

+++

```javascript
const foo = {
  bar: 'bar',
  foobar: function () {
    const something = function (string) {
      return string + this.bar;
    };
    return something('foo');
  }
}

console.log(foo.bar);
console.log(foo.foobar()); 
```
@[1-9]()
@[4-6]()
@[11](bar)
@[12]()
@[12](fooundefined)

+++

```javascript
const foo = {
  bar: 'bar',
  didClickButton: function () {
    console.log(this.bar);
  }
}

// ボタンを作る
const button = document.createElement('input');
button.type = 'button';
button.value = 'click here';
document.body.appendChild(button);

// クリックイベントを監視
button.addEventListener('click', foo.didClickButton);

// クリックイベントを発火
button.click();
```
@[1-6]()
@[8-12]()
@[13-14]()
@[16-17]()
@[16-17](undefined)

---

### <span style="color: #82e0aa;">that</span> is <span style="color: #e49436;">this</span> (昔の書き方)

#### 👇

+++

```javascript
const foo = {
  bar: 'bar',
  didClickButton: function () {
    const that = this;
    return function () {
      console.log(that.bar);
    }
  }
}

// ボタンを作る
const button = document.createElement('input');
button.type = 'button';
button.value = 'click here';
document.body.appendChild(button);

// クリックイベントを監視
button.addEventListener('click', foo.didClickButton());

// クリックイベントを発火
button.click();
```
@[1-9]()
@[4-7]()
@[11-15]()
@[17-18]()
@[20-21]()
@[20-21](bar)

---

### Arrow Function (※ES2015以降)

#### 👇

+++

```javascript
const foo = {
  bar: 'bar',
  didClickButton: function () {
    return () => {
      console.log(this.bar);
    }
  }
}

// ボタンを作る
const button = document.createElement('button');
button.innerHTML = 'click here';
document.body.appendChild(button);

// クリックイベントを監視
button.addEventListener('click', foo.didClickButton());

// クリックイベントを発火
button.click();
```
@[1-8]()
@[4-6]()
@[19-20]()
@[19-20](bar)

---

#### でもさ、毎回関数を返却する関数にするの？

---

### Bound Function

#### 👇

+++

### Bound Functionとは

- 関数内のthisが固定された関数のこと
- <span style="color: #82e0aa;">func.bind(thisArg)</span> で固定化される

+++

```javascript
const foo = {
  bar: 'bar',
  didClickButton: () => {
    console.log(this.bar);
  }
}

// ボタンを作る
const button = document.createElement('button');
button.innerHTML = 'click here';
document.body.appendChild(button);

// クリックイベントを監視
button.addEventListener('click', foo.didClickButton.bind(foo));

// クリックイベントを発火
button.click();
```
@[1-6]()
@[13-14]()
@[16-17]()
@[16-17](bar)

---

#### だからといって
#### 毎回 bind() 書くのめんどくさいよね・・・

---

### SP版での事例

---

### クラスを書いてみよう

---

### お題
#### ボタンをクリックするたびにFizz Buzzを出力

---

### 基本的な構文

```javascript
import BaseClass from './BaseClass';

class Foo extends BaseClass {
  constructor (string) {
    super();

    this.bar = string;
  }

  foobar () {
    return 'foo' + this.bar;
  }
}

const foo = new Foo('bar');

console.log(foo.foobar()); // -> foobar
```

---

### その他の例

```javascript
import BaseClass from './BaseClass';

class Button extends BaseClass {
  constructor (string) {
    super();

    this.bar = string;

    this.button = document.createElement('button');
    this.button.innerHTML = 'click here';

    this.button.addEventListener('click', this.didClickButton);
  }

  didClickButton () {
    console.log('foo' + this.bar);
  }

  appendTo (element) {
    element.appendChild(this.button);
  }

  click () {
    this.button.click();
  }
}

const button = new Button('bar');
button.appendTo(document.body);

button.click(); // -> foobar
```

---

### @m8x が書いたFizz Buzzボタン

```javascript
import BaseClass from './BaseClass';

class FizzBuzzButton extends BaseClass {
  constructor () {
    super();

    this.currentNumber = 0;

    this.button = document.createElement('button');
    this.button.innerHTML = `click here: ${this.currentNumber}`;

    this.button.addEventListener('click', this.didClickButton);
  }

  didClickButton () {
    this.currentNumber++;
    const result = this.fizzBuzz();
    this.button.innerHTML = `click here: ${this.currentNumber} : ${result}`;
  }

  fizzBuzz () {
    if (this.currentNumber % 15 === 0) {
      return 'FizzBuzz';
    } else if (this.currentNumber % 3 === 0) {
      return 'Fizz';
    } else if (this.currentNumber % 5 === 0) {
      return 'Buzz';
    }

    return this.currentNumber;
  }

  appendTo (element) {
    element.appendChild(this.button);
  }
}

const button = new FizzBuzzButton();
button.appendTo(document.body);
```

---

### ビルドしてみよう

---

### 次回

- 次回はCSS周りを触ります！
