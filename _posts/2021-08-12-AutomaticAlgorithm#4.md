---
title: "[모각코]프로젝트 외출 진행 사항: 자동 매칭 알고리즘 #4"
tags:
  - MongoDB
  - Mongoose
  - ExpressJS
  - Project_oc
excerpt: "자동 매칭 알고리즘 #4

저번에 매칭된 후보들을 DB에 점수와 함께 저장하는 것까지 완료하였다.

이제는 저장된 것들을 바탕으로 1 : 1 매칭을 성사시킬 것이다."
---

# 자동 매칭 알고리즘 #4

저번에 매칭된 후보들을 DB에 점수와 함께 저장하는 것까지 완료하였다.

이제는 저장된 것들을 바탕으로 1 : 1 매칭을 성사시킬 것이다.

1. 아직 매칭이 되지 않은 사람들을 DB에서 불러온다.
2. 매칭 후보의 수를 비교하여 적은 사람이 먼저 매칭이 될 수 있도록 한다.
3. 후보 중 점수가 가장 높은 사람과 매칭을 성사시킨다.

```javascript
export const select = async (request, response) => {
    try {
        let prev = null;
        while(1) {
            const infos = await StudentInfo.find({isMatched: false}) // 매칭이 되지 않은 모든 사람들 불러오기
            if(infos == prev) break;
        
            infos.sort(function(a, b) { // 매칭 후보의 수를 비교하여 오름차순으로 정렬
                return a.MatchedPeople.length - b.MatchedPeople.length
            })
    
            let max = 0
            for(let j = 1; j < infos[0].MatchedPeople.length; j++) { // 점수가 가장 높은 후보를 선택
                if(infos[0].MatchedScore[max] < infos[0].MatchedScore[j]) max = j;
            }
            let a = infos[0].STnumber
            let b = infos[0].MatchedPeople[max]
            await StudentInfo.updateOne({STnumber: a}, {MatchedPeople: [null], MatchedScore: [null], isMatched: true, selectedPerson: b}) // 선택된 후보를 DB에 저장
            await StudentInfo.updateOne({STnumber: b}, {MatchedPeople: [null], MatchedScore: [null], isMatched: true, selectedPerson: a})
            
            for(let i = 1;  i< infos.length; i++) { // 매칭이 완료된 사람들은 다른 사람들의 후보 리스트에서 제외
                let index = 0
                for(let j = 0; j < infos[i].MatchedPeople.length; j++) {
                    if(infos[i].MatchedPeople[index] == a || infos[i].MatchedPeople[index] == b) {
                        infos[i].MatchedPeople.splice(index, 1)
                        infos[i].MatchedScore.splice(index, 1)
                        index--
                    }
                    index++
                }
            }
            prev = infos
        }

        return response.render('select')
    } catch(error) {
        console.log(error)
    }
}
```

아직 완성되지 않아 오류가 있다.

함수를 중단 시키는 조건을 수정해야 할 것 같다.
