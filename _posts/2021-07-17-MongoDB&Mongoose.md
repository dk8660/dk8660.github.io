---
title: "[모각코]ExpressJS #6 보안 & Generator"
tags:
  - ExpressJS
  - Summary
excerpt: "보안

보안성 향상을 위해서 Express는 다음 사항들을 권고한다.

- 더 이상 사용되지 않거나 취약성이 있는 버전의 Express 사용 중지
- TLS 사용
- Helmet 사용
- 쿠키를 안전하게 사용
- 인증 체계에 대한 브루스 포트 공격 방지
- 종속 항목이 안전한지 확인
- 그 외의 알려져 있는 취약점 회피
- 추가적인 고려사항

Helmet

여기서 Helmet은 보안과 관련해서 자주 발생하는 이슈들을 자동적으로 해결해주는 모듈이다.

npm install --save helmet으로 설치할 수 있다."
---


# MongoDB

**MongoDB**는 **NoSQL**로 분류되는 Document 지향 데이터베이스 시스템이다. NoSQL은 "Not only SQL"의 약자로 SQL만을 사용하지 않는 데이터베이스 관리 시스템을 지칭하는 단어이다.



따라서 MySQL의 테이블과 같은 스키마가 고정된 구조 대신 JSON 형태의 동적 스키마형 문서를 사용한다.



MongoDB는 공식 사이트 또는 chocolatey에서 직접 설치하면 된다.



설치가 정상적으로 이루어지면 cmd 창에서 `mongo`를 입력했을 때, MongoDB shell이 열린다.

MongoDB로 접속이 되면 `connecting to: mongodb://127.0.0.1:27017/~~`이런 부분이 보이는데 연결된 mongodb의 주소이다.



# Mongoose

Mongoose는 MongoDB를 쉽게 다룰 수 있게 해주는 객체 모델링 도구이다.

데이터베이스 연결, 스키마 정의, 스키마에서 모델로의 전환, 모델을 이용하여 실제 데이터를 다루는 것까지 가능하다.

promise와 callback 두 가지의 방식으로 사용이 가능하다.



프로젝트 작업 중인 디렉토리에서 `npm install mongoose`를 통해 설치할 수 있다.



## DB 연결

Mongoose를 import 해와서 MongoDB와 연결하는 방법이다.

```javascript
var mongoose = require('./node_modules/mongoose')

mongoose.connect('mongodb://127.0.0.1:27017/oc', {useNewUrlParser: true, useUnifiedTopology: true})

const db = mongoose.connection

const handleOpen = () => console.log("Connected to DB")
const handleError = (error) => console.log("DB Error", error)
db.on("error", handleError)
db.once("open", handleOpen)
```

`mongodb://127.0.0.1:27017/oc` 이 부분은 localhost의 oc라는 데이터베이스로의 접근을 의미한다. 만약 oc라는 데이터베이스가 없다면 생성된다.

console.log()를 이용하여 정상적으로 DB에 연결되었는지를 확인할 수 있다.



## Schema & Model

MongoDB에는 정적인 schema가 없지만 Mongoose는 schema를 정의해 줄 수 있다.

schema는 문서 내부가 어떤 식으로 구성되어 있는지를 정의하는 객체이다.



model은 schema를 사용하여 만든 데이터베이스에서 실제 작어블 처리할 수 있는 함수들을 가진 객체이다.



```javascript
var mongoose = require('../node_modules/mongoose')

const Student_InfoSchema = new mongoose.Schema({
    STnumber : {type: Number, required: true},
    Name : {type: String, required: true},
    Major : {type: String, required: true},
})

const StInfo_Model = mongoose.model("StudentInfo", Student_InfoSchema)
module.exports = StInfo_Model
```



위와 같은 방법으로 학생들의 학번, 이름, 전공을 담은 schema와 model을 만들어 사용할 수 있다.
