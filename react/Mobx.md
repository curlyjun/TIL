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

