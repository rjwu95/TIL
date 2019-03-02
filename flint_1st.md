#### Animated view 사용법

이번 스프린트를 진행하면서 다음과 같은 화면의 Modal과 비슷한(?) 화면을 만들어야했다. 

![Screenshot_20190227-113013_Instagram](/home/yugeon/Downloads/Screenshot_20190227-113013_Instagram.jpg)

가장 직관적으로 와닿았던 방법은 Modal을 특정 flag에 의해 화면에 나오게 하는 방법이었는데, 그러면 문제점이 react-native-modal은 화면을 꽉 채우지 못해서 위와 같은 모습이 나오질 않고, 일반 modal로 한다면 화면을 다 채워서 헤더를 맞추기가 어려운 것이 문제점이었다. 그래서 찾아본 방법이 일반 View를 쓰고 animation을 줘서 header에서 view가 나오는 것처럼 보이는 형태를 사용했다. 이에 대한 예시코드는 다음과 같다.

```javascript
//index.js
toggleSubView = () => {
    const { bounceValue } = this.state;
    let toValue = 0;
    if (isHidden) {
      toValue = 200;
    }
    Animated.spring(bounceValue, {
      toValue,
      velocity: 3,
      tension: 2,
      friction: 8,
    }).start();
    // console.log(isHidden);
    isHidden = !isHidden;
  };

//Component.js
<Animated.View
style={[styles.subView, { transform: [{ translateY: this.props.bounceValue }], zIndex: 300 }]}
>
    <SubView toggleSubView={toggleSubView} />
</Animated.View>
```

위에선 index에서 Component로 bonceValue값을 주고, zIndex는 animation View가 맨앞으로 나와야 하기 때문에, 300이라는 값을 부여했다.