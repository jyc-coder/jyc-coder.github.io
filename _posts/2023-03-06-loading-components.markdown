---
layout: post
title: "Create loading component"
categories: UI
teags: component UI
---


# 로딩 컴포넌트... 왜 만들지?

- 사용자가 취한 액션을 처리할 경우 사용하며, 처리 과정이 진행중인 것을 유저에게 알리기 위해
- 사용자가 취한 액션이 완료될때 까지 남은 시간을 대략적으로 유추할수 있게 해준다.

솔직히 버튼 클릭했는데 아무 변화도 없이 있으면 이게 진행이 되고 있는건지 안되고 있는건지 파악이 어렵긴 하다. 

# lottie..?

디자인 시스템을 둘러보면서 여러가지 로딩 컴포넌트가 있지만, 쏘카프레임 이라는 디자인 시스템 사이트를 둘러보다가 
처음 보는 태그를 봤다.


![image](https://user-images.githubusercontent.com/56331400/223053783-57c10647-bab5-4f3a-ad5a-6d44683f5911.png)

lottie 파일로 제작하여 구현한다..? [lottieFiles ](https://lottiefiles.com/) 에서 생성한 이미지를 사용하여 페이지에 움직이는 이미지를 
추가하는네 이때 사용하는 파일이 바로 lottie 파일이다. lottie파일은 lottie-player태그에서 src로 작성하면 나타나는 방식이다.

페이지에 들어가서 애니메이션을 검색하면 다른 사람들이 만든 여러가지 애니메이션을 둘러보고 사용할수 있다.


Lottie는 에어비엔비에서 개발한 오픈소스 모바일 라이브러이이다. JSON 기반 애니메이션 파일 형식으로 실시간으로 애니메이션을 렌더링한다.
벡터는 점과 점을 잇는 선분과 면으로 이루어져있기 때문에 아무리 확대해도 깨지지 않는다

## 특징
- 벡터 기반이라 용량이 적고 해상도 저하가 없다.
- 개발에서 적용이 간단하다
- 웹,IOS,Android,Window 및 기타 플랫폼을 위한 오픈 소스 및 무료 플에이어가 있어서 변환 없이 사용이 가능하다.
- 사용자와 상호 소통이 가능한 애니메이션을 만들 수 있다.

# 나도 만들어볼래!

그래서 방법을 찾고 있는데, Adobe After Effects 없이도 로고에 애니메이션 효과를 주는 방법에 관련된 게시글을 페이지에 올려놓았길래 링크를 가져왔다.

링크 : https://lottiefiles.com/kr/blog/tutorials/kr-how-to-animate-logo-without-after-effects

간단하게 요약하면

1. Figma에서 로고 만들기
2. LottieFiles의 SVG to Lottie 툴에 끌어다 놓기
3. 내 애니메이션을 저장하기
4. 마음 껏 쓰세용



좋아 그럼 이대로 해보자! 


1. 피그마에 들어가서 진짜 아무생각없이 로고를 만들었다.


![sample](https://user-images.githubusercontent.com/56331400/223127999-85ee1873-f1b0-4f13-b159-19a895e17619.svg)

이제 이녀석을 로딩 애니메이션으로 만들어보자.

2. svt-tolottie 라는 사이트를 사용해서 svg를 lottie로 만든다 https://lottiefiles.com/svg-to-lottie

 
![image](https://user-images.githubusercontent.com/56331400/223128581-7392d3fb-ee86-4b2f-8902-1790e1be0f3f.png)

원하는 svg 를 드래그하여 추가하면 여러가지 애니메이션 효과를 가진 로고 이미지를 만들어낼수 있다.

![GOMCAM 20230306_2250540366](https://user-images.githubusercontent.com/56331400/223128833-9f5283b5-48eb-4017-b22d-3e0ca056390c.gif)

원하는 이미지 효과를 선택하고 다운로드를 하면 json 파일이 하나 생성되는데 이것을 프로젝트 폴더에 집어넣고 제대로 동작하는지 확인해봤다.

`images/lottie/` 경로에 `mylogo.json`이라는 파일을 집어넣었고, index.html 에 lottieplayer를 사용하기위한 script를추가하고 body에 lottie-player엘리먼트를 추가했다.

```html
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link
      rel="stylesheet"
      href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@48,400,0,0"
    />
    <link rel="stylesheet" href="/css/main.css" />

    <title>Document</title>
  </head>
  <body>
    <lottie-player
      src="./images/lottie/mylogo.json"
      background="transparent"
      style="width: 300px; height: 225px"
      speed="1.5"
      loop
      autoplay
    >
    </lottie-player>
  </body>
  <script src="https://unpkg.com/@lottiefiles/lottie-player@latest/dist/lottie-player.js"></script>
</html>
```
이렇게 해주고 페이지를 켜보면?


![GOMCAM 20230306_2306230108](https://user-images.githubusercontent.com/56331400/223132587-fb7d43ac-eb71-4ec7-a86c-f66ab21b3471.gif)

이렇게 내가 만든 로고가 로딩 애니메이션으로 만들어졌다.
참고로 src 뒤에 속성이 있는데 이를 통해서 반복 속도 및 여러가지 설정을 변경할수 있다.
- speed : 재생 속도
- loop : 반복 여부. 만약 해당 요소가 없으면 한번만 재생되고 반복되지 않는다.
- autoplay : 자동 재생 여부

쏘카프레임 사이트를 둘러보면서 '과연 아름답고 멋있는 컴포넌트가 정답일까?' 라는 생각을 해보게 됐다. 적은 용량으로도 가성비 좋은 컴포넌트를 사용해서
성능을 개선시키기 위해서, 쏘카 디자인 시스템 사이트에서는 lottie-player를 사용한 듯하다. 실시간으로 차량을 대여해주는 서비스이다보니, 다른 곳에서 용량을 낭비하고 싶지
않기 위해서 이런 방법을 선택한듯 하다.


