---
layout: post
title:  "Save After Drag"
categories: TodoList
tags : sortableJs Javascript

---
# 드래그 한 뒤 목록 저장

- 할일 요소를 옮긴 뒤 order 이 순서에 맞게 정렬되게 설정함
- sortablejs의 onEnd property에서 구한 이전 인덱스와 옮겨진 뒤의 인덱스값을 통해서 todos 데이터 배열의 순서를 splice 메서드를 사용하여 변경함

```js
Sortable.create(listEl, {
  animation: 150,
  ghostClass: 'blue-background-class',

  onEnd: async function (evt) {
    // 현재 api에 저장된 할일 목록 데이터를 불러옴
    const todos = await readTodos()
    // 배열이 변경되기전 드래그가 시작된 항목을 저장해놓은 변수를 선언
    const oldData = todos[evt.oldIndex]
    // 드래그 된 항목을 배열에서 제거
    todos.splice(evt.oldIndex, 1)
    // 드래그된 위치에서 oldData 추가함
    todos.splice(evt.newIndex, 0, oldData)
    
    await renderTodos(todos)
  },
})
```
이렇게만 해버리면 위치를 옮긴 뒤 새로고침을 해도 저장이 되기는 하지만 order의 값이 뒤죽박죽인지라. 이를 위해서 todos의 데이터들의 order 값을 현재 위치한 인덱스 값으로 변경하는 작업을 따로 진행했다.
```js
    todos.forEach(async (todo, idx) => {
      todo.order = idx
      await updateTodo(todo)
    })
```
변경한 내용을 api로 전달해야 새로고침을 해서 가져올때 그대로 전달 되기 때문에 updateTodo를 반복 호출했다.

