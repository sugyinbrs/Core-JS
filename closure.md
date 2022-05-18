# Core-JS study

## Callback

</br>

- 목차

1. Closure 의 의미  
   1-1. 특별한 현상이란
2. Closure 의 특징

</br>
</br>
</br>

## 1. Closure 의 의미

</br>
</br>
</br>

Closure 는 사전에서 닫힘 / 폐쇄 / 완결성 이라고 한다.

</br>

MDN 에서는 `내부함수`와 `Lexical Environment` 의 `조합` 으로 이해할 수 있다.

</br>

실행 컨텍스트 A 내부에서 함수 B 를 선언한 구조라면  
`A 의 Lexical Environment` 와 `내부함수 B`의 `조합`에서 나타나는 `특별한 현상` 이라고 이해할 수 있다.

</br>

|  B  | <h3>LexicalEnvirionment</h3> </br> EnvironmentRecord </br> outerEnvironmentReference ✔︎ |
| :-: | :-------------------------------------------------------------------------------------: |
|  A  | <h3>LexicalEnvirionment</h3> </br> EnvironmentRecord ✔︎ </br> outerEnvironmentReference |

</br>

다시 말해 `컨텍스트 A` 에서 `선언한 변수`를 `내부함수 B` 에서 `참조할 경우`에 `발생`하는 `특별한 현상` 이라고 할 수 있다.

</br>
</br>
</br>

## 1-1. 특별한 현상이란

</br>
</br>
</br>

```js
var outer = function () {
  var a = 1;
  var inner = function () {
    console.log(++a);
  };
  inner();
};
outer();
```

</br>

    1)

    전역 컨텍스트와 outer 내부에서 선언한 변수 사이에 클로저가 생성됨

    > 전역 컨텍스트에서 선언한 변수를 내부함수 outer 에서 참조하는 상황은 아님

    > 특별한 상황이 아님, 당연한 개념에 속하는 클로저

</br>

    2)

    inner 함수 안에서 outer 에서 선언한 변수 a 의 값을 변경하고 있음

    > outer 컨텍스트에서 선언한 변수를 내부함수 inner 에서 참조하고 있는 상황

    > 특별한 상황?

</br>

    전체 흐름을 살펴보자면

    > 전역 컨텍스트

    > outer() 호출
      > LexicalEnvironment
        > environmentRecord : { a: 1, inner: f }
        > outerEnvironmentReferece : { outer : f }

</br>

    inner 함수 호출

    > 전역 컨텍스트

    > outer() 호출
      > LexicalEnvironment
        > environmentRecord : { a: 1 -> 2, inner: f }
        > outerEnvironmentReferece : { outer : f }

    > inner() 호출
      > LexicalEnvironment
        > environmentRecord : {}
        > outerEnvironmentReferece : { a: 1 -> 2, inner: f }
        // outer() Lexical Environment 참조

</br>

    inner 실행 컨텍스트 종료 후 inner 컨텍스트가 사라짐

    > 전역 컨텍스트

    > outer() 호출
      > LexicalEnvironment
        > environmentRecord : { a: 2, inner: f }
        > outerEnvironmentReferece : { outer : f }

    outer 함수 실행이 종료되면 outer 컨텍스트도 사라짐

    전역 컨텍스트도 사라지면서 끝이 남

</br>

딱히 특별한 것이 없어 보인다.

</br>
</br>
</br>

아래와 같이 코드를 변경해보았다.

</br>

```js
var outer = function () {
  var a = 1;
  var inner = function () {
    return ++a;
  };
  return inner;
};
var outer2 = outer();
console.log(outer2());
console.log(outer2());
```

</br>

    전체 흐름을 살펴보자면

    > 전역 컨텍스트
      > environmentRecord : {outer: f, outer2: undefined}

    전역 실행컨텍스트는 outer, outer2 라는 변수를 수집

    > outer 함수 호출하기 직전까지만 놓고 보면 현재까지는 outer 변수에만 함수가 담겨있는 상태

    > outer2 는 outer 함수의 실행이 종료된 시점이 되어야 어떤 값이 담길 것임, undefined 상태

</br>

    > 전역 컨텍스트
      > environmentRecord : {outer: f, outer2: undefined}

    > outer() 호출
      > envirionementRecord : { a: 1, inner: f}
      > outerEnvironmentReference : { outer: f }

</br>

    outer() 실행이 끝나면 이 때 inner 가 반환되어 반환 결과인 inner 가 outer2 에 담기게 됨

    outer 실행 컨텍스트는 점선 처리 됨, outer2 에 의해 언젠가 inner 함수가 호출될 수 있기 때문임

    inner 내부에서 참조하고 있는 outerEnvironmentReference 상의 a 변수는 참조 카운트가 0 이 아닌 상태임, a 에 대한 참조가 살아있는 상태

    > 전역 컨텍스트
      > environmentRecord : {outer: f, outer2: inner}

      > outer()
      envirionementRecord : { a: 1, ~~inner: f~~}
      ~~outerEnvironmentReference : { outer: f }~~

    outer 실행 컨텍스트는 종료 되었지만 내부에서 선언한 변수 a 가 죽지 않은 것임

    원래는 죽는 것이 맞으나 참조 카운트가 0 이 아니라서 죽지 못한 상태임 (like a Zombie)

</br>

    console.log(outer2()) 호출

    > 전역 컨텍스트
      > environmentRecord : {outer: f, outer2: inner }

    --- outer
      > environementRecord : { a: 1 -> 2 } // a 값 변화
    ---

    > inner
      > environmentRecord : {}
      > outerEnvironmentReference : { a: 1 -> 2 } // 참조하는 a 값 같이 변화

    inner 컨텍스트 내부에서 a 에 접근,
    a 는 environmentRecord 에는 없고 outerEnvironmentReference 에만 있는 상태

</br>

    반환하면서 종료 시 inner 실행 컨텍스트 제거, 콘솔 2가 출력됨

    > 전역 컨텍스트
      > environmentRecord : {outer: f, outer2: inner }

    --- outer
      > environementRecord : { a: 2 }
    ---

</br>

    다시 console.log(outer2()) 호출

    > 전역 컨텍스트
      > environmentRecord : {outer: f, outer2: inner }

    --- outer
      > environementRecord : { a: 2 -> 3 }
    ---

    > inner
      > environmentRecord : {}
      > outerEnvironmentReference : { a: 2 -> 3 } // 참조하는 a 값 같이 변화

</br>

    반환하면서 종료 시 inner 실행 컨텍스트 제거, 콘솔 3이 출력됨, 후에 전역 컨텍스트도 종료되면서 끝이 될 것임

    > 전역 컨텍스트
      > environmentRecord : {outer: f, outer2: inner }

    --- outer
      > environementRecord : { a: 3 } // 전역 컨텍스트 종료 전까지 살아남음
    ---

    outer 2 변수가 inner 함수를 물고 있는 상황에서
    inner 함수의 outerEnvironmentReference 로 부터 a 를 참조하기 때문임, 참조 카운트가 0 이 되지 않음

</br>

    참조 카운트를 0 으로 만들기 위해서는 어디선가 끊어주어야 함

    > outer2 변수에 다른 것을 대입하면 될 것임

    > inner 에 대한 참조카운트가 0 이 되면서 GC 대상이 됨

    > inner 함수 내부에서 참조하고 있는 대상 역시 연결고리가 끊김으로 함께 GC 가 됨

    > 전역 컨텍스트
      > environmentRecord : {outer: f, outer2: ~~inner~~ }

    ~~outer
      > environementRecord : { a: 3 }~~

</br>

이것이 바로 `특별한 현상` 이다.

</br>

outer 에 있는 변수가 사라지지 않고 계속 좀비처럼 살아남고 있는 이것이 `클로저`의 `특별한 현상`이다.

</br>

## `컨텍스트 A` 에서 선언한 `변수 a` 를 참조하는 `내부함수 B` 를

</br>

## `A` 의 `외부로 전달`할 경우, `A` 가 종료된 이후에도 `a` 가 `사라지지 않는 현상`

</br>
</br>
</br>

## 2. Closure 의 특징

</br>
</br>
</br>

함수 종료 후에도 사라지지 않는 지역변수를 만들 수 있다.

</br>

```js
function user(_name) {
  var _logged = true;
  return {
    get name() {
      return _name;
    }, // getter 호출
    set name(v) {
      _name = v;
    },
    login() {
      _logged = true;
    },
    logout() {
      _logged = false;
    },
    get status() {
      return _logged ? "login" : "logout";
    },
  };
}
var sue = user("수");
```

</br>

```js
console.log(sue.name); // '수'
```

    sue 객체에는 name 프로퍼티가 없는 대신 name 에 대한 getter 가 있기 때문에 getter 가 호출됨

    name: (...)
    statue: (...)
    > get name: f name() // getter
    > set name: f name(v)

</br>

    get name() {
      return _name;
    },

    에서 _name 변수는 user 함수를 호출할 때 매개변수로 넘겨받은 값을 가지는 user 함수에서 선언된 변수임

    원래대로라면 user 함수의 실행 컨텍스트가 종료됨과 동시에 함께 사라지는 변수였던 것임

    그런데 함수 실행을 종료할 때 return 해줄 내용 안에서 해당 변수를 사용하고 있는 함수가 있어 즉, 참조 카운트가 0 이 아닌 상태이므로 '나중에 쓸 변수' 로 판단하여 살려둠

    그리하여 sue.name 의 값이 '수' 이게 되는 것

</br>
</br>

```js
sue.name = "수갱"; // 프로퍼티 값 변경
console.log(sue.name); // '수갱'
```

    sue.name 프로퍼티 값을 '수갱' 으로 바꾸려고 함

    > set name 이 발동되어 _name 값이 '수갱' 으로 바뀌게 됨

    name: (...)
    statue: (...)
    > get name: f name()
    > set name: f name(v)

    이어서 get name 호출하면 '수갱' 이 반환됨

</br>
</br>

```js
sue._name = "sugy";
console.log(sue.name); // '수갱'
```

    sue._name 에 'sugy' 를 할당하는 명령을 낸다고 한다면

    sue 객체에는 _name 프로퍼티가 없음, 새로 만들게 됨

    이 명령에 의해서는 user 함수에서 선언한 _name 변수에는 어떠한 영향도 주지 못하게 됨

    그래서 name getter 호출에 대한 결과는 여전히 '수갱' 이 됨

</br>
</br>

```js
console.log(sue.status); // 'login'
```

    sue 객체에서 getter 인 status 를 호출한다면

    user 함수에서 선언된 _logged 변수의 값에 따라

    'login' 또는 'logout' 이라는 문자열을 반환함

    현재는 true 이기 때문에 'login' 을 출력함

    name: (...)
    statue: (...)
    > get name: f name()
    > set name: f name(v)
    > get status: f status()

    즉 _logged 변수 역시 클로저에 의해서 좀비가 된 것이 확인됨

</br>
</br>

```js
sue.logout();
console.log(sue.status); // 'logout'
```

    이 상태에서 logout 메서드를 호출하면,

    좀비변수 _logged 가 false 가 되고

    sue 객체에서 getter 인 status 를 호출한다면

    현재는 false 이기 때문에 'logout' 을 출력함

    name: (...)
    statue: (...)
    > get name: f name()
    > set name: f name(v)
    > get status: f status()

</br>
</br>

```js
sue.status = true;
console.log(sue.status); // 'logout'
```

    sue 객체에서 status 라고 하는 프로퍼티의 getter 는 있지만 setter 는 없음

    sue.status 에 true 를 할당하라는 명령을 하면 이 명령은 무시됨

    > {login: f, logout: f}
      > login: f login()
      > logout: f logout()
      name: (...)
      statue: (...)
      > get name: f name()
      > set name: f name(v)
      > get status: f status()

</br>

위의 기나긴 예제를 통해 알 수 있는 점을 정리해본다면 아래와 같다.

</br>

a) 함수 종료 후에도 사라지지 않고 값을 유지하는 변수

    첫 번째로 _name, 그리고 _logged 라고 하는 두 변수

    => 클로저의 핵심 개념

</br>

b) 외부로부터 내부 변수 보호 (캡슐화)

    get name()
    set name(v)
    login()
    logout()
    get status()

    외부에 노출된 status 프로퍼티는 getter 로서만 역할을 하며,

    실제 _logged 의 값과는 별개의 문자열 만을 반환해줌

    직접적으로 _logged 의 변화에 영향을 주는 것은

    login/logout 메서드에 의해서만 가능

</br>

## 함수 종료 후에도 사라지지 않는 지역변수를 만들 수 있다!
