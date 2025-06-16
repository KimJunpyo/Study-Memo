# Promise

Created: 2025년 6월 10일 오후 11:52
Tags: JS
Date: 2025년 6월 10일

## Promise

비동기 작업의 결과를 처리하기 위한 객체

## Promise 구조

- 비동기 처리에 대한 상태값
    - pending(대기), fulfiled(성공), rejected(실패)
    - fulfiled, rejected 상태는 settled 상태라고도 부른다.
- 비동기 처리에 대한 결과값

## Promise 후속 메소드

### then

then(성공 callback, 실패 callback) 두 가지 인자를 받아 Promise의 상태에 따라 callback을 동작시키는 메소드

- 성공 callback과 실패 callback 둘 다 Promise를 반환한다.

```jsx
const p = new Promise((resolve) => resolve(1));
console.log(p.then((result) => console.log(result)));

// Promise<Pending> 출력, 1 출력
```

- 성공 callback의 결과값으로 resolve가 되어 1이 출력된다.
- 하지만 then 자체의 반환값은 Promise로 결과값을 Promise로 만들어 반환한다.

### catch

catch(에러 callback) 으로 Promise에서 rejected 상태가 되었을 때 동작시키는 메소드

- then(undefined, 에러 callback)과 같은 코드이다.

### finally

finally(callback) 성공과 실패 여부에 상관없이 반드시 한번 callback을 실행시키는 메소드

## Promise 코드 패턴

```jsx
const p = new Promise((_, reject) => reject(new Error("error")));

p.then(() => console.log(1), (err) => console.log(err));

p.then(() => console.log(1)).catch((err) => console.log(err));
```

- 두 코드의 예시 전부 비동기 처리가 실패했을 때를 위해 구현한 코드이다.
- 그러나 만약 실제 api 호출이 이루어지고 네트워크의 상태에 따라 성공과 실패가 결정된다면 두번째 패턴을 추천한다.
    - 첫번째 패턴은 성공했을 때의 callback에서 어떤 에러가 발생하더라도 해당 에러를 캐치할 수 없다.
    - 두번째 패턴은 Promise가 실패했을 때 catch가 동작되고, 성공했을 때 then의 성공 callback 내에서도 에러가 발생한다면 catch가 해당 에러를 캐치할 수 있다.
    - 따라서 두번째 방식이 에러 핸들링에 유용한 패턴이다.

## Promise와 마이크로태스크 큐

1. Promise는 new Promise 생성자 함수로 생성할 때, 내부 함수를 곧바로 실행한다.
2. 내부 함수가 실행되며, 함수가 종료되며 Promise의 상태가 결정된다
3. 함수의 결과값이 결정되었고 후속 메소드가 존재한다면, 결과값에 따라 then과 catch 메소드 중 하나가 실행된다.
4. then, catch 메소드의 callback 함수는 마이크로태스크 큐에 저장되며, 실행 컨텍스트가 빌 때 까지 대기한다.
5. 이후, 실행 컨텍스트가 모두 종료되면 마이크로태스크 큐에서 이벤트루프가 콜 스택으로 callback을 옮기고, JavaScript Engine은 실행한다.

```jsx
new Promise((resolve) => resolve(1)).then(result => console.log(result))
	.then(result => console.log(result)).catch((err) => console.log(err));
console.log("동기");
```

1. Promise 생성자 함수 실행 ⇒ resolve 함수를 이용하여 성공시켰기 때문에 then 메소드의 callback을 마이크로태스크 큐에 저장
2. “동기” 메세지 출력
3. 전역 실행 컨텍스트 종료 후, 첫번째 then의 callback을 실행
4. then 함수에서 에러가 나지 않고 종료되어, fulfiled값을 갖는 Promise를 반환
5. 두번째 then 메소드의 callback이 마이크로태스크 큐에 저장
6. 실행 컨텍스트가 비어있기 때문에 두번째 callback 실행
7. catch는 error throw가 되지 않았기 때문에 동작하지 않음

### Promise.all에서의 특징

- 하나라도 Promise가 rejected되면 곧바로 모든 Promise 비동기 함수들을 종료한다.
- Promise.all의 배열 요소로 Promise가 아닌 인자를 건네면, Promise.resolve로 감싼다.

### Promise.race와의 차이점

Promise.race는 요소들 중 하나라도 성공한다면 곧바로 resolve한 Promise를 반환하고 종료하지만, all은 요소가 전부 성공해야만 Promise를 반환한다.

### Promise.allSettled

요소들의 결과에 상관없이 모든 결과를 Promise로 반환한다.

- 성공한 경우, 실패한 경우에 관계가 없다.