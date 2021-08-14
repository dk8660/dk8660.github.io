---
title: "[모각코]프로젝트 외출 진행 사항: 자동 매칭 알고리즘 #5"
tags:
  - MongoDB
  - Mongoose
  - ExpressJS
  - Project_oc
excerpt: "자동 매칭 알고리즘 #5

현재 1 : 1로 매칭하여 정보를 DB에 저장하고 매칭이 불가능 한 사람을 제외하고 모든 매칭이 끝났을 때, 매칭을 올바르게 종료하여 에러가 나지도 않게 하였다."
---

# 자동 매칭 알고리즘 #5

현재 1 : 1로 매칭하여 정보를 DB에 저장하고 매칭이 불가능 한 사람을 제외하고 모든 매칭이 끝났을 때, 매칭을 올바르게 종료하여 에러가 나지도 않게 하였다.

```javascript
export const select = async (request, response) => {
    try {
        while(1) {
            const infos = await StudentInfo.find({isMatched: false}) // 매칭이 되지 않은 모든 사람들 불러오기
        
            infos.sort(function(a, b) { // 매칭 후보의 수를 비교하여 오름차순으로 정렬
                return a.MatchedPeople.length - b.MatchedPeople.length
            })
            
            let index = 0;
            for(let i = 0; i < infos.length; i++) { 
                if(infos[i].MatchedPeople.length > 0) { // 매칭 가능한 후보가 있으면 매칭 시작
                    index = i
                    break
                }
                if(i == infos.length - 1) {return response.render('select')} // 매칭이 성사될 수 없는 사람을 제외하고 모든 사람의 매칭이 끝나면 종료
            }

            let max = 0
            for(let j = 1; j < infos[index].MatchedPeople.length; j++) { // 점수가 가장 높은 후보를 선택
                if(infos[index].MatchedScore[max] < infos[index].MatchedScore[j]) max = j;
            }
            let a = infos[index].STnumber
            let b = infos[index].MatchedPeople[max]
            await StudentInfo.updateOne({STnumber: a}, {MatchedPeople: [null], MatchedScore: [null], isMatched: true, selectedPerson: b}) // 선택된 후보를 DB에 저장
            await StudentInfo.updateOne({STnumber: b}, {MatchedPeople: [null], MatchedScore: [null], isMatched: true, selectedPerson: a})
            
            const updateInfos = await StudentInfo.find({isMatched: false})
            for(let i = 1;  i< updateInfos.length; i++) { // 매칭이 완료된 사람들은 다른 사람들의 후보 리스트에서 제외
                index = 0
                for(let j = 0; j < updateInfos[i].MatchedPeople.length; j++) {
                    if(updateInfos[i].MatchedPeople[index] == a || infos[i].MatchedPeople[index] == b) {
                        updateInfos[i].MatchedPeople.splice(index, 1)
                        updateInfos[i].MatchedScore.splice(index, 1)
                        index--
                    }
                    index++
                }
                await StudentInfo.updateOne({STnumber: updateInfos[i].STnumber}, {MatchedPeople: updateInfos[i].MatchedPeople, MatchedScore: updateInfos[i].MatchedScore})
            }
        }

        return response.render('select')
    } catch(error) {
        console.log(error)
    }
}
```

아직 지금은 3명의 사람의 데이터로만 테스트를 진행해보았기 때문에 사실 인원이 많아졌을 경우 코드가 올바르게 작동하지 않을 수도 있다. 더 많은 데이터들로 테스트를 해보아야 할 것 같다.

자동 매칭 알고리즘이 완성되면, 프론트 엔드와 백 엔드를 잇는 작업을 할 것이다.
