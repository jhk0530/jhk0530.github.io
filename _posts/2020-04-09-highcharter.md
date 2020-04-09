---
titie : highcharter
date : 2020-04-09
---

이 그림은 약 한달 전쯤 `CellEnrich`에 넣었던 그림이다. <br>

`ggplot2`로 그렸다.

솔직히 말하면 2018년 까지만 해도, 난 `ggplot`을 쓸 줄 몰랐다.<br>

`qplot`이랑 `ggplot`이랑 `ggplot2`랑 무슨 차이인지도 모르고, 그림을 그려야 할 일이 생기면 학부생처럼 그냥 `plot`으로 그렸다. <br>

`ggplot`을 잘 쓰려면 `grammar of graphics`를 이해해야 한다고 생각했고, <br>

이를 설명하는 pdf나 도서가 풀려 있었지만 양도 엄청 많아서 읽을 엄두가 안났기 때문에 <br>

스스로 여러 핑계를 대며, 정확히는 **쫄아서** 그냥 `plot`으로 조각칼로 소잡듯이 그림을 그렸다, 물론 결과는 당연히 허접했고.

첫 아들래미 `GScluster`가 나온 이후로 ,내가 만들었지만 퀄리티가 많이 아쉬웠기 때문에, <br>

앞으로는 가능하면 안해봤던 새로운 기술들을 하나씩은 써보자 라는 결심을 했다. <br>

그 결과들은 이러하다.

- `cytoscape.js`, `htmlwidget`, `shinyCyJS` ([`netGO`](https://github.com/jhk0530/netGO)).
- `learnR` ,`rmarkdown` ([`learnTidy`](https://github.com/jhk0530/learnTidy)).
- `colourpicker`, `shinymaterial` ([`shinyAssemble`](https://github.com/jhk0530/shinyAssemble))
- `semantic.dashboard`, `markdown` ([`shinyReadme`](https://github.com/jhk0530/shinyReadme) 
- `CRAN submission`, `argonDash`, `hexSticker` ([`polaroid`](https://github.com/jhk0530/polaroid))
- `pkgdown` ([`shinyCyJS`](https://github.com/jhk0530/shinyCyJS))

<img src='https://user-images.githubusercontent.com/6457691/76387865-259fab80-63ab-11ea-9741-46fdb87c0bec.png' width ='500'>

그리고 이 그림은 오늘 `CellEnrich`에 넣은 그림으로, <br>

<img src='https://user-images.githubusercontent.com/6457691/78905547-223a3580-7ab9-11ea-8851-e45520f7b62a.gif' width = '500'>

`highcharter`로 그렸다. <br>

저번에 `triage` 에서 배웠지만, 말 그대로 갔다 쓰는 정도만 했던 (★ 1개짜리) `highcharter` 라이브러리를, <br>
오늘 한 6시간 정도 맨땅에 들이받으면서 `ggplot`에서 교체했다.<br>

장단점은 물론 다 있겠지만, `highcharter`의 가장 큰 장점은, `export`나 `ux` 처럼 교수님이 딱 좋아할 만한 요소들이 있다는 것.

`cellenrich`에는 `sortable`, `highcharter`, 그리고 `waiter`정도로 충분 할 것 같다.


<img src='https://user-images.githubusercontent.com/6457691/78907512-c1f8c300-7abb-11ea-85f4-66e62b8d7a21.png' width ='500'>
