 `waiter` 라는 Shiny 패키지를 보고 말 그대로 감동을 받았다. 이유는 말 그대로 "Fancy" 하기 때문에.<br>
 해당 리포에서 어케 만들었나 보면서 있던 와중에 issue 항목에 **`cheatsheet`이 필요하다** 라는 내용을 확인 할 수 있었다.
 
 여기서 `cheatsheet`이란, `hexbin`과 더불어 내가 좋게 생각하는 R package의 관습 중 하나인데<br>
 [Rstudio](https://rstudio.com/resources/cheatsheets/) 에서 처음 만들었는지 어쩐지는 모르겠지만, <br>
 package의 주요 내용을 말그대로 `cheatsheet`으로 쓸 수 있게 <br>
 
 - 필요한 내용을 
 - 이쁘게
 - 잘 요약해서 
 
 몇장으로 참고 할 수 있게 요약 해놓은 매뉴얼 정도로 생각하면 될 것 같다.
 
 실제로 구현 된 내용은 더 있었지만, `waiter`는 크게 3가지 `waiter`, `waitress`, `hostess`로 구성되어 있었고.
 각 구성요소 마다 약간씩은 다르긴 했지만 사용 방법도 
 
 1. 포함
 2. 선언
 3. 제어
 4. 종료
 
 라는 공통의 단계를 가지는 비교적 깔끔한 구성을 가지고 있었기에 옳다구나 하고 만들었다.
 
 Rstudio 에서는 자기들이 `cheatsheet`을 만들기도 하지만, `cheatsheet`을 사용자들이 contribution으로 만들어 주길 official하게 권장하며, <br>
 template이나 예시 등을 공유 해두어 필요한 아이디어나 컨셉, 인사이트를 참조 할 수 있게 해두었다.
 
 그래서 ppt로 만들어진 template을 받아서, <br>
 `hexbin`으로 부터 사용할 색 테마를 정한 다음 <br>
 내용을 정리하고 <br>
 레이아웃을 다시 정리하고 <br> 
 [PR](https://github.com/JohnCoene/waiter/pull/45)을 올렸다.
 
 작업에는 한 3 ~ 4시간 정도 걸린 것 같다.
 
 생각보다 빨리 response가 왔고, `gitignore` 나 `test` 같은 항목에 문제가 있어서,<br>
 원 저자 [JohnCoene](https://github.com/JohnCoene)의 도움을 받아 약간의 추가 작업 후 PR merge 되었다.
 
 ![image](https://user-images.githubusercontent.com/6457691/73797954-b99eb600-47f4-11ea-90e4-2d87e394cb83.png)

 ![image](https://user-images.githubusercontent.com/6457691/73798178-6e38d780-47f5-11ea-8a0e-f8d83b98f86f.png)
 
 이미지들은 각각 template과 final cheatsheet <br>
 
 크킄
