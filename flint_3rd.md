#### Server route생성하기 (시작)

기본적으로 route를 생성하는 방법은 아래 코드와 같다.

아래의 폴더구조는 다음과 같다.

```javascript
// 폴더구조
app.js
src
	lib
    models
    route
    	api
        	challenges
        oauth
    service
```



```javascript
// app.js
const express = require('express');

const app = express();
const port = process.env.PORT || 3000;

app.use('/api', require('./src/route/api'));
app.use('/oauth', require('./src/route/oauth'));

app.listen(port, () => {
    console.log(`The server is listening on port ${port}`)
});

// api/index.js
const route = require('express').Router();

route.use('/challenges', require('./challenges'));

module.exports = route;

// api/challengs/index.js
const route = require('express').Router();
const dashboardController = require('./controller')

route.get('/getInProgressChallenges/:userId', controller.getInProgressChallenges);

//api/challenges/controller.js
const Sequelize = require('sequelize');
const { Challenges } = require('../../../models');

const { Op } = Sequelize;

// GET /api/challenges/getInProgressChallenges/:userId/ 요청 route
exports.getInProgressChallenges = async (req, res) => {
  const { userId } = req.params;
  const challenges = await Challenges.findAll({ where: { userId, state: {[Op.ne]:'inProgress'} } });
  res.status(200).send({ challenges });
}; 
```

위에서 사용한 것중에 익숙하지 않은 것이 Op라는 키워드 인데 이 키워드는 조건에 따라 db를 탐색하는 것을 도와주는 키워드이다. 사용법은 다음과 같다.

```javascript
const Op = Sequelize.Op

[Op.and]: {a: 5}           // AND (a = 5)
[Op.or]: [{a: 5}, {a: 6}]  // (a = 5 OR a = 6)
[Op.gt]: 6,                // > 6
[Op.gte]: 6,               // >= 6
[Op.lt]: 10,               // < 10
[Op.lte]: 10,              // <= 10
[Op.ne]: 20,               // != 20
[Op.eq]: 3,                // = 3
[Op.not]: true,            // IS NOT TRUE
```

더 자세한 내용은 http://docs.sequelizejs.com/manual/tutorial/querying.html에 있다.

