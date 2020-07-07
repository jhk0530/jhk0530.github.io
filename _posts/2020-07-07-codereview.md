---
title : code review with R
date : 2020-07-07
---

# Code review 

<img src = 'https://mathewanalytics.com/wp-content/uploads/2020/07/reviewcode.152f235.921f924.jpeg' width = '500'>

Code review에 관한 좋은 소개글이 있어서 작성함.

## Code review란? 

Code review란, 소프트웨어 개발의 중요한 한 단계로 말 그대로 `코드를 리뷰`하는 프로세스이다.

엥 이게 무슨 소리야, 코드를 다시 보는게 다야? 라고 할 수 있겠지만, 

일반적인 코드 검토와는 약간 다르게, 자기가 보는게 아닌 Peer review 처럼, 다른 사람들이 코드를 리뷰하는 것이 다른 점이다.

Code review의 첫번째 목적은 당연히 

- 추가로 작성된 코드가, 해야 할 일을 문제 없이, 잘 해내는지를 확실히하기 위함이다.

그러나 사실, 이 검증의 목적 외에도 다른 이유들이 있는데,

소규모의 팀 단위에서 이뤄지는 코드 리뷰는, 

서로의 코드를 보면서 `잡기술`을 배울 수 있게 한다. 그 예로는

- function, clousre, classes등의 fancy 한 사용법.
- code style 의 단일화 (사실 정말 정말 중요하다 이 부분은)
- 코딩 경험이 적은 주니어급이 시니어 혹은 다른 주니어로 부터 배울 수 있다.
- 새로운 기술 (패키지나 라이브러리)과 그 적용을 배울 수 있다. 

같은 것들이 있다.

## 문제

이렇게 좋은 Code review지만, 단점이 없지는 않다. 

모든 코드를 다 review하기엔, 시힘귀. 즉 시간도 없고 힘들고, 귀찮기 때문.

그렇기 때문에 원문의 저자는 모든 코드가 아닌, 중요성에 따라서 필요한 코드만 리뷰를 해야한다 라고 말을 함.

<img src='https://mathewanalytics.com/wp-content/uploads/2020/07/Screen-Shot-2020-07-05-at-5.41.33-PM.png' width = '500'>

아래와 같이 5개의 코드가 있다면, 어떤 것을 봐야 할까.

- 명백한 코드가 아닌, 복잡한 로직이 들어가 있는 코드. `basic_eda.R` 같은 코드는 상대적으로 명백하므로 제외.
- 적게 실행 되거나, 퍼포먼스가 많이 중요하지 않은 코드, `helper_functions.R`같은 코드는, 다른 곳에서 쓰이기 위한, 필수적이지 않은 **코드를 위한 코드** 이므로 생략.
- 핵심이 되는 코드 `dataset_builder.R`이나 `execute_analysis.R` 처럼, `killer content`로 간주 되는 코드가 Code review의 대상이 된다.

## 시간

다시, Code review가 좋은건 맞지만 물리적인 이유로 항상 할 수는 없다. 

code를 만들어야 code review를 하는데 review만 하면, code를 못 만들기 때문

저자는 자기 팀 기준으로 주 1회 2시간 정도씩 한다고 함.

아무래도 팀바팀, 리더 맘대로 정해야겠지.

## 방법

구체적으로 code review를 하는 방법.

- 원작자가 쓴 Code를 뿌림. (2인 이상)
- 이 때 code는 500줄 이하로 권장 ( 너무 길면, 그건 code review 보다는, refactoring을 해야함)
- reviewer는 제안 혹은 추천을 작성함, 단 이때 받은 코드에 주석과, 대문자로 이뤄진 문장을 달기를 권장 ( 코드와 구분 되며, 읽기 쉽게 )

여기서 제안 혹은 추천은 다음 요소들을 우선으로 생각한다.

- code가 목적에 맞게 잘 작동 하는지
- 로직에 오류가 있는지
- code style에 잘 맞추어 작성 되었는지
- 계산 방식 등에서 개선 가능한 부분이 있는지 (최적화)
- 모든 edge case ( 에러가 날만한 혹은 특수한 상황 ) 를 다 다루고 있는지 
- documentation이나 comments가 적절하게 작성 되었는지

## 태도

사실 code review를 검색해보면 나오는 많은 글이 있지만, 

꽤 많은 글에서 공통적으로 언급 하는 이슈는 Attitude이다.

즉 생산적인 code review가 아닌, 헐뜯기 위한 code review로 변절 되어버려서 

process 자체에 스트레스를 받는 개발자가 많다는 것. 

**be nice** 하게 하라는데 이는 review 프로세스 자체 뿐만 아니라, reviewer에게도 nice한 rewards가 필요하다는 내용이 아닐까...?


[Best Practices for Code Review: R Edition](https://mathewanalytics.com/best-practices-for-code-review-r-edition/?utm_source=feedburner&utm_medium=email&utm_campaign=Feed%3A+rweekly-live+%28R+Weekly+Live%29)

참고로 원문 링크는 R Edition이라고 달아놨지만, 실제로 R과는 거의 관계가 없다. 실망
