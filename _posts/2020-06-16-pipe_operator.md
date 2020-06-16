---
title : Pipe operator
date : 2020-06-16
---

`%>%`. 혹은 Pipe operator.

<img src='https://user-images.githubusercontent.com/6457691/84745881-2bcba680-aff0-11ea-9f65-28477072af3a.png' width = '150'>

`R` 이 `Python`에 비해 몇 안되는 장점들 중 하나가 이것이지 않을까?

`Python`도 가능은 한데 **계산적으로 expensive하니까 잘 생각하고 쓰세요**. 라고 말하는 중

[Functional-pipes-in-python-like-from-rs-magrittr](https://stackoverflow.com/questions/28252585/functional-pipes-in-python-like-from-rs-magritrr/31037901)

오늘은 이 Pipe operator에 대해서 좋은 글을 읽었기에 이를 정리해 보고자 한다. 

# Purpose

Pipe operator의 역할은 `여러 단계의 function`을 **다시 표현** 하는 것이다. 

가령 `x`라는 데이터를 순차적으로 `f1`, `f2`, `f3`이라는 함수에 사용해야 한다면

```R
x <- f1(x)
x <- f2(x)
x <- f3(x)
```

이런 식으로 코드를 작성 할 수 있을것이다. 하지만 Pipe를 사용하면

```R
# one-line
x %>% f1() %>% f2() %>% f3()

# tidyverse style
x <- x %>%
  f1() %>%
  f2() %>%
  f3()  
```
다음과 같이, 하려는 내용을 조금 더 intuitive 하게 알 수 있다.

물론, Pipe가 만능은 아니다. 아래와 같은 상황에서는 Pipe 사용 보다는 코드 검토 부터 다시 하자.

- 함수 구성 단계가 10개 이상일때.
- 함수가 여러개의 `input` 과 `output`을 필요로 할 때

# Deep inside

그럼 이 Pipe의 작동 원리는 무엇일까

원래 Console에 `%>%`를 치면, 아래처럼 꽤 복잡한 내용이 나온다.

<img src='https://user-images.githubusercontent.com/6457691/84746464-f1aed480-aff0-11ea-8376-6fcb2b28caed.png' width = '500'> 

못해도 몇시간은 들이 부어야 이해 할 수 있는 내용이지만, 토마스 선생님께서 친절한 해석글을 써주셨다.

[How-does-the-pipe-operator-actually-work](https://thomasadventure.blog/posts/how-does-the-pipe-operator-actually-work/)

```R
`%>%` <- function(lhs, rhs) {
  lhs <- substitute(lhs)
  rhs <- substitute(rhs)
  building_blocks <- c(
    rhs[[1L]],
    lhs,
    as.list(rhs[-1L])
  )
  call <- as.call(building_blocks)
  eval(call, envir = parent.frame())
}
```
위 코드를 기준으로 저걸 다시 재해석, 요약하자면, 

- Pipe는 좌항(`lhs`)과 우항(`rhs`)를 입력 받는 연산자이다. 단, 좌항은 목적 obejct, 우항은 목적 function.
- `substitute`는 그 양 입력값의 원본을 **읽어내는** 함수로. `substitute(1+2)`는 `1+2`가 된다 `3`이 아니라.
- 그 후 `building blocks` 는 우항과 좌항을 묶은 `object`인데. 
- 좌항은 그대로 쓰이고, 함수가 들어갈 우항은 `[[ ]]`를 통해 각각 함수의 이름과 (`[[1]]`) 함수의 parameter (`[[-1]]`)로 구분지어짐.
- 이 후 그 내용을 `as.call`과 `eval`을 통해 연산.

Full Code에는 `environment`와 `wrap`, `visible` 같은 다른 이슈들을 포함하는 내용도 같이 구현되어 있지만 핵심은 파악했다.

---

이제 ggplot2 의 `+` operator도 알아보자

```R
function (e1, e2) 
{
    if (missing(e2)) {
        abort("Cannot use `+.gg()` with a single argument. Did you accidentally put + on a new line?")
    }
    e2name <- deparse(substitute(e2))
    if (is.theme(e1)) 
        add_theme(e1, e2, e2name)
    else if (is.ggplot(e1)) 
        add_ggplot(e1, e2, e2name)
    else if (is.ggproto(e1)) {
        abort("Cannot add ggproto objects together. Did you forget to add this object to a ggplot object?")
    }
}
```

- 에러 핸들링
- 우항 `substitute` 후 `parse`
- 그 후 `eval`, `as.call` 대신 `add_ggplot` 호출.

이제 **base R에서 `+` 랑 `ggplot2`에서 `+`랑 하는 역할이 뭐고 어떻게 다른가요** 라고 물어보면 대답 할 수 있을 것 같다. 

# TMI

`dplyr`를 위시한 여러 `tidyverse` 에서 자주 쓰이는 이 pipe operator `%>%`는 사실 `magrittr`에서 먼저 쓰였다. 

magrittr 혹은 마그리트,가 뭐냐고? 이 **This is not a pipe** 그림의 주인이시다.

<img src='https://user-images.githubusercontent.com/6457691/84744488-50bf1a00-afee-11ea-9a58-6bce51ef6ada.jpg' width = '500'>  

사실 저 magrittr 패키지에는 pipe 말고도 다른 여러 operator가 있다고 한다. 

- `%<>%` : `pipe` + `assign`
- `%T>%` : `skip`
- `%$%` : `attribute`

각 예시에 대한 더 자세한 설명이 잘 정리된 [블로그](https://blog.naver.com/cnrrn214/221614040645)가 있으니 참고하면 좋을 것 같다.

블로그 주인은 국방의 의무를 지는 중인 학식맨이라는데 탐나는 인재인 것 같다.

---

`Rstudio` 기준, `Ctrl` + `Shift` + `M` 으로 `%>%`를 대체 하여 쓸 수 있다.  

---

# Reference 

[R4DS](https://r4ds.had.co.nz/pipes.html)

