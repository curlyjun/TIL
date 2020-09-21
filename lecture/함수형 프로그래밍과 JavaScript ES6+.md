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