# Core-JS study

## Callback

</br>

- 목차

1. Callback 의 의미
2. 제어권 위임  
   2-1. 실행 시점  
   2-2. 매개변수  
   2-3. this
3. this 정리

</br>
</br>
</br>

## 1. Callback 의 의미

</br>
</br>
</br>

Callback 자체로는 '회신하다/답신하다' 라는 뜻을 지닌다.

</br>

그렇다면 Callback function 하면 `회신되는 함수` 라는 뜻이 되겠다.

</br>

넘기고자 하는 대상에게 콜백함수에 대한 제어권을 넘긴다(`제어권 위임`)는 뜻이 된다.

</br>

넘겨주게 될 제어권에는 `실행 시점`, `매개변수`, `this` 이 있다.

</br>
</br>
</br>

## 2. 제어권 위임

</br>
</br>
</br>

## 2-1. 실행 시점

</br>
</br>
</br>

```js
setInterval(function () {
  console.log("1초마다 실행될 겁니다.");
}, 1000);
```

</br>

    setInterval 주기함수 호출
    > 인자1: 콜백함수
    > 인자2: 주기(ms)

</br>
</br>

첫 번째 인자를 cb 변수로 할당 후 코드 작성하면 아래와 같다.

```js
var cb = function () {
  console.log("1초마다 실행될 겁니다.");
};

setInterval(cb, 1000);
```

</br>

    setInterval(callback, milliseconds)

    > setInterval 에게 함수를 넘겨주고 나면 알아서 자동으로 milliseconds 주기마다 한 번씩 함수를 실행함
    > 제어권을 setInterval 에게 위임한 것이 됨

</br>
</br>
</br>

## 2-2. 매개변수

</br>
</br>
</br>

```js
var arr = [1, 2, 3, 4, 5];

var entries = [];

arr.forEach(
  function (v, i) {
    entries.push([i, v, this[i]]);
  },
  [10, 20, 30, 40, 50]
);

console.log(entries);
```

</br>

결과는 아래와 같다.

```js
[
  [0, 1, 10],
  [1, 2, 20],
  [2, 3, 30],
  [3, 4, 40],
  [4, 5, 50],
];
```

</br>

    forEach: 배열에 있는 요소들을 순서대로 하나씩 꺼내서
    첫 번째 매개변수(v)에 값, 두 번째 매개변수(i)에 index를 부여하면서 이 함수를 실행함

    > 첫 번째 배열의 this[i] 가 10 이 나온 것으로 보아 forEach 첫 번째 인자는 callback 함수를, 두 번째 인자는 thisArg 가 들어가는 규칙을 알 수 있음

</br>

    forEach 호출
    > 인자1: 콜백함수
    > 인자2: this로 인식할 대상 (생략 가능)

    Array.prototype.forEach() from MDN

    > arr.forEach(callback[, thisArg]) 로 정의 되어 있음

    > forEach 에게 콜백을 넘기면, 이 콜백함수에 들어올 매개변수의 내용, 순서는 전적으로 forEach 에 정의된 방식에 따를 수 밖에 없음
    > forEach 에 콜백으로 넘길 용도로 함수를 만들거면 forEach 에 정의된 방식에 따를 수 밖에 없음

</br>
</br>
</br>

## 2-3. this

</br>
</br>
</br>

```js
document.body.innerHTML = '<div id="a">abc</div>';
function cbFunc(x) {
  console.log(this, x);
}

document.getElementById("a").addEventListener("click", cbFunc);
```

</br>

    id 가 a 인 element 를 클릭했을 때 cbFunc 을 호출하도록 addEventListener 에 넘기고 있는 상황

</br>

결과는 아래와 같다.

```js
<div id="a">abc</div>
> PointerEvent {isTrusted: true, pointerId: 1, width: 1, height: 1,}
```

</br>

    this 에는 클릭한 대상인 id 가 a 인 div 가 나옴

    x에는 PointerEvent 라고 해서, click event 에 대한 이벤트 객체가 나옴
    > 왜냐하면 addEventListener 가 그렇게 정의했기 때문임
    > addEventListener 가 콜백함수를 받을 때 this 는 eventTarget, 콜백함수 첫 번째 인자에는 event 객체를 넘겨주도록 정해놓았기 때문

</br>

    addEventListener(type, callback, options) from MDN

    > type: 'click', 'mousemove', 'keyup', 'dragstart', 'scroll' 등의 반응할 이벤트 유형을 나타내는 대소문자 구분 문자열

    > callback: 콜백함수 (단일매개변수를 받으며 이 매개변수에는 '발생한 이벤트를 설명하는 Event 에 기반한 객체' 즉, Event 인스턴스가 오며 반환값이 없음)

    > this: 전달된 이벤트 argument 의 currentTarget 속성과 같음 즉, this 에는 currentTarget 이 바인딩됨

    > options: 다양한 옵션들을 담은 객체가 오거나, 캡쳐 여부에 관한 useCapture 라는 boolean 값이 올 수 있음

    결국 addEventListener 에는 매개변수가 event 객체로 지정이 되고 this 에는 currentTarget 이 바인딩 된다는 것이 정해져 있음

</br>
</br>

클릭 이벤트 발생 시 호출 될 콜백함수 안에서 this 를 obj 로 바꾸고자 한다면 아래와 같이 bind 처리 해주면 된다.

```js
document.body.innerHTML = '<div id="a">abc</div>';
function cbFunc(x) {
  console.log(this, x);
}

const obj = { a: 1 };

document.getElementById("a").addEventListener("click", cbFunc.bind(obj));
```

</br>

결과는 아래와 같다.

```js
{a: 1}
> PointerEvent {isTrusted: true, pointerId: 1, width: 1, height: 1,}
```

</br>
</br>
</br>

## 3. Callback 함수의 특징 정리

</br>
</br>
</br>

a) 다른 함수(A)의 인자로 콜백함수 (B)를 전달하면, A가 B의 `제어권`을 갖게 된다.

</br>

b) 특별한 요청 (bind) 이 없는 한 A에 `미리 정해놓은 방식` 에 따라 B를 호출한다.

</br>

c) 미리 정해놓은 방식이란 어떤 `시점`에 콜백을 호출할지, `인자`에는 어떤 값들을 지정할지, `this` 에 무엇을 바인딩할지 등이다.

</br>
</br>

### +) 주의할 점

</br>
</br>

Callback 은 함수이다. `Callback` 으로 넘기는 것은 무조건 `함수`이다.

</br>

```js
var arr = [1, 2, 3, 4, 5];
var obj = {
  vals: [1, 2, 3],
  logValues: fuction(v, i) {
    if(this.vals) {
      console.log(this.vals, v, i);
    } else {
      console.log(this, v, i);
    }
  }
};

obj.logValues(1, 2); // [1, 2, 3] 1 2 - this 를 메소드로 호출
arr.forEach(obj.logValues); // 전역 객체 - 콜백함수로 전달
/*
- forEach 에 콜백함수로써 전달
- forEach 가 어떤 식으로 호출하느냐에 따라 this 가 달라짐
- obj.logValues 가 가리키는 '함수' 만 전달, 전달 받은 forEach 는 함수로써 호출

> Window {...} 1 0
> Window {...} 2 1
> Window {...} 3 2
> Window {...} 4 3
> Window {...} 5 4
*/
```

</br>

obj 로 지정하려고 한다면 아래와 같이 작성해야한다.

```js
arr.forEach(obj.logValues.bind(obj)); // obj 를 bind
arr.forEach(obj.logValues, obj); // forEach 두 번째 매개변수 thisArg
```