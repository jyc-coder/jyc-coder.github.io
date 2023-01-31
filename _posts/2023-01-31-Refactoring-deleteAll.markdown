---
layout: post
title:  "Refactoring And Add DeleteAll Button"
categories: TodoList
tags : Javascript
---

# document selector 메소드 모듈화

여러개의 document 관련 메소드가 호출되는 코드가 지저분해보여서 document.createElement 와 document.querySelector를 모듈화 시켜서 리팩토링을 진행하기로 함. 

`js/docCreate.js`
```js
export const $ = (selector) => document.createElement(selector)
export const $$ = (selector) => document.querySelector(selector)
```


# DeleteAll 기능 버튼 추가

전체 항목을 지울수 있는 기능을 가진 버튼을 구현함
- `render.js`내부에 `deleteAllBtnEl` 버튼 엘리먼트를 생성하고 클릭하면 `todoListEl`의 `innerHTML`을 공백으로 변경해서 사용자의 눈에 보이지 않게 설정함
- 물론 데이터 자체도 지워져야 하기 때문에 데이터 배열 todos를 forEach를 사용해서 반복적으로 deleteTodo(todo)를 호출해서 데이터를 제거함
- 처음에는 지워진 데이터를 다시 readTodos()를 호출해서 데이터의 길이가 0이라면 공백처리를 하려고 했지만, 빨리 지워지지 않아서 시각적으로 보이지 않게만 해주면 새로고침할때 renderTodos()가 호출 되기 때문에 선택하지 않음

`js/render.js`
```js
const deleteAllBtnEl = $('button')
  deleteAllBtnEl.className = 'delete-all'
  deleteAllBtnEl.innerHTML = '전부 제거하기'
  .
  .
  .
  deleteAllBtnEl.addEventListener('click', async () => {
    todos.forEach(async (e) => await deleteTodo(e))
    todoListEl.innerHTML = ''
  })
```
