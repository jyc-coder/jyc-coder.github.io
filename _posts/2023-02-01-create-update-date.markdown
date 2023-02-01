---
layout: post
title:  "Added Create/Update Date"
categories: TodoList
tags : sortableJs momentJs Javascript
---

# 생성 및 수정 일자 추가

`render.js`내부에 생성일자와 수정일자 데이터를 표시할 span 엘리먼트를 생성하고 readTodos()로 가져온 todos에 내장된 생성일자(todos.createAt) 와 수정일자(todos.updatedAt)를 각각의
innerHTML에 추가시켰다.
`js/render.js`
```js
    const createDateEl = $('span')
    const editDateEl = $('span')
    .
    .
    .
    
 editEl.textContent = '수정'
    deleteEl.textContent = '삭제!'
    createDateEl.className = 'list__createdate'
    createDateEl.innerHTML = `생성 일자 : ${moment(todo.createdAt).format(
      'YY년 M월 D일, h:mm a'
    )}`
    editDateEl.className = 'list__editdate'
    editDateEl.innerHTML = `수정 일자 : ${moment(todo.updatedAt).format(
      'YY년 M월 D일, h:mm a'
    )}`
```

![image](https://user-images.githubusercontent.com/56331400/215970344-c2693cba-cb42-4095-bb24-7c0f6467c95b.png)

# editEl의 addEventListener 수정

일자 추가 기능을 추가하기 전에는 input의 value 값이 그대로 유지되어도 문제 없었기 때문에 처음에 가져온 todos를 rederTodos의 파라미터로 설정했지만, 할일 목록을 작성한 뒤에 수정 일자를 
바로 최신화 주고 싶었기 때문에 readTodos()를 다시 호출해서 얻은 데이터 `updateTodos`를 파라미터로 rederTodos를 호출했다.
`js/render.js`
```js
 editEl.addEventListener('click', async () => {
      const edit = await updateTodo(todo)
      const updatedTodos = await readTodos()
      renderTodos(updatedTodos)
    })
```

![ezgif com-gif-maker (56)](https://user-images.githubusercontent.com/56331400/215973574-6b8c8b8a-83c0-43f1-97f9-8c143d122d5c.gif)
