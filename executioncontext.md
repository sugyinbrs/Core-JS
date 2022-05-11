# Core-JS study

## 실행 컨텍스트

</br>

- 목차

1. 실행 컨텍스트  
   1-1. 실행 컨텍스트의 의미  
   1-2. 실행 컨텍스트 코드 예시와 콜스택  
   1-3. 실행 컨텍스트 내부 (환경 정보)
2. 실행 컨텍스트 정리

</br>
</br>
</br>

> ## 1. 실행 컨텍스트

</br>
</br>
</br>

### 1-1. 실행 컨텍스트의 의미

</br>
</br>
</br>

`실행 컨텍스트` 즉, `Execution Context` 의 단어 그대로 이해해보자면

`Execution` 는 `실행`, `Context` 는 `문맥, 맥락, 환경` 정도로 해석할 수 있다.

</br>

어떤 `코드`가 이 자리에서 `어떤 역할을 수행`하는 지를 이해하기 위해서는

그 `코드에 영향을 주는 주변 코드나 변수들을 파악`해야한다.

그런 영향을 주는 `환경`을 일컬어서 `Context` 라고 한다.

</br>

정리해보자면 `실행 컨텍스트` 는 `코드를 실행하는 데 필요한 배경이 되는 조건, 환경`이라고 할 수 있다.

</br>

    <Execution>
    동일한 조건 / 환경을 지니는 코드 뭉치


    <Context>
    를 실행할 때 필요한 조건 / 환경정보

</br>

> 자바스크립트 상에서 동일한 조건을 지닐 수 있는 조건이 3가지가 있다.

</br>

1. `전역공간` - 자바스크립트 코드가 실행되는 순간에 바로 전역 컨텍스트가 실행됨,
   전체 코드가 끝날 때에 전역 컨텍스트가 종료가 되므로 이를 하나의 거대한 함수 공간이라고 봐도 무방

</br>

2. `module` - module 역시 어디선가 import 되는 순간에 module 내부에 있는 컨텍스트가 생성됨,
   module 코드가 끝날 때에 컨텍스트가 종료되므로 이 역시 하나의 함수 공간이라고 봐도 무방

</br>

3. `함수` - 결국 `자바스크립트의 독립된 코드뭉치`라고 할 수 있는 것은 `함수`라고 볼 수 있음

</br>
</br>

즉, `전역 공간, 모듈, 또는 함수로 묶인 내부`에서는 결국 `"같은 환경 안에 있다"` 라는 것이 성립한다.  

`자바스크립트`는 오직 `함수에 의해서만 컨텍스트를 구분`할 수 있다.

</br>
</br>

### +) if, for, switch, while 문?

</br>

    모두 블록스코프이며, 별개의 실행컨텍스트를 생성하지 않는다.

</br>
</br>

### 즉, `Execution Context` 는 `함수를 실행할 때 필요한 환경정보를 담은 객체`이다.

</br>
</br>
</br>

### 1-2. 실행 컨텍스트 코드 예시와 콜스택

</br>
</br>
</br>

먼저 실행되는 순서부터 나열을 해본다면 1-2-3-4 순이다.

</br>
</br>

```js
var a = 1;
function outer() {
  console.log(a); // 1

  function inner() {
    console.log(a); // 2
    var a = 3;
  }

  inner();

  console.log(a); // 3
}
outer();
console.log(a); // 4
```

</br>

    1) 하단의 outer() 를 실행하는 명령을 만나고 outer() 를 호출

    2) function outer() 내부를 한 줄 씩 실행

    3) 1번 console.log 출력

    4) 중간의 inner() 를 실행하는 명령을 만나고 inner() 를 호출

    5) function inner() 내부를 한 줄 씩 실행

    6) 2번 console.log 출력

    7) function inner() 호출 종료

    8) 3번 console.log 출력

    9) function outer() 호출 종료

    10) 4번 console.log 출력

    11) 전역 컨텍스트 종료

</br>
</br>

    `first-in-last-out` 형태를 띄게 되는데 이것을 "스택" 이라고 한다.
    코드 실행에 관여하는 스택을 "콜스택" 이라고 한다.

</br>
</br>

> ## Call Stack

</br>
</br>

    콜스택은 현재 어떤 함수가 동작 중인지, 다음에 어떤 함수가 호출될 예정인지 등을 제어하는 자료구조

</br>
</br>

    처음에는 비어있음
    > 최초에 전역 공간에 대한 컨텍스트가 콜스택에 쌓임, 1)
    > 2)
    > 3)
    > inner() 실행

</br>
</br>

|   쌓이는 순서    |
| :--------------: |
|     3) inner     |
|     2) outer     |
| 1) 전역 컨텍스트 |

</br>
</br>

    inner() 호출 종료
    > 하나씩 순서대로 실행 후 호출 종료로 제거됨, 1)
    > 2) 실행 후 호출 종료 제거
    > 3) 실행 후 호출 종료 제거
    > 비어있게 되어 최종 종료

</br>
</br>

|  제거되는 순서   |
| :--------------: |
|     1) inner     |
|     2) outer     |
| 3) 전역 컨텍스트 |

</br>
</br>
</br>

### 1-3. 실행 컨텍스트 내부 (환경 정보)

</br>
</br>
</br>

실행 컨텍스트에는 3가지 환경 정보인

`Variable Environment`, `Lexical Environment`, `This Binding` 이 들어간다.

</br>
</br>

|               |                      |                                                                         |
| :-----------: | :------------------: | :---------------------------------------------------------------------: |
|     inner     | Variable Environment | environmentRecord (snapshot) </br> outerEnvironmentReference (snapshot) |
|     outer     | Lexical Environment  |            environmentRecord </br> outerEnvironmentReference            |
| 전역 컨텍스트 |     This Binding     |

</br>
</br>

`Variable Environment`, `Lexical Environment` 에는 `현재 환경과 관련된 식별자 정보들` 이 담긴다.

</br>
</br>

a) Variable Environment

- 식별자 정보를 수집하는 용도
- 컨텍스트 내부 코드들을 실행하는 동안 `변수의 값들이 변화`가 생기면 `변화 반영 X`

</br>

b) Lexical Environment (Lexical Environment 위주로 살펴봄)

- 식별자에 담긴 데이터를 추적하는 용도
- 컨텍스트 내부 코드들을 실행하는 동안 `변수의 값들이 변화`가 생기면 `변화 반영 O`

</br>
</br>

그 중 변화가 반영되는 `Lexical Environment` 위주로 살펴볼 것이다.

</br>
</br>

`Lexical Environment` 는 `어휘적 환경, 사전적인 환경` 이라고 한다.

</br>
</br>

이해하기 쉽게 보자면 `Lexical Environment` 를 아래와 같이 실행 컨텍스트 A 에 대한 `환경정보가 담겨 있는 사전`이라고 생각하면 된다.

</br>
</br>

    내부식별자 a : 현재 값은 undefined 이다.
    내부식별자 b : 현재 값은 20이다.
    외부 정보 : D를 참조한다.

</br>
</br>

### 즉, `Lexical Environment` 는 `실행컨텍스트를 구성하는 환경 정보들을 모아 사전처럼 구성한 객체`이다.

</br>
</br>

`Lexical Environment` 에는 `environmentRecord`(식별자 정보 수집) 와 `outerEnvironmentReference`(외부 환경에 대한 참조) 가 있다.

</br>
</br>

a) environmentRecord

- 현재 문맥의 `식별자 정보가 수집`됨 (`실행컨텍스트가 최초 실행 시 제일 먼저 수행`)

- 현재 컨텍스트 `식별자 정보들을 수집`해서 environmentRecord 에 담는 과정을 `"Hoisting"` 으로 생각
  - `Hoisting` 은 실제하는 현상은 아니고 environmentRecord 정보 수집 과정을 쉽게 이해하기 위해 만든 허구의 개념
  - 실행 컨텍스트 맨 위로 `식별자 정보를 끌어올린다` 라는 뜻

</br>

+) Hoisting 예시

</br>

> Hoisting 전

</br>

```js
console.log(a());
console.log(b());
console.log(c());

function a() {
  return "a";
} // 함수 선언문 - 다른 식별자와 다르게 함수선언문은 전체를 끌어올리는 특징이 있음

var b = function bb() {
  return "bb";
};

var c = function () {
  return "c";
};
```

</br>

> Hoisting 후

</br>

```js
// 여기부터
function a() {
  return "a";
}
var b;
var c;
// 여기까지 내용 전체가 environmentRecord 에 수집됨

console.log(a());
console.log(b());
console.log(c());

b = function bb() {
  return "bb";
};

c = function () {
  return "c";
};
```

</br>
</br>

b) outerEnvironmentReference

- `외부의 환경(Lexical Environment)에 대한 참조`

- 즉, 현재 문맥과 관련 있는 `외부 식별자 정보를 참조`
  - 예를 들어 현재 inner 문맥을 실행 중이라고 하였을 때  
    inner 문맥의 outerEnvironmentReference 는 아래 outer 라고 하는 Lexical Environment를 참조함
  - 마찬가지로 outer 문맥의 outerEnvironmentReference 는 아래 전역컨텍스트의 Lexical Environment 를 참조

- 그로 인해 `Scope Chain` 에 관여하게 됨 (outerEnvironmentReference 에 의해 만들어짐)
  - inner 컨텍스트에서 선언한 변수는 environmentRecord 에 의해 접근 가능,  
    outerEnvironmentReference 에 의해 outer 컨텍스트에서 선언한 변수에도 접근 가능
  - outer 컨텍스트에서 선언한 변수는 environmentRecord 에 의해 접근 가능,  
    outerEnvironmentReference 에 의해 전역 컨텍스트에서 선언한 변수에도 접근 가능
  - 반대로 inner 에서 선언한 변수는 outer 컨텍스트에서 접근 불가,  
    outer 에서 선언한 변수는 전역 컨텍스트에서 접근 불가
    
- 이것이 `Scope` 임, 외부로는 나갈 수 있으나 자기보다 안쪽으로는 들어갈 수 없음, `변수의 유효범위를 의미`함
  - inner 함수에서 선언한 변수의 유효범위는 inner 함수 내부에만 국한됨
  - outer 함수에서 선언한 변수의 유효범위는 inner 와 outer 임, 전역에서 접근 불가
  - 전역 공간에서 선언한 변수는 outer 와 inner 에서 접근 가능

- 결국 `Scope Chain` 은 inner 에서 `특정 변수를 찾을 때 가장 가까운 자기 자신부터 점점 멀리 있는 스코프로 찾아 나가는 것`

- `가장 먼저 찾아진 것만 접근할 수 있는 개념` 을 `Shadowing` 이라고 함

</br>
</br>

|               |                      |                                                                         |
| :-----------: | :------------------: | :---------------------------------------------------------------------: |
|     inner     | Variable Environment | environmentRecord (snapshot) </br> outerEnvironmentReference (snapshot) |
|     outer     | Lexical Environment  |            environmentRecord </br> outerEnvironmentReference            |
| 전역 컨텍스트 |     This Binding     |

</br>
</br>
</br>

### 2. 실행 컨텍스트 정리

</br>
</br>
</br>

    Execution Context : 함수를 실행할 때 필요한 환경 정보를 담은 객체
    - Variable Environment
    - Lexical Environment
      - environmentRecord : 현재문맥의 식별자 (hoisting)
      - outerEnvironmentReference : 외부 식별자 (scope chain)
    - this

</br>
</br>
