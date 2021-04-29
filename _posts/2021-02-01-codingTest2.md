---
title: "프로그래머스 코딩 테스트 #2 완주하지 못한 선수"
categories:
  - Programmers
tags:
  - Programmers
  - Javascript
  - Study
---

프로그래머스 사이트의 코딩테스트 연습 - [해시 - 완주하지 못한 선수](https://programmers.co.kr/learn/courses/30/lessons/42576)

### 코드 리뷰
처음 작성했던 코드는 <code>completion</code>을 <code>forEach</code>문으로 돌면서 <code>participant</code>에서 일치하는 값을 찾고 해당값의 인덱스를 가져와 <code>splice()</code>를 통해 일치하는 값을 배열에서 삭제해 나가는 방법이였습니다. 채점 결과 테스트 결과는 일치하지만 효율성 테스트를 통과하지 못해 코드 수정이 필요했습니다.

※ 테스트 결과는 일치하지만 효율성 테스트를 통과 하지 못했던 코드
```javascript
function solution(participant, completion) {
  let answer = '';
  completion.forEach(function(element){
    participant.splice(participant.indexOf(element), 1);
  });
  answer = participant[0];
  return answer;
}
solution(["leo", "kiki", "eden"], 	["eden", "kiki"]);
```

효율성 테스트 통과를 위해 코드 수정이 필요 하였고, 배열을 정렬 한 후 값들을 하나씩 비교해 나가 값이 같지 않으면 <code>answer</code>에 값을 담아 리턴하는 방법으로 수정하였습니다.
코드 수정 후 효율성 테스트까지 통과했습니다.

※ 코드 수정 후 효율성 테스트까지 통과한 코드
```javascript
function solution(participant, completion) {
  let answer = '';
  const ps = participant.sort();
  const cs = completion.sort();
  for(let i = 0; i < ps.length; i++){
    if(ps[i] !== cs[i]){
      answer = ps[i]
      break;
    }
  }
  return answer;
}
solution(["leo", "kiki", "eden"], 	["eden", "kiki"]);
```