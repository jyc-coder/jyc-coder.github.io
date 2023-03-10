---
layout: post
title:  "edit sortable"
categories: TodoList
tags: Javascript
---


# Sortable 생성 메소드 위치 변경

## 이전

render.js 파일의 renderTodos(todos) 메소드 내부에 작성해서 사용자가 항목을 추가/수정/삭제 할때마다 이 메서드가 호출되어서 성능 저하의 요인일수 있다고 판단하여
해당 메서드를 index.js로 옮겨서 로드된 상황에서 한번만 호출되게 위치를 변경했다. 

`render.js`
```js
import { readTodos, updateTodo, deleteTodo } from './request.js'

/** 할일 목록을 렌더링하는 메소드 */
async function renderTodos(todos) {
  console.log(todos)
  const listEl = document.querySelector('.list')
  const liEls = todos.map((todo) => {
    const liEl = document.createElement('li')
    const titleEl = document.createElement('input')
    const btnBoxEl = document.createElement('div')
    const checkLabelEl = document.createElement('label')
    const checkEl = document.createElement('input')
    const checkSpanEl = document.createElement('span')
    const deleteEl = document.createElement('button')
    const editEl = document.createElement('button')
    titleEl.className = 'list__title'
    btnBoxEl.className = 'list__btnbox'
    checkLabelEl.className = 'list__checkbox'
    checkEl.type = 'checkbox'
    checkEl.checked = todo.done
    checkEl.className = 'list__checkbox__check'
    checkSpanEl.className = 'list__checkbox__checkmark'
    editEl.className = 'list__editbtn'
    deleteEl.className = 'list__deletebtn'
    titleEl.value = todo.title
    editEl.textContent = '수정'
    deleteEl.textContent = '삭제!'
    checkLabelEl.append(checkEl, checkSpanEl)
    btnBoxEl.append(editEl, deleteEl)
    liEl.append(titleEl, checkLabelEl, btnBoxEl)

    checkEl.addEventListener('click', async () => {
      todo.done = !todo.done
      await updateTodo(todo)
      renderTodos(todos)
    })

    titleEl?.addEventListener('input', () => {
      todo.title = titleEl.value
      console.log(todo.title)
    })

    editEl.addEventListener('click', async () => {
      await updateTodo(todo)
      renderTodos(todos)
    })

    deleteEl?.addEventListener('click', async () => {
      await deleteTodo(todo)
      const todos = await readTodos()
      renderTodos(todos)
    })
    return liEl
  })

  listEl.innerHTML = ''
  listEl.append(...liEls)

  return liEls
}

export default renderTodos

```
sortable.create 메서드 내용을 전부 제거함

`index.js`

```js
/** 항목 드래그 앤 드롭  */
Sortable.create(listEl, {
  animation: 150,
  ghostClass: 'blue-background-class',

  onEnd: async function (evt) {
    const todos = await readTodos()
    let selTodos = []
    // 함수를 따로 만들면 되지 않았을까?
    switch (sortEl.value) {
      case 'all':
        selTodos = todos
        break
      case 'done':
        selTodos = todos.filter((e) => e.done === true)
        break
      case 'notdone':
        selTodos = todos.filter((e) => e.done === false)
        break
      default:
        selTodos = todos
        break
    }
    const oldData = selTodos[evt.oldIndex]

    if (!sortEl.value || sortEl.value === 'all') {
      selTodos.splice(evt.oldIndex, 1)
      selTodos.splice(evt.newIndex, 0, oldData)
      selTodos.forEach(async (todo, idx) => {
        todo.order = idx
        await updateTodo(todo)
      })
      await renderTodos(selTodos)
    }
  },
})
```

테스트 결과 기능동작에는 문제 없
