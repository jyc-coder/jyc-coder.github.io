---
layout: post
title: "Impliment Search Filter"
categories: TeamProject
teags: javascript 
---


# 검색 필터 구현

## appendFilterList(category,menu)
- 최상위 메뉴를 생성하는 메소드
- category : 메뉴 이름 , menu : 하위 메뉴 정보
- 최상위 메뉴를 클릭했을때 하위 메뉴들을 보여준다.
- menu는 내부에 존재하는 appendMenu(container,menu,isfirst) 메소드의 파라미터로 사용됨

## appendMenu(container,menu,isfirst)
- ul,li 엘리먼트를 생성하고 search_filter에 append 하는 메소드
- 재귀 용법을 사용하여 세부 메뉴가 존재하는 경우 해당 메소드를 계속 호출하여 세부메뉴가 없을 때까지 호출한다.
- li엘리먼트에는 해당 메뉴이름을 값으로하는 tag-name 데이터 속성을 부여함
- 생성된 li에 이벤트 부여 
  - 기존에 존재한 제품 목록을 제거한다 
  - 클릭한 메뉴에 존재하는 tag-name을 파라미터로 사용하여 appendSmallProducts()를 호출한다.
  - toggleOnly(li)를 호출한다
  ### toggleOnly(me)
  - 체크박스 클릭시 자신을 제외한 모든 항목의 체크해제, 자신의 체크를 토글하는 목적으로 제작함
  - search_filter하위에 존재하는 menu 클래스를 가진 엘리먼트를 모두 선택한 배열을 사용해서 me를 제외한 모든 항목의 이미지를 unchecked로 변경한다. 
  - 만약 다른 카테고리의 메뉴를 선택할 경우 이전의 카테고리 메뉴의 서브메뉴를 보이지 않게 제작했음
  

# 가격별 검색은 어떻게 했는가?

- appendSmallItem(tag, dpnum, listIndex)에 내용을 추가함
- resultData를 searchByTag(tag)를 호출해서 얻은 값으로 지정했으나, 그 대신 tag의 값에 따라서 resultData에 다른 값들을 삽입했다. 가격 조건 태그는 searchByTag에 들어가는 조건이 될수 없기 때문
- 먼저 searchAll()을 호출하여 얻은 데이터(allData)를 선언한뒤, switch문을 사용해서 tag의 값에 따라서 allData.filter()로 조건에 맞는 값을 얻는다.
- 그리고 default로 이전에 사용하던 searchByTag(tag)를 사용한다.

![ezgif com-video-to-gif (4)](https://user-images.githubusercontent.com/56331400/220656884-03c99d22-7315-4813-b41f-a5b5580d40d0.gif)

