---
layout: post
title:  "Webpack error"
categories: TodoList
tags : webpack
---

# webpack 빌드 오류 발생

momentjs를 import 해서 사용하려고 했는데, 어째서인지 빌드 오류가 발생했다. 
- live server 에서는 npm 모듈을 사용할수 없음
- 그래서 webpack으로 빌드를 하려고 했는데 `Field 'browser' doesn't contain a valid alias configuration` 이런 오류가 나타나면서 파일을 resolve 할수 없다고 함


## 해결 방법
생각보다 간단했다. 프로젝트 폴더 내부에 파일을 정리한답시고 폴더를 생성해서 파일을 집어넣었는데, 이를 webpack 엔트리 파일 path에 업데이트 하지 않은것이 원인!
index.js와 main.css 를 각각 js와 css라는 이름의 폴더 내부로 옮겼었다.

이전: `entry: ['./index.js', './main.css'],`
이후: `entry: ['./js/index.js', './css/main.css'],`

이렇게 옮기고 빌드를 진행하니 제대로 동작했다! 
짧은 내용이지만 이렇게 올린 이유는 이런 실수는 되도록이면 만들지 말라는 의도로 작성했다.
