---
published: false
---
---

layout: post
title: 'sort option'
categories: TodoList
tags: Javascript

---
# select option 값에 따른 렌더링 구현하기


할 일을 완료하지 않은 항목과 완료한 항목을 분류해서 출력하기 
select 엘리먼트를 생성하고 `addEventListener`를 추가해서 선택한 oprion의 값에 따라서 데이터 값을 가공한다.
filter 메소드를 사용해서 readTodos를 호출하여 가져온 데이터를 가공해서 renderTodos의 파라미터로 사용한다.
```js
sortEl.addEventListener('change', async (e) => {
  const todos = await readTodos()
  const doneTodos = todos.filter((e) => e.done === true)
  const notDoneTodos = todos.filter((e) => e.done === false)
  switch (e.target.value) {
    case 'all':
      renderTodos(todos)
      break
    case 'done':
      renderTodos(doneTodos)
      break
    case 'notdone':
      renderTodos(notDoneTodos)
      break
    default:
      break
  }
})
```
![GOMCAM 20230129_1658580372.gif]({{site.baseurl}}/_posts/GOMCAM 20230129_1658580372.gif)

# 정렬된 데이터 드래그 앤 드랍후 전체 데이터 렌더링 현상

이렇게 선택된 조건으로 렌더링된 데이터를 드래그 앤 드랍을 하면 전체 항목이 렌더링 되는 현상이 발생한다.
이런 현상이 발생하는 이유는 `render.js`에서 `Sortable`의 onEnd 옵션에서 드래그 앤 드랍을 통해 `updateTodo`를 통해서 수정이 된 다음 rendoerTodos(todos)를 호출해서 데이터 전체를 렌더링 해버리기 때문이었고 이를 수정하기 위해서 select 엘리먼트를 불러와서 해당 값에 따라 다른 데이터를 렌더링 하게 설정했다.
```js
  /** 항목 드래그 앤 드롭  */
const sortEl = document.querySelector('#todolist')
.
.
.

    Sortable.create(listEl, {
      animation: 150,
      ghostClass: 'blue-background-class',

      onEnd: async function (evt) {
        const todos = await readTodos()
        let selTodos = []
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
            break
        }
        const oldData = selTodos[evt.oldIndex]
        selTodos.splice(evt.oldIndex, 1)
        selTodos.splice(evt.newIndex, 0, oldData)
        selTodos.forEach(async (todo, idx) => {
          todo.order = idx
          await updateTodo(todo)
        })
        await renderTodos(selTodos)
      },
    })
```
switch문으로 `sortEl.value`에 값에 따라서 변경된 파라미터로 renderTodos를 호출한다.

# 조건 데이터 정렬후 드래그를 하고 다시 새로고침을 하면 원본 순서가 변경되는 현상발생

원본 데이터의 순서는 그대로 유지하고 조건 정렬을 했을때는 그냥 드래그 기능만 하도록 설정하려고 했는데, 조건을 따로 부여하지 않아서 드래그만 하면 무조건 order값 재설정 프로세스가 동작해서 일어난 문제였다.

따라서 sortEl.value값이 특정 값일 경우에만 드래그 할때 order값 재설정 프로세스가 동작하게 수정했다.

```js
if (!sortEl.value || sortEl.value === 'all') {
          selTodos.splice(evt.oldIndex, 1)
          selTodos.splice(evt.newIndex, 0, oldData)
          selTodos.forEach(async (todo, idx) => {
            todo.order = idx
            await updateTodo(todo)
          })
          await renderTodos(selTodos)
        }
```
이렇게 해주고 다시 결과를 확인해봤다.


![ezgif.com-gif-maker (55).gif]({{site.baseurl}}/_posts/ezgif.com-gif-maker (55).gif)
처음 상태에서 순서를 변경하고 option을 선택한뒤 맘대로 드래그를 해도 원래 순서 그대로 돌아와있다.
