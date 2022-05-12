# Core-JS study

## this

</br>

- 목차

1. this 의 생성
2. this 의 호출 방식  
   2-1. 전역공간에서  
   2-2. 함수 호출시  
   2-3. 메서드 호출시  
   2-4. callback 함수 호출시  
   2-5. 생성자함수 (new 연산자) 함수 호출시
3. this 정리

</br>
</br>
</br>

## 1. this 의 생성

</br>
</br>
</br>

|               |                      |                                                                         |
| :-----------: | :------------------: | :---------------------------------------------------------------------: |
|     inner     | Variable Environment | environmentRecord (snapshot) </br> outerEnvironmentReference (snapshot) |
|     outer     | Lexical Environment  |            environmentRecord </br> outerEnvironmentReference            |
| 전역 컨텍스트 |     This Binding     |

</br>
</br>

`ThisBinding` 은 실행 컨텍스트가 활성화 될 때, 생성될 때 한다.

</br>
</br>

그런 실행 컨텍스트가 생성되는 순간은 `해당 함수`가 `호출되는 순간`이다.

</br>
</br>

다시 말하자면 `this` 는 `함수가 호출될 때에 비로소 결정`된다.

</br>
</br>

해당 함수를 `어떤 식으로 호출했는지에 따라`서 this 는 얼마든지 달라질 수 있다. (동적 바인딩)

</br>
</br>
</br>

## 2. this 의 호출 방식

</br>
</br>
</br>

### 2-1. 전역공간에서

- `브라우저`에서는 `window`, `node.js` 에서는 `global` 이라는 `전역 객체`
- 개념상 전역 컨텍스트를 실행하는 주체가 전역 객체이기 때문

</br>
</br>
</br>

### 2-2. 함수 호출시

- 함수 호출시에도 전역 객체를 가리킴
- `브라우저`에서는 `window`, `node.js` 에서는 `global` 이라는 `전역 객체`
- 함수 호출 시 전역 공간에서 함수가 호출됨, 호출한 대상이 전역 객체가 됨

</br>

```js
function b() {
  function c() {
    console.log(this);
  }
  c();
}
b();
```

</br>

    Q.

    b() 호출 시 주체가 전역 객체임은 알겠으나 c() 호출 시에는
    b() 함수 내부에서 호출한 것이니 호출한 주체를 b() 로 봐야 하지 않을까?

    A.

    실제로는 전역 객체가 나옴,
    ECMAScript6 에서는 이러한 의견을 수렴하여 this 바인딩을 하지 않는 arrow function 이 나옴,
    arrow function 은 바로 위 컨텍스트에 있는 this 를 그대로 가져다 사용함,
    ES5 환경에서는 함수로써 호출했을 때 this 는 언제나 전역 객체를 가리킴

</br>
</br>
</br>

### 2-3. 메서드 호출시

- 메서드 호출 주체 (메서드명 앞 - 메소드 명의 '점' 바로 앞에 있는 것이 this)
- `원래는 함수`인데 `어떤 객체 ('점' 앞) 와 관련된 동작`을 하게 되면 그 때 `'메서드'`라고 부르겠다는 것

</br>

```js
var d = {
  e: function () {
    function f() {
      console.log(this);
    }
    f(); // 함수로써 호출함, this는 전역 객체
  },
};
d.e();
```

</br>
</br>

```js
var a = {
  b: function () {
    console.log(this);
  },
};
a.b(); // a 가 this 가 됨
// b 함수는 a 의 프로퍼티, 프로퍼티에 함수를 할당한 것
// b 함수를 a 객체의 메서드로써 호출
```

</br>

    출력
    > {b: f}
      > b: f()
      > [[Prototype]]: Object

</br>
</br>

```js
var a = {
  b: {
    c: function () {
      console.log(this);
    },
  },
};
a.b.c(); // a.b 가 this 가 됨
// c 함수에게 "a 와 b 라는 객체와 관련된 동작을 수행해라" 는 명령
```

</br>

    출력
    > {c: f}
      > c: f()
      > [[Prototype]]: Object

</br>
</br>

### +) Dot notation vs Bracket notation

    obj.func(); // this -> obj
    obj['func](); // this -> obj

    person.info.getName(); // this -> person.info
    person.info['getName']; // this -> person.info
    person['info'].getName(); // this -> person['info']
    person['info]['getName'](); // this -> person['info']

</br>
</br>

### +) 메서드 내부함수에서의 우회법

</br>
</br>

```js
var a = 10;
var obj = {
  a: 20,
  b: function () {
    console.log(this.a); // 20

    function c() {
      console.log(this.a); // 10
    }
    c(); // this 가 없는 c, c 함수 내부의 this 는 전역 객체를 가리킴
  },
};
obj.b(); // 메서드로써 호출됨
```

</br>

    c 함수 내부의 this.a 는 전역객체의 프로퍼티 a (window.a) 를 뜻한다고 봐야 함
    > 그런데 10 이 나옴
    > 전역 객체의 프로퍼티 a 를 요청했는데 전역 변수 a 를 준 셈
    > 전역 객체와 전역 변수는 별개의 개념으로 여겨야 할 것 같은데 실제로는 전역 변수가 전역 객체의 프로퍼티로 동작한 것

</br>

    내부함수 this === obj 으로 하려면 어떻게 해야할까?

    > 스코프체인을 이용한 '다른 변수를 사용' 하면 됨
    > 내부 함수보다 상위에서 self 라고 하는 변수에 this 를 담고 안쪽에서 self 사용
    > 내부 함수는 자신의 LexicalEnvironment 에서 self 를 찾고 없으니
    outerEnvironmentReference 를 타고 외부에 있는 즉, b 함수의 Lexical Environment 에서 self 를 찾게 됨
    > 해당 self 에는 앞서 들어온 this 가 있을 것, 해당 this 는 obj.b 호출 시의 this 이므로 obj 를 가리킴
    > 메서드에서와 동일한 this 를 사용할 수 있게 됨

</br>

```js
var a = 10;
var obj = {
  a: 20,
  b: function () {
    var self = this;
    console.log(this.a); // 20

    function c() {
      console.log(self.a); // 20
    }
    c();
  },
};
obj.b();
```

</br>

```js
// ES6, this 를 바인딩 하지 않는 arrow function 이 등장, 우회 필요 X
// Scope Chaining 상의 this 로 바로 접근 가능

var a = 10;
var obj = {
  a: 20,
  b: function () {
    console.log(this.a); // 20

    const c = () => {
      console.log(this.a); // 20
    };
    c();
  },
};
obj.b();
```

</br>
</br>
</br>

### 2-4. callback 함수 호출시

- 기본적으로 `함수`의 this 와 같다.
- `제어권을 가진 함수`가 콜백의 `this를 지정`해둔 경우도 있다.
- 이 경우에도 `개발자가 this를 바인딩`해서 콜백을 넘기면 그에 따른다.

</br>
</br>

> 이해하기 위해서는 배경 지식 필요

### +) call, apply, bind 메소드에 대하여

</br>
</br>

```js
function a(x, y, z) {
  console.log(this, x, y, z);
}
var b = {
  bb: "bbb",
};

a.call(b, 1, 2, 3);

a.apply(b, [1, 2, 3]);

var c = a.bind(b); // 호출되지 않은 상태, this 만 가지고 있는 상태
c(1, 2, 3);

var d = a.bind(b, 1, 2); // 호출되지 않은 상태, this, 1번째 , 2번째 매개변수까지 넘겨줌
d(3); // 3을 넘기면서 호출

// 위의 4가지 호출 모두 동일한 결과가 나옴
// {bb: "bbb"} 1 2 3
// {bb: "bbb"} 1 2 3
// {bb: "bbb"} 1 2 3
// {bb: "bbb"} 1 2 3
```

</br>

API 문서 (대괄호 부분은 생략 가능)

```js
func.call(thisArg[, arg1[, arg2[, ...]]])

func.apply(thisArg, [argsArray])

func.bind(thisArg[, arg1[, arg2[, ...]]])
```

</br>

    thisArg -> func 호출 시 this 는 해당 부분으로 호출하도록 명시하는 부분

    => 그래서 명시적 (this) 바인딩 이라고 함

</br>
</br>
</br>

돌아와서 callback 함수 호출 시 예제를 살펴본다.

</br>
</br>

```js
var callback = function () {
  console.dir(this); // 전역 객체 window
};
var obj = {
  a: 1,
  b: function (cb) {
    cb(); // 함수로써 callback 호출
  },
};
obj.b(callback);
```

</br>

    obj 객체 안에 b함수가 메소드로서 존재
    > obj.b 메서드로써 호출, callback 이라고 하는 함수를 넘겨주고 있음

</br>
</br>

```js
var callback = function () {
  console.dir(this); // obj
};
var obj = {
  a: 1,
  b: function (cb) {
    cb.call(this); // b 의 this -> obj
  },
};
obj.b(callback);
```

</br>

    obj 객체 안에 b함수가 메소드로서 존재
    > obj.b 메서드로써 호출, callback 이라고 하는 함수를 넘겨주고 있음
    > cb 호출 시 thisArg 에 this 를 넘김
    > b 의 this 는 obj 이므로 this 로 obj 를 넘김

    넘겨받은 대상이 cb.call 로 this 를 넘겨주고 있다라고 하면 obj 가 됨

</br>

### `Callback 함수`에서의 `this` 는 지정하는 바에 따라 달라진다. `상황에 따라 다르게` 된다.

</br>
</br>

다른 예시를 살펴 본다.

</br>
</br>

```js
var callback = function () {
  console.dir(this); // 전역 객체 window
};
var obj = {
  a: 1,
};
setTimeout(callback, 100); // setTimeout 은 this 를 별도 처리하지 않음
```

</br>

우리가 원하는 this 를 호출하고 싶을 때는 아래와 같이 bind 사용하면 된다.

</br>

```js
var callback = function () {
  console.dir(this); // Object
};
var obj = {
  a: 1,
};
setTimeout(callback.bind(obj), 100);
```

    this 결과

    > Object
      a: 1
      > [[Prototype]]: Object

</br>
</br>

이번에는 이벤트 핸들러 예시를 본다.

</br>
</br>

```js
document.body.innerHTML += '<div id="a">클릭하세요</div>';

document.getElementById("a").addEventListener("click", function () {
  console.dir(this);
});
```

</br>

    innerHTML 로 <div> 를 주고 id=a 를 선택하여 클릭 이벤트에 대해서 핸들러로 콜백함수를 넘기는 코드

    실행하게 되면 Html DOM 엘리먼트(div#a)가 나옴
    > div#a
      accessKey: ""
      align: ""
      .
      .

    addEventListener 라는 함수가 콜백함수를 처리할 때

    this 는 이벤트가 발생한 그 타겟 대상 엘리먼트로 하도록 정의가 되어있기 때문

    콜백함수의 this 를 별도로 지정해둔 경우

</br>
</br>

우리가 원하는 this 를 호출하고 싶을 때는 아래와 같이 bind 사용하면 된다.

</br>

```js
document.body.innerHTML += '<div id="a">클릭하세요</div>';
var obj = { a: 1 };

document.getElementById("a").addEventListener(
  "click",
  function () {
    console.dir(this);
  }.bind(obj) // 콜백함수를 넘겨줄 때 this 를 obj 로 하도록 명시적 바인딩을 해줌
);
```

</br>

    this 결과
    > Object
      a: 1
      > [[Prototype]]: Object

</br>
</br>

### 콜백함수 호출 시 정리

- 기본적으로 `함수`의 this 와 같다.
- `제어권을 가진 함수`가 콜백의 `this를 지정`해둔 경우도 있다.
- 이 경우에도 `개발자가 this를 바인딩`해서 콜백을 넘기면 그에 따른다.

</br>
</br>
</br>

### 2-5. 생성자함수 (new 연산자) 함수 호출시

- `new 연산자를 사용`했다는 말은 `생성자 함수의 내용`을 바탕으로 `인스턴스 객체를 만드는 명령`
- `새로 만들 인스턴스 객체` 그 자체가 곧 `this` 가 된다.

</br>
</br>

```js
function Person(n, a) {
  this.name = n;
  this.age = a;
}
var sue = Person("수", 100);
console.log(window.name, window.age);
```

</br>

    일반적으로 new 연산자 없이 Person 을 사용하면 sue 에는 아무것도 담기지 않게 됨

    > 그저 함수로써 호출한 것이기 때문에 그 때 this 는 전역 객체를 가리킴

    > 전역 객체의 name 프로퍼티와 age 프로퍼티에 값이 할당이 되어서 "수 100" 이 출력됨

</br>
</br>

```js
function Person(n, a) {
  this.name = n;
  this.age = a;
}
var sue = new Person("수", 100);
console.log(sue);
```

</br>

    new 연산자를 넣은 채로 호출하게 되면 생성자 함수로써 호출하게 된 것

    > sue 라고 하는 변수 즉, 새로 생성될 Person 의 인스턴스 객체 자신이 곧 this 가 됨

    > 객체가 새로 만들어지면서 그 객체 안에 name 프로퍼티, age 프로퍼티가 생성되면서

    > name 에 수, age 에 100 이 담기게 되고 그 객체가 다시 sue 라는 변수에 담기게 됨

</br>
</br>

    출력 결과

    > Person {name: "수", age: 100}
      age: 100
      name: "수"
      > [[Prototype]]: Object

</br>
</br>
</br>

### 3. this 정리

</br>

|      호출 장소      |                                                                                                             this                                                                                                             |
| :-----------------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|    전역공간에서     |                                                                                                       window / global                                                                                                        |
|    함수 호출 시     |                                                                                                       window / global                                                                                                        |
|   메서드 호출 시    |                                                                                                메서드 호출 주체 (메서드명 앞)                                                                                                |
|  callback 호출 시   | 기본적으로는 함수 내부에서와 동일하게 전역 객체를 보지만 </br> callback 함수를 어떤 식으로 처리하는 지에 따라서 this 가 얼마든지 달라질 수 있음 </br> 그런 경우 bind 명령 등을 통해서 사용자가 직접 this 를 명시할 수도 있음 |
| 생성자 함수 호출 시 |                                                                                                           인스턴스                                                                                                           |

</>
</br>
</br>
