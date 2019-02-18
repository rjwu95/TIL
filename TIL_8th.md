#### push notification 

```javascript
async registerForPushNotificationsAsync() {
    const { status: existingStatus } = await Permissions.getAsync(
      Permissions.NOTIFICATIONS
    );
    let finalStatus = existingStatus;
    if (existingStatus !== 'granted') {
      const { status } = await Permissions.askAsync(Permissions.NOTIFICATIONS);
      finalStatus = status;
    }

    if (finalStatus !== 'granted') {
      return;
    }

    let token = await Notifications.getExpoPushTokenAsync();
    return fetch('https://exp.host/--/api/v2/push/send', {
      body: JSON.stringify({
        to: token,
        title: this.state.title,
        body: this.state.body,
        data: { message: `${this.state.title} - ${this.state.body}` }
      }),
      headers: {
        'Content-Type': 'application/json',
        Accept: 'application/json'
      },
      method: 'POST'
    });
  }
```

위와 같은 형식으로 notification에 관한 token을 만들고 위의 로직안에서 push까지 처리를 하여 App의 상태가 background 일때만 push를 해주면 원하는 결과를 얻을 수가 있다. 

위의 과정을 시험해볼 때에는 특정 url에 post요청을 보내기 때문에 postman으로 요청을 보내서 요청이 제대로 처리되는지 확인을 하면 된다.

처음에 고민했던 부분이 fetch를 보내는 부분을 밖으로 따로 빼서 함수를 두개로 나눠서 실행을 하려했는데 그럴 경우 token을 어떤 공간에 담아놓고 바로 쓰는게 비동기로 처리되기 때문에 어려우니 fetch함수를 함수안에 같이 넣는 것이 중요하다.



### react native warning

Unrecognized WebSocket connection option(s) `agent`, `perMessageDeflate`, `pfx`, `key`, `passphrase`, `cert`, `ca`, `ciphers`, `rejectUnauthorized`. Did you mean to put these under `headers`? 와 같이 react native를 websocket에 연결하면 특정오류가 뜨는데 검색결과 따로 warning을 해결하진 않고 밑에 코드처럼 무시하는 방법을 선택했다. 확실한 방법이진 않지만 현재로선 최선인 것 같다.

```javascript
import { YellowBox } from 'react-native';

YellowBox.ignoreWarnings([
  'Unrecognized WebSocket connection option(s) `agent`, `perMessageDeflate`, `pfx`, `key`, `passphrase`, `cert`, `ca`, `ciphers`, `rejectUnauthorized`. Did you mean to put these under `headers`?'
]);
```



### AppState

```javascript
import { AppState } from 'react-native';  
componentDidMount() {
    AppState.addEventListener('change', this._handleAppStateChange);
  }

  componentWillUnmount() {
    AppState.removeEventListener('change', this._handleAppStateChange);
  }

  _handleAppStateChange(appState = 'active') {
    AsyncStorage.setItem('appState', appState);
  }
```

위와 같이 componentDidMount와  componentWillMount로 AppState의 상태를 확인할 수 있다.