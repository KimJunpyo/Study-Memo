# Async, Await

Created: 2025년 6월 11일 오후 9:32
Tags: JS
Date: 2025년 6월 11일

## Async

비동기 함수인 Promise를 동기처럼 동작시키기 위한 비동기 제어 함수

- Async 함수는 항상 Promise를 반환한다.
- Async 함수 내부에서만 Await 키워드를 사용할 수 있다.

## Await

Promise가 settled 상태가 될 때 까지 함수의 실행을 일시 중지(블로킹)하는 키워드

- Await 키워드는 함수가 settled 상태가 되었을 때 resolve한 결과값을 반환한다.
- 함수가 일시 정지 되었을 때, 해당 함수의 실행 컨텍스트는 Suspend 상태가 되며 실행 컨텍스트의 환경들을 내부 엔진에 저장하며 콜 스택에서 pop 된다.
    - 이 때, await 이후의 함수들은 async 함수를 재개하는 작업으로 마이크로태스크 큐에 저장된다.

```jsx
const x = async () => {
  const response = await new Promise((resolve) => resolve(1));
  console.log(response);
};

x();
console.log(2);

// 2, 1 출력
```

1. async 함수인 x 함수 실행
2. Promise 생성자 함수 실행
3. Promise 함수가 resolve되는걸 기다리는 동안 await 키워드가 x 함수의 실행을 일시 중지하며 실행 컨텍스트의 상태가 Suspend가 됨
4. 해당 함수의 실행 컨텍스트는 pop됨
5. await 이후의 코드가 async 함수를 재개하는 작업으로 마이크로태스크 큐에 저장
6. x 함수가 일시 중지되었기에 console.log(2) 동작
7. 이후 실행 컨텍스트가 전부 종료
8. Promise가 settled 되었고 실행 컨텍스트가 전부 종료되었다면 마이크로태스크 큐에서 재개 작업 코드와 내부 엔진에 저장되어 있던 실행 컨텍스트의 환경을 합쳐 새로운 실행 컨텍스트를 생성하여 콜스택에 push되어 실행됨.