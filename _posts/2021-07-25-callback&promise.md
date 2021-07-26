---
title: "[모각코]프로젝트 외청 진행 사항: callback >> promise"
tags:
  - MongoDB
  - Mongoose
  - ExpressJS
  - Project_oc
projects:
  - oechul
excerpt: "Callback >> Promise

이전까지 callback 방식으로 함수를 작성했는데 그렇게 했을 때 return 되는 값이 원하는 값과 차이가 커서 전부 promise 방식으로 바꿔 진행하게 되었다.



callback 방식으로 원래 router.js에 직접적으로 함수를 작성했었다면, 지금은 promise 방식으로 함수를 controller.js에 작성하고 export하여 router.js에서 import하여 기능을 구현한다."
---

# Callback >> Promise

이전까지 callback 방식으로 함수를 작성했는데 그렇게 했을 때 return 되는 값이 원하는 값과 차이가 커서 전부 promise 방식으로 바꿔 진행하게 되었다.



callback 방식으로 원래 router.js에 직접적으로 함수를 작성했었다면, 지금은 promise 방식으로 함수를 controller.js에 작성하고 export하여 router.js에서 import하여 기능을 구현한다.



#### router.js

```javascript
import express from 'express';
import { response } from 'express';
import { NotExtended } from 'http-errors';
import {home, getinfo, getinfo_process, Delete, delete_process, update, update_process, edit_process, watch} from '../controllers/infoController'
import {matching} from '../controllers/matchingFunction'
var indexRouter = express.Router();

indexRouter.get('/',home)

indexRouter.get('/getinfo', getinfo)

indexRouter.post('/getinfo_process', getinfo_process)

indexRouter.get('/delete', Delete)

indexRouter.post('/delete_process', delete_process)

indexRouter.get('/update', update)

indexRouter.post('/update_process', update_process)

indexRouter.post('/edit_process', edit_process)

indexRouter.get('/matching', matching)

indexRouter.get('/info/:id', watch)

export default indexRouter
```



#### controller.js

```javascript
import StudentInfo from '../models/Student_Info'

export const home = async (request, response) => {
    const infos = await StudentInfo.find({})
    // StudentInfo.find({}, (error, infos) => {
    //   // console.log("errors", error)
    //   // console.log("infos", infos)
    //   response.render('home', {infos})
    // })
    return response.render('home', {infos})
}

export const getinfo = async (request, response) => {
    return response.render("getinfo")
}

export const getinfo_process = async (request, response) => {
    const {STnumber, Name, Field, instagramID, kakaoID, KoM, OwnMBTI, WantedMBTI, Major, MilitaryService, Smoker, Fre_Drinking, Bodyform, Hobby, SameMajor, O_MilitaryService, O_Smoker, O_Fre_Drinking} = request.body
    const info = new StudentInfo({
      _id: Number(STnumber),
      STnumber: STnumber,
      Name: Name,
      Field: Field.split(",").map((word) => `${word}`),
      instagramID: instagramID,
      kakaoID: kakaoID,
      KoM: KoM,
      OwnMBTI: OwnMBTI,
      WantedMBTI: WantedMBTI.split(",").map((word) => `${word}`),
      Major: Major,
      MilitaryService: MilitaryService,
      Smoker: Smoker,
      Fre_Drinking: Fre_Drinking,
      Bodyform: Bodyform,
      Hobby: Hobby,
      SameMajor: SameMajor,
      O_MilitaryService: O_MilitaryService,
      O_Smoker: O_Smoker,
      O_Fre_Drinking: O_Fre_Drinking,
    })
    await info.save()
    return response.redirect(`/`)
}

export const Delete = async (request, response) => {
    return response.render("delete")
}

export const delete_process = async (request, response) => {
    const {STnumber} = request.body
    await StudentInfo.deleteOne({STnumber: STnumber}, (err) => {
        if(err) console.log(err)
    })
    return response.redirect(`/`)
}

export const update = async (request, response) => {
    return response.render("update")
}

export const update_process = async (request, response) => {
    const {STnumber} = request.body
    await StudentInfo.findById(Number(STnumber), (error, info) => {
      if(!info) return response.render("404")
      return response.render('edit', {info})
    })
}

export const edit_process = async (request, response) => {
    const {STnumber, Name, Field, instagramID, kakaoID, KoM, OwnMBTI, WantedMBTI, Major, MilitaryService, Smoker, Fre_Drinking, Bodyform, Hobby, SameMajor, O_MilitaryService, O_Smoker, O_Fre_Drinking} = request.body
    await StudentInfo.findById(Number(STnumber), (error, info) => {
      info._id = Number(STnumber),
      info.STnumber = STnumber,
      info.Name = Name,
      info.Field = Field.split(",").map((word) => `${word}`),
      info.instagramID = instagramID,
      info.kakaoID = kakaoID,
      info.KoM = KoM,
      info.OwnMBTI = OwnMBTI,
      info.WantedMBTI = WantedMBTI.split(",").map((word) => `${word}`),
      info.Major = Major,
      info.MilitaryService = MilitaryService,
      info.Smoker = Smoker,
      info.Fre_Drinking = Fre_Drinking,
      info.Bodyform = Bodyform,
      info.Hobby = Hobby,
      info.SameMajor = SameMajor,
      info.O_MilitaryService = O_MilitaryService,
      info.O_Smoker = O_Smoker,
      info.O_Fre_Drinking = O_Fre_Drinking
  
      info.save()
    })
     return response.redirect(`/`)
}

export const watch = async (request, response) => {
    const STnumber = request.url.slice(6)
    await StudentInfo.findById(Number(STnumber), (error, info) => {
      return response.render("detail", {info})
    })
}
```



또한 promise 방식을 사용하게 되면 async와 await을 사용할 수 있어 코드를 조금 더 쉽게 원하는 방향으로 작성할 수 있게 된다.



이제 이와 같은 방식으로 자동 매칭 알고리즘을 이용한 matching function을 작성할 것이다.