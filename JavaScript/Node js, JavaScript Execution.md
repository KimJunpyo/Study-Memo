# Node.js, JavaScript Execution

Created: 2025년 5월 28일 오전 10:22

## JavaScript의 실행 방식

1. JavaScript 엔진이 존재하는 런타임 환경이 실행됨
2. 런타임 환경에서 실행할 코드(ex. node index.js 라면 실행할 코드는 index.js)를 호출
3. 호출된 코드는 바이트 코드로의 변환 과정을 거침
    1. 소스코드를 토큰으로 분해하는 Tokenizing 작업
    2. 토큰을 AST(Abstract Syntax Tree)로 변환하는 Parsing 작업
    3. AST를 기반으로 인터프리터가 바이트코드를 생성
4. 파싱이 끝난 뒤, JavaScript 엔진이 코드를 평가함
5. 전역 실행 컨텍스트를 생성하며 call stack에 push
    1. 이때, JavaScript 엔진의 내부 자료구조에서 파싱된 코드와 실행 컨텍스트를 매핑하는 어떠한 내부 방식 동작
6. 전역 실행 컨텍스트에 해당 함수에 대한 식별자들을 렉시컬 환경의 환경 레코드에 선언 및 저장
7. 평가가 종료된 뒤, 콜스택은 전역 실행 컨텍스트를 실행함
8. JavaScript 엔진에 전역 실행 컨텍스트와 매핑된 추상화된 소스코드 읽으며 실행
9. 함수 실행문을 만나면 함수 실행 컨텍스트를 만들면서 5~8번 반복
10. 실행문 종료 후, call stack에서 pop
11. 코드 실행이 종료된 후, 전역 실행 컨텍스트를 call stack에서 pop
12. 런타임 환경 종료

## 런타임

프로그램이 실행중인 상태

## 런타임 환경

프로그램이 실행되는데 필요한 환경

프로그램을 실행될 때 필요한 모든 시스템, 내부적 자원들을 갖춘 것

## Node.js

JavaScript 런타임 환경

JavaScript 코드가 실행될 수 있도록 하는 환경을 의미한다.

내부적으로 JavaScript 엔진, 내장 모듈, 이벤트루프 등 다양한 JavaScript의 실행을 지원하는 자원을 제공한다.

File System 접근이나 네트워크 소켓 등의 기능을 지원한다.

## 브라우저에서의 JavaScript

모든 브라우저는 JavaScript 엔진을 탑재하고 있다.

엔진을 제외한 이벤트루프 등 Node.js 에서 내장하고 있는 기능들을 대신하여 Web API를 이용해 동작을 구현한다.