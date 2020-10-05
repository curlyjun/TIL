# Mobx

mobx 가 오늘 6버전으로 업데이트 되면서 더이상 데코레이터를 사용할 필요가 없게되었다.



```tsx
import { makeAutoObservable } from 'mobx'
import React from 'react'
import { observer } from 'mobx-react-lite'

class MyStore {
  count:number = 0
  
  constructor(){
    makeAutoObservable(this)	// 이렇게 this를 인자로 넣어주면 끝난다.
  }
  
  increase() {
    this.number++
  }
}

const store = React.createContext(new MyStore());

const App = observer(() =>{
	const counter = React.useContext(store);
  
 	return (
    <div>
    	<div>{counter.count}</div>
      <button onClick={() => counter.increase()}>click</button>
    </div>
  )
})


```



## 비동기 처리

비동기 액션을 그동안 잘 못 써왔다.

액션에 async await 만 잘 작동했기에 그렇게 사용해도 되는줄 알았는데

첫번째 비동기 함수 실행 후 , 즉 스케쥴된 함수에서는 액션이 적용되지 않았다.

첫 비동기 실행 이후에 변경되는 스테이트 값들은 runInAction에 넣어서 변경해줘야 action 함수 내에서 변경된다.

```typescript
import { observable, runInAction, action } from 'mobx'

class Store {
  @observable count: number = 0;
  
  @action
  increase = async () {
  	await Promise.resolve();
    runInAction(() => {
      this.count ++;
    })
  }
}
```

이보다 직관적인 flow를 사용해서 작성하는게 좋아보인다. 



```typescript
import { observable, flow, action } from 'mobx'

class Store {
  @observable count: number = 0;
  
  @action
  increase = flow(function* () {
  	yield Promise.resolve();
    this.count ++;
  }
}
```

