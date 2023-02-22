---
layout: post
title: "Create Error Page"
categories: TeamProject
teags: javascript 
---

# 에러 페이지 생성
- navigo의 notFound()를 사용해서 만약 사용자가 조건에 맞지않는 경로를 입력하여 들어갔을 때 보여줄 에러페이지를 제작함
- 메인페이지에 사용된 헤더 엘리먼트와 함께, 오류 코멘트를 보여주며, 5초 뒤에 메인페이지로 이동시킨다.
- 타이머 애니메이션을 추가해서 시각적으로 시간의 흐름을 보여준다.

## appendErrorPage()
- error 페이지 innerHTML을 제작한다음 setTimeout을 사용해서 5초 뒤에 location.href에 작성된 링크로 이동시킨다.


![ezgif com-video-to-gif (5)](https://user-images.githubusercontent.com/56331400/220658827-b97fd474-3963-4cb1-a761-f68b3a50a5a5.gif)
