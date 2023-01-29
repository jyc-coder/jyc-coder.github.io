---
layout: post
title:  "Draggable"
categories: TodoList
tags : sortableJs
---
# drag n drop 기능 구현

sortablejs 라이브러리를 사용해서 할일 목록을 드래그하여 사용할수 있게 구현함 

```js
import Sortable from './node_modules/sortablejs/modular/sortable.core.esm.js'

Sortable.create(listEl, {
      animation: 150,
      ghostClass: 'blue-background-class',
    })
```
