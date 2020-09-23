# 함수형 프로그래밍과 JavaScript ES6+

- 일급 함수 - 함수가 값으로 다뤄질 수 있다.

  ```javascript
  // 함수가 값으로 사용되기때문에 add5라는 변수에 담을 수 있다.
  const add5 = a => a + 5;
  
  ```

- 고차 함수 - 함수를 값으로 다루는 함수

  - apply1

  - times

    ```javascript
    // apply1
    const apply1 = f => f(1);
    const add2 = x => x + 2;
    console.log(apply1(add2)); // 3
    
    // times
    const times = (fn, n) => {
      let i = 0;
      while(i++ < n) fn(i);
    }
    
    times(console.log, 3);
    // 1
    // 2
    // 3
    ```

    

  - 함수를 만들어 리턴하는 함수 (클로저를 만들어 리턴하는 함수)

    - addMaker

      ```javascript
      const addMaker = a => b => a + b
      
      const add10 = addMaker(10);
      console.log(add10(5)); // 15
      console.log(add10(10)); // 20
      ```

      

- for of

  - iterable 객체만 가능하다. (Array, `string`, Map, Set 등..)

  - for of 는 for문 처럼 i번째 값을 가져오는 방식이 아님. 

    ```javascript
    const set = new Set([1,2,3]);
    console.log(set[0]); // undeinfed
    ```

  - 

  - 이터러블 : 이터레이터를 리턴하는 [Symbol.iterator] () 을 가진 값
  - 이터레이터: {value, done} 객체를 리턴하는 next() 를 가진 값
  - 이터러블/이터레이터 프로토콜



- 사용자 정의 iterator 객체만들기

  ```javascript
  // i가 0이 될 때까지 for of를 돌 수 있다.
  const myIterable = {
    [Symbol.iterator](){
      let i = 3;
      return {
      	next(){
          return i === 0 ? {
            done: true
          }:
          {
            value: i--,
            done: false
          }
        }
      }
    }
  }
  
  const iterator = myIterable[Symbol.iterator]();
  
  for(let i of myIterable) console.log(i);
  // 3
  // 2
  // 1
  
  for(let i of iterator) console.log(i);
  // Uncaught TypeError: iterator is not iterable
  ```

  이처럼 만들 수 있지만 완벽한 iterable 객체라고 할 수 없다. 왜냐하면 제대로 구현된 iterable 객체의 iterator 도 iterable 객체여야 하며 next()를 n번 실행한 뒤 해당 iterator를 순회하면 n번 진행된 이후만 순회되어야 한다. 아래 코드 참고

  ```javascript
    const arr = [1,2,3,4,5]; 
  
    const arrIterator = arr[Symbol.iterator](); // arr의 iterator를 변수에 담은 후
  
    console.log(arrIterator[Symbol.iterator]() === arrIterator) // true ->  자기자신을 반환하는 Symbol.iterator를 갖고있다.
  
    cosole.log(arrIterator.next()) // {value: 1, done: false}
  
    /** arrIterator 또한 Symbole.iterator 를 갖고있기 때문에 for of 문 가능
    * 위에서 arrIterator의 next 메소드를 실행했기 때문에 2부터 실행
    */
    for(let i of arrIterator) console.log(i);	
    // 2
    // 3
    // 4
    // 5
  
  ```

  동일하게 동작하려면 `myIterable` 의 Symbol.iterator 에 코드를 추가해 주어야한다. 

  ```javascript
  const myIterable = {
    [Symbol.iterator](){
      let i = 3;
      return {
      	next(){
        	...	
        },
        [Symbol.iterator]() {	// 자기 자신을 반환하는 메소드 추가.
          return this;
        }
      }
    }
  }
    
  const iterator = myIterable[Symbol.iterator]();
  
  iterator.next();
  
  for(let i of iterator) console.log(i);
  // 2
  // 1
  // 이제 에러가 나지않고 순회 가능!
  ```

- Array, Set 등 뿐만 아니라 브라우저에서 지원하는 Web Api들 등.. 자바스크립트의 순회가 가능한 대부분의 것들은 이터러블,이터레이터 프로토콜을 따른다.
  - document.querySelectorAll('*')을 통해 반환되는 `NodeList` 등..