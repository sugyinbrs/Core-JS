# Core-JS study

## Class

</br>

- 목차

1. Class  
   1-1. Class 의 의미와 용어 정리  
   1-2. Class 의 실제 활용 사례  
   1-3. Class 정리
2. Class Inheritance (클래스 상속)  
   2-1. Class 상속 구조 만드는 과정

</br>
</br>
</br>

## 1. Class

</br>
</br>
</br>

## 1-1. Class 의 의미와 용어 정리

</br>
</br>
</br>

`Class` 는 사전적 의미로는 계급, 집단, 집합이라고 풀이되는데

</br>

이를 풀어서 해석하면 `공통적인 속성`을 모아 `한데 묶은 덩어리` 또는 `명세` 라고 할 수 있다.

</br>

또한 클래스와 함께 등장하는 용어로 `인스턴스` 가 있는데

</br>

`인스턴스`는 해당 클래스의 속성을 지닌 구체적인 객체들이다.

</br>

예를 들어 귤, 바나나, 사과, 오렌지, 배 등은 `인스턴스`이며 과일은 `클래스` 이다.

</br>

또한 과일은 음식이라고 하는 큰 범주에 속하기 때문에 과일은 음식보다 하위 클래스에 속하게 된다.

</br>

그런데 귤, 바나나, 사과, 오렌지, 배 등은 직접 볼 수도, 먹을 수도 있는 `구체적인 개념`이고

</br>

과일이나 음식 등은 `추상적인 개념`이다.

</br>

이처럼 어떤 공통된 특성을 지니는 구체적인 대상들을 `인스턴스` 라고 하고

</br>

인스턴스들의 공통 속성을 모은 추상적인 개념이 곧 `클래스` 이다.

</br>
</br>

하지만 `프로그래밍 언어`에서는 `상위의 클래스`가 `먼저 정의` 되어야 하위 클래스 및 인스턴스를 생성할 수 있다.

</br>

즉, 음식 클래스가 먼저 정의 되어야 음식 클래스의 속성을 지니면서 그 중에서 좀 더 구체적인 특성을 지니는 하위 클래스인 과일 클래스를 정의할 수 있다.

</br>

과일 클래스가 정의 되면 귤, 바나나, 사과, 오렌지, 배 등의 구체적인 객체들이 음식이기도 하면서 과일인 인스턴스가 될 수 있다.

</br>

음식 클래스는 과일 클래스에게 상위 클래스 (superclass) 이다.

</br>

반면 과일 클래스는 음식 클래스에게 하위 클래스 (subclass) 이다.

</br>
</br>
</br>

## 1-2. Class 의 실제 활용 사례

</br>
</br>
</br>

우리가 배열 리터럴을 생성할 때 Array 라는 생성자 함수를 new 연산자와 함께 호출한 결과와 같다고 알고 있다.

</br>

그 중 배열 리터럴이 아닌 `Array 생성자 함수 부분`만 분리해서 본다면 그 부분이 바로 일반적인 개념상의 `클래스` 역할을 담당한다.

</br>

왜냐하면 `Array 생성자 함수`는 그 자체로 어떤 역할을 수행한다기 보다 주로 `new 연산자`를 통해 `생성한 배열 객체들의 기능`을 `정의`하는 데에 주력하고 있기 때문이다.

</br>

그런가 하면, `클래스를 통해 생성한 객체` 즉, `배열 객체`를 `인스턴스`라고 한다.

</br>

`구체적인 데이터`를 가지고 `실제 코드 상`에서 `동작`을 `수행`하는 실체 중 하나이다.

</br>

![img-17](./img/img-17.png)
출처 - 인프런 코어자바스크립트

</br>
</br>

이 중에서 Class 파트만 떼어 놓고 본다면 prototype 프로퍼티 내부에 할당되지 않고 `Array 생성자함수 객체`에 `직접 할당`되어 있는 `프로퍼티들(isArray, of, arguments 등)` 을 `static methods`, `static property`라고 한다.

</br>

이들은 `Array 생성자 함수`를 new 연산자 없이 `함수(객체)`로써 `호출`할 때에만 의미가 있는 값들이다.

</br>

보통 `해당 클래스 소속`의 `인스턴스들`의 개별적인 동작이 아닌 `소속 여부 확인, 소속 부여` 등의 `공동체적인 판단`을 `필요로 하는 경우`에 `스테틱 메서드`를 `활용`하곤 한다.

</br>

한편 `prototype 내부에 정의된 메서드`들은 prototype 을 생략하고 `메서드`라고 하는 경우가 많다.

</br>

클래스와 인스턴스 관계를 살펴본다면 `스테틱한 값`들은 클래스 왼쪽에, 그리고 `프로토타입 메서드`는 클래스 오른쪽에 두었을 때 `인스턴스로부터 직접 접근`이 `가능`한지 `여부`가 서로 `다르다`.

</br>

`프로토타입 프로퍼티`는 인스턴스와 [[Prototype]] 으로 연결되어 있고 [[Prototype]] 가 인스턴스와 겹쳐서 동작하기 때문에 `인스턴스`에서 직접 `프로토타입 메서드`에 `접근 가능`한 반면,

</br>

`생성자 함수 내부`에 있는 `프로퍼티나 메소드(스테틱)` 는 직접 `접근할 방법이 없다`.

</br>
</br>

구체적인 코드를 통해 살펴본다.

</br>

```js
function Person(name, age) {
  this._name = name;
  this._age = age;
}

Person.getInformations = function (instance) {
  return {
    name: instance._name,
    age: instance._age,
  };
};

Person.prototype.getName = function () {
  return this._name;
};

Person.prototype.getAge = function () {
  return this._age;
};
```

</br>

      이 중에서 Person.getInformations 는 static method

      나머지 Person.prototype.getName 와 Person.prototype.getAge 는 프로토타입 메서드

</br>

sue 라는 Person 의 인스턴스를 생성한다.

```js
var sue = new Person("수", 100);

console.log(sue.getName()); // 출력 ok
console.log(sue.getAge()); // 출력 ok

console.log(sue.getInformations(sue)); // Error
console.log(Person.getInformations(sue)); // 출력 ok
```

</br>

      프로토타입 체이닝은 대각선으로만 검색하기 때문에 스테틱 메서드에서 제대로 된 결과를 얻기 위해서는 인스턴스가 아닌 생성자 함수에서 직접 접근해야 제대로 된 결과를 얻을 수 있음

</br>
</br>
</br>

## 1-3. Class 정리

</br>
</br>
</br>

![img-18](./img/img-18.png)
출처 - 인프런 코어자바스크립트

</br>

`클래스`는 `어떤 공통된 속성`이나 `기능`을 `정의`한 `추상적인 개념`이고,

</br>

이 `클래스에 속한 객체`를 `인스턴스`라고 한다.

</br>

`클래스`에는 인스턴스에서 직접 접근할 수 없는 `클래스 자체에서만 접근 가능`한 `스테틱 메소드, 프로퍼티`가 있으며,

</br>

`인스턴스에서 직접 활용`할 수 있는 `프로토타입 메서드`가 있다.

</br>
</br>
</br>

## 2. Class Inheritance (클래스 상속)

</br>
</br>
</br>

![img-19](./img/img-19.png)
출처 - 인프런 코어자바스크립트

</br>

Person 이라고 하는 클래스에는 name, age 프로퍼티가 있고, getName 과 getAge 라는 메서드를 사용할 수 있다고 친다.

</br>

한편 Employee 라는 클래스에는 name, age, position 프로퍼티가 있고, getName, getAge, getPosition 이라는 메서드를 사용할 수 있다고 친다.

</br>

그런데 겹치는 메서드가 보인다. 그림 상으로는 프로퍼티에 있는 getName, getAge 가 겹친다.

</br>

이를 Person 클래스가 Employee 상위에 있는 구조로 만들어주면 해결 될 것이다.

</br>

겹치는 메서드는 상위인 Person 에만 두고 Employee 에는 겹치지 않는 메서드를 둔다.

</br>

그렇게 만든 Employee 의 인스턴스는 프로토타입 체이닝을 타고 Employee 의 getPosition 메서드 뿐만 아니라 Person 의 getName, getAge 메서드도 사용할 수 있게 된다.

</br>

![img-20](./img/img-20.png)
출처 - 인프런 코어자바스크립트

</br>

그렇다면 이러한 다중 상속 구조는 어떻게 만드는 지 알아본다.

</br>
</br>
</br>

## 2-1. Class 상속 구조 만드는 과정

</br>
</br>
</br>

그림을 상상해 본다면 Employee 의 오른쪽 꼭짓점과 Person 의 아랫쪽 꼭짓점을 연결하면 된다.

</br>

그것을 코드로 표현한다면 `Employee.prototype = new Person()` 이다.

</br>

`Employee 의 prototype` 에 `새로운 Person 인스턴스`를 `연결`하면 된다.

</br>

하지만 위의 코드로는 Employee.prototype 를 새로운 객체로 대신하는 결과가 나오기 때문에 기존의 프로토타입과 동일하게 동작하도록 해주기 위해서는 본래 갖고 있던 기능을 다시 부여해줄 필요가 있다.

</br>

`프로토타입의 constructor` 를 `Employee 로 지정`해주는 과정이 필요하다.

</br>

`Employee.prototype.constructor = Employee`

</br>

이렇게 하면 서로 다른 클래스가 슈퍼 클래스, 서브 클래스 구조를 갖게 된다.

</br>

위의 구조를 코드를 통해 살펴본다.

</br>

```js
function Person(name, age) {
  this.name = name || "이름없음";
  this.age = age || "나이모름";
}

Person.prototype.getName = function () {
  return this.name;
};

Person.prototype.getAge = function () {
  return this.age;
};

function Employee(name, age, position) {
  this.name = name || "이름없음";
  this.age = age || "나이모름";
  this.position = position || "직책모름";
}
```

</br>

이 상태에서 앞서 봤던 내용들을 구현해본다.

</br>

```js
Employee.prototype = new Person();
Employee.prototype.constructor = Employee;
Employee.prototype.getPosition = function () {
  return this.position;
};
```

    맨 아래줄 코드가 아래에 있는 이유는

    맨 위의 Employee.prototype = new Person(); 코드보다

    먼저 상위에서 프로토타입에 어떤 프로퍼티나 메서드를 정의해봤자

    Employee.prototype 참조를 새로운 객체인 new Person 인스턴스로 바꿔치울 것이기 때문에 의미 없게 됨

    따라서 프로토타입을 덮어 씌운 다음에 정의를 할 수 밖에 없음

</br>

위와 같은 상태에서 Employee 의 인스턴스를 생성하고 내용을 출력하면 아래와 같다.

</br>

```js
var sue = new Employee("수", 100, "dev");
console.dir(sue);
```

</br>

![img-21-1](./img/img-21-1.png)

    sue 라는 객체 자신이 갖는 프로퍼티

    sue 객체는 Employee 에 new 연산자를 통해 생성한 Employee 인스턴스

</br>

![img-21-2](./img/img-21-2.png)

    sue 의 [[Prototype]] 영역

    Employee 의 prototype 과 같음

    또한 상속 관계를 부여하는 가장 핵심 포인트였던

    Employee.prototype = new Person() 을 떠올려보면 곧 Person 의 인스턴스임을 알 수 있음

    그래서 age, name 프로퍼티가 보임

    constructor 로는 우리가 부여해준 Employee 가 확인됨

    Employee 인스턴스가 사용할 getPosition 도 보임

</br>

![img-21-3](./img/img-21-3.png)

    sue.__proto__ 에 다시 __proto__ 를 찾아간,

    프로토타입 체이닝을 두 번 건너 뛴 내용

    Person 의 프로토타입

    따라서 constructor 가 Person 생성자 함수를 가리키고 있음

    마지막 줄 [[Prototype]] 에는 Object 가 표시되어 있는데 사실 Object.prototype 을 가리킴

    즉, Person.prototype 은 Object 의 인스턴스가 되는 것임

</br>

문제가 되는 부분이 있다.

![img-21-4](./img/img-21-4.png)

    위와 같은 정보가 추상적이어야 할 클래스의 프로토타입에 담겨 있는 것이 바람직하지 않음

    만약 실수로 sue 객체에서 name 프로퍼티를 지워버린 상태에서

    getName 메서드를 호출한다면

    원래대로라면 undefined 가 반환되어야 할 상황인데

    실제로는 프로토타입 체이닝을 타고 '이름없음' 이라는 엉뚱한 결과가 반환될 것


    프로토타입 체이닝 상에는 프로퍼티가 아닌 메소드들만 존재 하게끔 하는 것이 '추상적인 클래스' 라고 하는 정의에 부합할 것임

    name: '이름없음', age: '나이모름' 을 지워버리고 싶음


    생각해보면 각 클래스의 프로토타입을 [[Prototype]] 로 내려 받는 무언가가 필요한 것이지,

    반드시 Person 의 인스턴스가 필요한 것이 아님

    즉, Person.prototype 을 상속 받는 별도의 인스턴스가 있고

    그 객체에는 아무런 프로퍼티도 존재하지 않으면 됨