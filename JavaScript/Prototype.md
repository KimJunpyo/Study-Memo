# Prototype

Created: 2025년 6월 3일 오후 9:56
Tags: JS
Date: 2025년 6월 6일

## prototype

JavaScript의 객체가 갖는 고유한 속성

- 모든 객체는 [[Prototype]] 이라는 내부 슬롯을 갖는다.
- 이 내부 슬롯에는 어떠한 생성자 함수의 prototype이 저장된다.
- [[Prototype]] 내부 슬롯은 직접 접근할 수 없고, __proto__라는 접근자 프로퍼티를 통해서만 호출 및 설정할 수있다.

## __proto__

[[Prototype]]에 저장된 prototype을 호출하거나 설정할 수 있도록 get, set 속성으로 구현된 접근자 프로퍼티

- __proto__ 접근자 프로퍼티는 각 객체가 소유하는 것이 아닌, Object.prototype이 갖고 있고 상속으로 내려주는 것이다.
- 해당 접근자 프로퍼티를 사용하는 이유는 직접적으로 Prototype에 값을 할당할 수 있게 된다면, 자칫 서로가 서로를 참조하는 순환 참조가 발생할 수 있기 때문이다.
이를 방지하기 위해 상속된 접근자 프로퍼티를 이용하여 해당 객체의 Prototype이 무엇인지 검사 후에 순환되는 상속인지 확인하는 것이다.
- 직접 상속을 통해 Object.prototype을 상속받지 않는 객체를 생성할 수도 있기 때문에 __proto__를 직접 코드내에서 로직적으로 사용하는것은 권장하지 않는다.
- Object.prototype에서 제공하는 Object.getPrototypeOf와 Object.setPrototypeOf를 이용하는 것을 권장한다.

## 함수의 prototype

함수는 function 키워드를 이용하여 정의한 함수(함수 선언문)일 때, 생성자 함수를 함께 생성한다.

해당 함수의 [[Prototype]] 에는 함수가 생성되었으니 Function.prototype을 상속받고 있게 되며, 자체적으로 prototype 프로퍼티도 갖게 된다.

이 prototype 프로퍼티에는 해당 함수의 생성자 함수가 존재한다.

그러나, arrow 함수나 함수 리터럴 문으로 구현한 함수의 경우에는 생성자 함수가 생성되지 않는다.

그리고 해당 함수들은 [[Prototype]] 에 Function.prototype이 상속되어 있지만, 함수가 prototype 프로퍼티를 갖지 않는다.

즉, 생성자 함수가 있어야 prototype 프로퍼티를 갖게 된다.

생성자 함수를 이용하여 new 키워드로 함수의 인스턴스를 구현하면, 해당 인스턴스의 [[Prototype]] 에는 생성자 함수의 prototype 프로퍼티가 연결되어 있다.

## 객체의 prototype

객체는 Object.create(null)로 구현하지 않는 이상, 모든 객체의 [[Prototype]] 에 Object.prototype을 상속받는다.

OrdinaryObjectCreate라는 추상 연산에 의해 생성되고 이때, [[Prototype]] 에 상속을 시키는 방식이다.

## 빌트인 자료구조의 prototype

array, set, string, number 등의 자료구조들은 각각 Array.prototype 등의 프로토타입을 갖는 생성자 함수들이 존재한다.

이 생성자 함수들의 [[Prototype]] 에는 Object.prototype이 있다.

JavaScript는 Object.prototype이 가장 최상단에 있는 prototype이다.

## prototype chain

객체가 자신의 prototype에 접근할 수 있고, 그 prototype도 다른 prototype을 가질 수 있는 부모-자식 관계의 prototype 연결 구조

자신의 prototype에서 프로퍼티를 검색하고 존재하지 않다면 prototype의 prototype에서 검색하는 식으로 prototype을 올라가며 찾는 메커니즘에서 사용된다.

## prototype 섀도잉

상위 prototype 어딘가에서 정의된 프로퍼티를 동일한 이름으로 하위 prototype에서 정의하여 오버라이딩 되는 현상