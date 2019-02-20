// 코드리뷰를 하면서 받은 커멘트
**config file 항상 확인하기
**react나 react native에선 한눈에 컴포넌트와 뷰들을 볼 수 있어야 하기 때문에 onPress안의 함수와 style을 모두 빼는 것이 좋다.
**react를 쓰던 native를 쓰던 lifecycle을 파악하는게 매우 중요하다.

**react같은 경우는 함수 여러개를 export할 때 다음과 같이 한개 한개마다 앞에 export를 붙여준다.

```javascript
// export.js
export function call(){
    
}
export function get(){
    
}

//import.js
import {call, get} from './export.js'
```



##### *react에서 render부분은 진짜 render시킬 부분만 넣는 곳이고, 나머지 함수정의나 로직은 다 그 위에서 진행된다! 매우 중요. 그 이유는 render는 lifecycle에 따라 실행되는데 그 때마다 원하지 않는 동작들이 일어나면 안되기 때문.

