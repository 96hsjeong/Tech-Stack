# ES6문법 - Promise
*written by sohyeon, hyemin 💡*

<br>

## 1. Promise란?

자바스크립트는 비동기 처리를 위한 하나의 패턴으로 콜백함수를 사용한다. 하지만 전통적인 콜백 패턴은 가독성이 나쁘고 **비동기 처리 중 발생한 에러의 예외 처리가 곤란**하며 여러개의 비동기 처리 로직을 한꺼번에 처리하는 것도 한계가 있다.  
ES6에서 비동기 처리를 위한 또 다른 패턴으로 프로미스(Promise)를 도입했다. 콜백 패턴이 가진 단점을 보완하며 **비동기 처리 시점을 명확하게 표현한다.**  

## 2. 생성

프로미스는 생성자 함수를 통해 인스턴스화 한다.  
Promise 생성자 함수는 비동기 작업을 수행할 콜백 함수를 인자로 전달받는데  
이 콜백 함수는 resolve와 reject 함수를 인자로 전달 받는다.  

```Javascript
// Promise 객체의 생성
const promise = new Promise((resolve, reject) => {
  // 비동기 작업을 수행한다.

  if (/* 비동기 작업 수행 성공 */) {
    resolve('result');
  }
  else { /* 비동기 작업 수행 실패 */
    reject('failure reason');
  }
}
```
Promise는 비동기 처리가 성공(fulfilled)하였는지 또는 실패(rejected)하였는지 등의 상태(state) 정보를 갖는다.  

Promise 생성자 함수가 인자로 전달받은 콜백 함수는 내부에서 비동기 처리 작업을 수행한다.  
이때 비동기 처리가 `성공`하면 콜백 함수의 인자로 전달받은 resolve 함수를 호출한다.  
이때 프로미스는 `fulfilled`(비동기 처리 수행 성공) 상태가 된다.  
비동기 처리가 `실패`하면 `reject` 함수를 호출한다. 이때 프로미스는 `rejected`(비동기 처리 수행 실패) 상태가 된다.  

### 2-1. 예시
```Javascript
const promiseAjax = (method, url, payload) => {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open(method, url);
    xhr.setRequestHeader('Content-type', 'application/json');
    xhr.send(JSON.stringify(payload));

    xhr.onreadystatechange = function () {
      // 서버 응답 완료가 아니면 무시
      if (xhr.readyState !== XMLHttpRequest.DONE) return;

      if (xhr.status >= 200 && xhr.status < 400) {
        // resolve 메소드를 호출하면서 처리 결과를 전달
        resolve(xhr.response); // Success!
      } else {
        // reject 메소드를 호출하면서 에러 메시지를 전달
        reject(new Error(xhr.status)); // Failed...
      }
    };
  });
};
```
위 예제처럼 비동기 함수 내에서 Promise 객체를 생성하고 그 내부에서 비동기 처리를 구현한다. 이때 비동기 처리에 성공하면 resolve 메소드를 호출한다. 이때 resolve 메소드의 인자로 비동기 처리 결과를 전달한다. 이 처리 결과는 Promise 객체의 후속 처리 메소드로 전달된다.  
만약 비동기 처리에 실패하면 reject 메소드를 호출한다. 이때 reject 메소드의 인자로 에러 메시지를 전달한다. 이 에러 메시지는 Promise 객체의 후속 처리 메소드로 전달된다.

## 3. 후속 처리 메소드

Promise로 구현된 비동기 함수는 Promise 객체를 반환하여야 한다.   Promise로 구현된 비동기 함수를 `호출하는 측`(promise consumer)에서는 Promise 객체의 후속 처리 메소드(then, catch)를 통해 비동기 처리 결과 또는 에러 메시지를 전달받아 처리한다. Promise 객체는 상태를 갖는다고 하였다. 이 **상태에 따라 후속 처리 메소드를 체이닝 방식으로 호출**한다. Promise의 후속 처리 메소드는 아래와 같다.

* `then 메소드`  
    두 개의 콜백 함수를 인자로 전달 받는다.  
    첫 번째 콜백 함수는 성공(fulfilled, resolve 함수가 호출된 상태) 시 호출되고  
    두 번째 함수는 실패(rejected, reject 함수가 호출된 상태) 시 호출된다.  
    then 메소드는 Promise를 반환한다.

* `catch 메소드`  
    예외(비동기 처리에서 발생한 에러와 then 메소드에서 발생한 에러)가 발생하면 호출된다.  
    catch 메소드는 Promise를 반환한다.  

### 3-1. 예시
```Javascript
/*
    비동기 함수 promiseAjax은 Promise 객체를 반환한다.
    Promise 객체의 후속 메소드를 사용하여 비동기 처리 결과에 대한 후속 처리를 수행한다.
*/
promiseAjax('GET', 'http://jsonplaceholder.typicode.com/posts/1')
  .then(JSON.parse)
  .then(
  // 첫 번째 콜백 함수는 성공(fulfilled, resolve 함수가 호출된 상태) 시 호출된다.
  render,
  // 두 번째 함수는 실패(rejected, reject 함수가 호출된 상태) 시 호출된다.
  console.error
);

```

## 4. 에러 처리

비동기 처리 시 발생한 에러 메시지는 `then 메소드의 두 번째 콜백 함수`로 전달된다.  
Promise 객체의 후속 처리 메소드 `catch`를 사용해도 에러를 처리할 수 있다.

### 예제
```Javascript
promiseAjax('GET', 'http://jsonplaceholder.typicode.com/posts/1')
  .then(JSON.parse)
  .then(render)
  .catch(console.error);
```

then 메소드의 두 번째 콜백 함수는 비동기 처리에서 발생한 에러(reject 함수가 호출된 상태)만을 캐치한다.  
하지만 catch 메소드는 비동기 처리에서 발생한 에러(reject 함수가 호출된 상태)뿐만 아니라 then 메소드 내부에서 발생한 에러도 캐치한다.  
따라서 에러 처리는 catch 메소드를 사용하는 편이 보다 효율적이다.  

## 5. 프로미스 체이닝

비동기 함수의 처리 결과를 가지고 다른 비동기 함수를 호출해야 하는 경우,  
함수의 호출이 중첩(nesting)이 되어 복잡도가 높아지는 `콜백 헬`이 발생한다. **프로미스는 후속 처리 메소드를 체이닝(chainning)하여 여러 개의 프로미스를 연결하여 사용할 수 있다**. 이로써 콜백 헬을 해결한다.

Promise 객체를 반환한 비동기 함수는 프로미스 후속 처리 메소드인 then이나 catch 메소드를 사용할 수 있다.  
따라서 then 메소드가 Promise 객체를 반환하도록 하면(then 메소드는 기본적으로 Promise를 반환한다.) 여러 개의 프로미스를 연결해 사용할 수 있다.

<br>

## Reference & Additional Resources
* [프로미스](https://poiemaweb.com/es6-promise)