
<img src= 'https://user-images.githubusercontent.com/6457691/74031451-df92a900-49f4-11ea-82ee-a6bbfac90de4.png' width = '300'/>

----

[GSCluster](https://github.com/unistbig/gscluster)와 [netGO](https://github.com/unistbig/netgo)의 핵심 기술 중 하나는 아래처럼 interactive 한
graph / network를 shiny에 뿌려주는것이 아닐까 생각한다.

![I5](https://user-images.githubusercontent.com/6457691/74031375-af4b0a80-49f4-11ea-80f5-842b9d2911d0.gif)

이를 위해 원래는 javascript 라이브러리인 [cytoscape.js](http://js.cytoscape.org/)를 R에 긁어와서 shiny에 쓰고 싶었는데<br>

기존에 만들어둔 패키지는 정상적으로 사용 할 수 없었다.

1. [RCyjs](http://www.bioconductor.org/packages/release/bioc/html/RCyjs.html)는 shiny를 지원하지 않았고
2. [rcytoscapejs](https://github.com/cytoscape/cyjShiny)는 그나마 gscluster 첫 구현때 쓰던거였지만, 2.* 버전을 사용하는 아주 구식 ( 현재 3.12)
3. rcytoscapejs를 만들던 팀이 cyjshiny를 만들긴 했지만 이것도 정작 netGO를 쓸때는 미구현상태였다.

지금 다시 찾아보니 이 팀이 [r2cytoscape](https://github.com/cytoscape/r2cytoscape), [rcy3](https://github.com/cytoscape/RCy3)등 
이름을 열심히 바꿔가며 온갖 시도를 하는것 같아 보이지만 솔직히 아직도 미완성이라는건 매우 실망스럽다.

결국 꾸역꾸역 내가 만들었는데, [dean attali 선생님](https://deanattali.com/shiny/)의 명강의([timevis](https://deanattali.com/blog/htmlwidgets-tips/))를 보고 
 cytoscape.js를 긁어와 shinyCyJS를 완성. <br>
이후 netGO는 이 "엔진"을 기반으로 만들고, GScluster도 구닥다리 rcytoscapejs를 버리고 shinyCyJS로 갈아타는 작업에 성공했다. <br>

이에 대해서는 따로 쉬운 버전으로 [repo](https://github.com/jhk0530/shinyRoughJSbasic) 파두었으니 필요시 참조 

이후 netGO를 완성하고 시간이 조금 지난 이후에 shinyCyJS 작업을 조금 더 하고 cytoscape.js에 우리 이런거 만들었어요~ 하고 올려서 
오피셜하게 unistbig 이름을 박아버렸다. 교수님은 전혀 신경 안쓰는게 안타깝지만 

기억속에서 좋은 추억으로 잊혀가는가 싶었는데 최근, 모르는 사람한테서 문의가 왔다. 
shinycyjs를 쓰고 싶은데 진행 내용이 궁금하다. 대충 이런 내용으로

그래서.. 

![image](https://user-images.githubusercontent.com/6457691/74031052-e40a9200-49f3-11ea-8d23-b4042d5fbba7.png)

- 코드 한 파일에 있던것 쪼개고
- `roxygen2` 써서 매뉴얼 다 달고
- `travis-ci` 써서 테스트 하고
- `testthat`써서 테스트 코드 기본적인것만 몇개 작성해서 

결국 해냈다. 어 예 

![image](https://user-images.githubusercontent.com/6457691/74029803-da335f80-49f0-11ea-97a2-c15275c43ae4.png)

보통 한번에 통과하는 경우는 없고, 특히 첫 submission에 경우는 수정할게 더 많다고 하는데.

인생의 좋은 경험이라고 생각하고 하여튼 열심히 해야지

- 참고로 낸지 30분만에 에러 났다고 메일 왔다. 일단 퇴근하고 내일해야지
