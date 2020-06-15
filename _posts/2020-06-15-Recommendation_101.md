---
title : Recommendation 101
date : 2020-06-15
---

Recommendation, 혹은 추천 시스템은 Data science의 잘 알려진 여러 내용 중 하나이다.

Netfilx나 Amazon 등의 IT 공룡들이 이 Recommendation을 잘 이용해먹은 것은 너무나 유명하다. 

이에 대해서 내용을 대충은 아는데, 다시 공부 할 겸 여러 Article들을 정리함.

# Introduction

<img src='https://user-images.githubusercontent.com/6457691/84632068-57378e00-af29-11ea-99e6-8125f2f13d12.jpg' width = '600'>

Collective Intelligence, 혹은 집단 지성은, 

**내가 특정 컨텐츠를 선호하는지**를 예측하는, 이미 우리가 충분히 겪고 있는 기술을 뜻하는 표현이다. 

그 예시로는, 

- 내가 알 것 같은 사람을 추천하여, 사회적 관계를 넓히거나, (Facebook, Linkedin, 트인낭)

- 특정 제품을 구매했을때 같이 구입할 만한 물품을 추천해주거나, (Amazon)

- 내 취향에 따라서 노래 혹은 영화를 추천해주는 것이 있다 (Spotify, Netflix)

# Basics 

Recommender system, 추천 시스템은 크게 2가지 방식으로 나누어진다.

제품 X를 구매한 고객에 대하여 

1. X의 **어떤 특징이** 고객이 구매하게 만들었는지를 분석 (Content based, CB라 줄여쓰겠음)
2. X를 구매한 **다른 사람들은 어떤 제품을** 같이 구매하였는지를 분석 (Collaborative filtering, CF라 줄여쓰겠음)

예를 들어,

<img src='https://user-images.githubusercontent.com/6457691/84633041-ca8dcf80-af2a-11ea-9c6b-ef539f5ee10a.jpg' width = '600'>

내가 **The Mission** 이라는 영화를 보고 높은 평점을 주었다면 ( X = 'Mission' )

CB는 다음과 같이 영화의 Feature를 모델링 할 것이다.

```R
list(
  actors = c("Robert De Niro", "Jeremy Irons"), 
  director = c("Roland Joffé"), 
  topics = c("18th century", "Spanish colonization", "Christian Evangelization")
)
```

한편, CF는 

- [imdb](http://imdb.com) 같은 다른 사용자 평가를 모아둔 DB를 확인하여
- **The Mission**에 높은 평을 준 사용자 데이터를 바탕으로,
- 다른 높은 평가를 받을 만한 영화를 추천 할 것 이다. (추천되는 영화는 생략함.)

이 Article에서는, CF를 구현하는 방법을 소개한다. 

CF의 한가지 큰 issue는 **cold start problem**이라 하는 것으로. 
평가 데이터가 어느 정도 쌓여 있지 않은 초창기 부분일때, 퍼포먼스가 떨어지는 issue이다.

이를 위해서 많은 방식들이 2가지 방법을 공통으로 사용하는 hybrid recommender system을 사용한다고 하는데.
2가지가 뭔지 원글에선 안써둠. CB랑 CF겠지 뭐.

# CF의 목적

1. 아직 사용자가 평가하지 않은, 물건의 평점을 예측함.
2. Top N recommend item 이라는 **추천리스트**를 만듬.

# CF의 평가 방식

예측의 퍼포먼스는 보통, 실제값과 예측값의 차이, 분산을 바탕으로 측정된다.

예측을 실제에 더욱 가깝게 할 수록 그 차이는 적을테고 이는 좋은 퍼포먼스를 가진다고 볼 수 있기 때문이다.

제일 기본 방식은 Mean Average Error (MAE) 혹은 여기에 루트를 씌우느 Root Mean Square Error (RMSE)이다.

<img src='https://user-images.githubusercontent.com/6457691/84639871-ec3f8480-af33-11ea-9e28-5895f795f2f1.png' width = '500'>

- K = dataset
- P = predicted
- R = real

*원글에서는 RMAE라고 했지만, MSE나 MAE나 단어 선택의 차이지 크게 다르진 않다고 생각함.*

# 구현

Conversion rate optimization은 CRO 혹은 라고 불리며, 전환율 최적화라고 도 불리며 목적은 방문자를 구매자로 전환하는 것이라고 함.
* 그럴싸 하게 써놨는데 별 소리 아님, 얼마나 더 많이 팔까를 수치화 했다고 생각 하면 됨

## 1. 데이터 수집

- 어느 유저가, 
- 어떤 아이템을
- 언제 
- 어떤 방식으로 
- 얼마나 오랫동안 
이런 식의 기초 정보를 담고 있는 데이터가 있다고 해보자.

<img src='https://user-images.githubusercontent.com/6457691/84640977-5efd2f80-af35-11ea-9964-4d947936f5dc.png' width = '600'> 

내가 대학원 생활 동안 분석 했던 유전체 데이터는 대부분 Feature-Sample matrix 형태로 아래 그림처럼 분석하기 예쁘지만, 

<img src='https://user-images.githubusercontent.com/6457691/84641381-e185ef00-af35-11ea-831e-4245e8dd9c8b.png' width = '500'>

실제 데이터는 저런 형태가 아닌 중구난방한 테이블이 더 많다.

그렇기 때문에 이를 만들기 위해 User-Item 간의 Relation을 잴 수 있는 간단한 통계치를 하나 소개한다.

<img src='https://user-images.githubusercontent.com/6457691/84642216-1b0b2a00-af37-11ea-9c9d-c82b1471d140.png' width = '500'>

특정 User-Item 의 추천도는, (위데이터 기준) User의 모든 접속 Session에 대하여 추천도를 더한 값이다.

그리고 특정 Session에 대한 추천도는, 최근에 접속 했을 수록 weight를 주고, 여러 측정 값들에 대하여 weighted sum을 준다는 소리이다. 

이 간단하고 상식적인걸 표현을 못해서 면접을 떨어지다니 얼마나 어처구니가 없었는지 (weight를 어떻게 주는지는 다른 문제이다)

## 2. 데이터 normalize

내가 알던 normalize랑 원문의 Normalize는 전혀 다르다. 

<img src = 'https://user-images.githubusercontent.com/6457691/84642678-c61be380-af37-11ea-8395-2e00f02e83ca.png' width = '500'>

내가 틀릴 일은 없으니, 원문이 틀렸음 이건 normalize가 아니라 전처리임

아래는 위 그림과 같은 일을 하는, `recommenderlab` 패키지의 `realRatingMatrix` 형태로 변환 하는 코드 

```R
library('recommenderlab')
score <- read.csv('collected_data.csv')
score.matrix <- as(score, 'realRatingMatrix')
```

## 3. CF 모델 생성

```R
CF.model <- Recommender(score.matrix[1:5000], method = 'UBCF')

CF.model <- Recommender(score[1:400], method = 'UBCF', 
  param = list(normalize = 'Z-score', method = 'Cosine', nn = 5, minRating = 1))
```

- 이 코드는, cosine 유사도를 이용하여 사용자간의 거리를 측정한다. 유사도에는 jaccard나 pearson 등 여러 방식이 있다고 함.

아니 그냥 overlap 이랑 cor-relation 다 유전체에서 했던거랑 다를게 하나도 없네 오히려 나는 network까지 넣어서 유사도 측정했지;

- UBCF는 User based CF
- IBCF는 Item based CF

## 4. 모델 사용 

위에서 만들어진 모델은 아래처럼 사용 할 수 있다.

```R
recommend.items <- predict(CF.model, score.matrix['USER',], n = 5)  # TOP 5 element

as(recommend.items, 'list')
recommend.itsms.top3 <- bestN(recommend.items, n = 3)
```

## 5. 검증


```R
e <- evaluationScheme(score.matrix[1:1000], method = 'split', train = 0.9, given = 15)

# Model 
CF.UBCF <- Recommender(getData(e,'train'), 'UBCF')
CF.IBCF <- Recommender(getData(e,'train'), 'IBCF')

# rediction
Pred.UBCF <- predict(CF.UBCF, getData(e, 'kwown'), type = 'ratings')
Pred.IBCF <- predict(CF.IBCF, getData(e, 'kwown'), type = 'ratings')

# Error
E.UBCF <- calcPredictAccurary(Pred.UBCF, getData(e,'unknown'))
E.IBCF <- calcPredictAccurary(Pred.IBCF, getData(e,'unknown'))

```

## 정리

- Recommendation 은 2가지 방법으로 나뉠수 있다.
- 그 중 CF는, DB로부터 유사도 계산을 통해, 평가 되지 않은 추천 항목 & TOP N List 등을 만들어 낼 수 있다.
- `recommenderlab` package는 R에서 Recommendation 을 모델링, 비교, 평가 할 수 있는 하나의 방법이다.

## 기타

위에서 소개한 CF는 DB에 매우 매우 퍼포먼스가 의존되어있다. 

<img src='https://user-images.githubusercontent.com/6457691/84651716-662c3980-af45-11ea-8ff5-46f43f3d2b3e.jpg' width = '500'>

- 너무 적어도 문제겠지만, 너무 많으면 O(n^2)로 유사도를 계산해야 하기 때문에 
- 이를 위해서 PCA같은 Dimension Reduction 을 사용하기도 함.

- **다수의 결정이 옳을 것이다라는 가정이 맞다 라는 보장이 없음**
- 즉, 의견에 bias가 생길 수도 있기 때문에 minority data에 weight 를 주는것도 고려해볼만함

# References

[Recommender Systems 101 - a step by step practical example in R](https://www.r-bloggers.com/recommender-systems-101-a-step-by-step-practical-example-in-r/)

[Building a Movie Recommendation System](https://rpubs.com/jeknov/movieRec)

