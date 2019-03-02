#### JSX에서 삼항연산자를 쓰지 않고 조건부 rendering 하는 방법

jsx를 사용하다보면 제일 많이 고민되는 부분이 함수를 쓰지 못하기 때문에 어떻게 조건에 따라서 rendering을 할지에 대한 고민이다. 나도 처음엔 삼항연산자를 주로 썼지만 검색과 eslint의 경고문으로 인해 좀 더 간단하게 쓰는 방법을 찾았다.

```javascript
A || B // 이와 같은 경우는 삼항연산자로는 A ? A : B로 쓸 수 있고, 코드의 중복이 없어졌다.
		// 뜻으로는 A가 true면 A를 return 아니면 B를 return 한다.
A && B // 이 같은 경우는 삼항 연산자로 대체할 수 없는데 A가 true인 경우에는 B를 return 하고 false인 경우는 아무것도 return 하지 않는다.
```



#### PropTypes 사용하는 방법

PropTypes는 간단히 말하면 props를 하위 component로 내려줄 때 하위 component에서 그 props의 type을 지정하여 맞지 않으면 어떤 신호를 보내주는 것이다. 일반적으로 쓰는 방법은 다음과 같다.

```javascript
import PropTypes from 'prop-types';

ChildComponent.propTypes = {
    sampleProps: PropTypes.string.isRequired // isRequired는 꼭 있어야 하는 값에 붙여준다.
}
// string에 해당하는 부분에는 func, number, bool 등이 들어갈 수 있다.
```



나는 eslint를 airbnb standard를 사용하고 있었기 때문에 사용할 수 없는 부분이 있었는데, object와 array는 위와 같은 형태로 지정해 줄 수 없다. 따라서 eslint를 해결할 수 있는 방법은 다음과 같다.

```javascript
sampleArray1: PropTypes.arrayOf(PropTypes.number) // number로 이루어진 array 
sampleArray2 : PropTypes.arrayOf(PropTypes.oneOfType([PropTypes.number, PropTypes.string])).isRequired // value가 number또는 string인 array
sampleObject: PropTypes.shape({
    id: PropTypes.number.isRequired,
    email: PropTypes.string.isRequired 
}) // id는 number이고 email은 string인 object
```

