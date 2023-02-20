---
layout: post
title: "create search filter"
categories: TeamProject
teags: javascript 
---



# 검색 필터 리스트 만들기
- 최상위 타이틀 내부에 여러개의 메뉴들이 추가된다.
```js
export function appendFilterList(category, menu) {
  //filterList 렌더링
  const filterListEl = document.createElement('div')
  filterListEl.className = 'filter_list'
  filterListEl.innerHTML = /*html */ `
    <div class="filter_title">
       <div class="title_box">
              <span class="main_title">${category}</span>
              <div class="placeholder">모든 ${category}</div>
            </div>
            <div class="icon_box"><img src=${plus} alt="확장" /></div>
            
    </div>
    <div class="filter_menu"> </div>
  `
  document.querySelector('.search_filter').append(filterListEl)
  }
```



- 필터 리스트를 렌더링하는 `appendMenu(container,menu,isfirst)`를 구현함
- `container` : 렌더링 위치 , `menu`: 메뉴 정보, `isfirst`: 최상위 메뉴인지 아닌지 여부 파악을 위한 변수
- 재귀 방식으로 메소드를 구현해서, ul엘리먼트를 생성한 다음 그 안에 li엘리먼트를 추가한다.
- 만약 세부 메뉴(submenus)가 존재한다면 해당 메소드를 한번 더 호출한다. 
- 세부 메뉴가 다 더이상 존재하지 않는다면 거기서 종료
- 세부 메뉴와 상위 메뉴의 구분을 위해서 ul의 클래스에 sub을 추가했고, sub 클래스에는 padding 값을 부여했다.
`css/shop.scss`
```css
 .menu_list {
          box-sizing: border-box;
          margin: 0;
          padding: 0;
          list-style: none;
          max-height: 315px;
          overflow-y: auto;
          .sub {
            padding: 5px 0 6px 25px;
            display: none;
          }
          ...
          }
```

`js/shop.js`
```js
export function appendMenu(container, menu, isfirst, test) {
  if (!menu || !menu.length) return
  console.log(container, menu)
  const ul = document.createElement('ul')
  ul.className = isfirst ? 'menu_list' : 'menu_list sub'

  for (const { title, submenus } of menu) {
    // 체크박스에 태그 이름과 체크 여부 파악을 위한 데이터 속성 부여
    const li = document.createElement('li')
    li.className = 'menu'
    li.innerHTML = /*html */ `
     <a href="#" class="menu_link">
                  <img src=${unchecked} alt="체크박스" />
                  <span class="link_text">${title}</span>
    `
    if (submenus) {
      appendMenu(li, submenus, false, true)
      li.className = 'menu'
    }

    ul.appendChild(li)
  }
  ...
  
  }
```
![image](https://user-images.githubusercontent.com/56331400/220144279-aee0b1e7-6fe6-46c0-a038-08d5a6c1c364.png)

외관상으로는 좋지만 아직 기능을 추가하지 않았다.

# 상위 타이틀을 클릭하면 메뉴가 나타나고, 메뉴를 클릭하면 세부 메뉴가 나타나게 해야한다.

- 각각의 메뉴에 이벤트를 추가했고, 클릭하면 display의 속성을 토글하는 방식으로 구현했다.
```js
// 기능 구현 1. 하나만 체크할수 있게 설정
  const menuListEl = container.querySelector('.menu_list')
  const menuEls = menuListEl.querySelectorAll('.menu')

  menuEls.forEach((e) => {
 
    e.addEventListener('click', (target) => {
      // console.log('click')
      target.preventDefault()
      target.stopPropagation()
    
      // console.log(target.target.innerText)
      menuEls.forEach((j) => {
        const checkbox = j.querySelector('img')
        checkbox.src = unchecked
      })
      e.querySelector('img').src =
        e.querySelector('img').src === unchecked ? checked : unchecked
      const subEls = e.querySelector('.menu_list .sub')
      if (subEls) {
        subEls.style.display =
          subEls.style.display === 'block' ? 'none' : 'block'
      }
      target.stopPropagation()
    })
  })
```
## 문제: 세부 메뉴를 클릭했을때 이벤트가 2번 발생한다.

- 항목을 클릭했을때 'click'이 2번 발생하는 문제가 있었다. 
- 렌더링을 전부 진행한 다음에 기능을 추가했는데, 중복으로 추가된 것이 원인으로 추정됨

## 해결 : 이벤트를 추가하는 위치를 변경함
- li 엘리먼트를 생성하고 innerHTML에 내용을 추가한 다음에 li에 토글 메소드를 추가했다.
- 재귀 방식으로 메소드를 구현하다보니 전체 만들어진 엘리먼트에 기능을 추가하면서 중복될 가능성이 있다고 판단하여 위치를 변경했더니 해결됨

## 문제2: 다른 메뉴를 클릭할때 다른 곳의 체크박스를 전부 해제하고 싶다.
- 체크박스를 클릭했을때 이미지 변경이 잘 되지만 전체 필터중에서 선택되는 곳을 한군데로 줄여서 클릭된 태그에 해당하는 결과값을 렌더링 해주고 싶었다.

## 해결 : 선택된 항목을 제외한 나머지 전부의 이미지를 변경함
- search_filter 내부의 menu 클래스를 가진 li 엘리먼트 (allMenuEl)을 선언함
- allMenuEl을 배열화 시켜서 선택된 li를 제외한 모든 엘리먼트의 이미지src를 unchecked로 변경함으로써 문제 해결

```js
export function appendMenu(container, menu, isfirst, test) {
  ...

  for (const { title, submenus } of menu) {
    // 체크박스에 태그 이름과 체크 여부 파악을 위한 데이터 속성 부여
    const li = document.createElement('li')
    li.className = 'menu'
    li.innerHTML = /*html */ `
     <a href="#" class="menu_link">
                  <img src=${unchecked} alt="체크박스" />
                  <span class="link_text">${title}</span>
    `
    function toggleSiblings(me) {
      const checkbox = me.querySelector('img')
      checkbox.src = checked
      const subEls = me.querySelector('.menu_list.sub')
      if (subEls) {
        subEls.style.display =
          subEls.style.display === 'block' ? 'none' : 'block'
      }
      const allMenuEl = document
        .querySelector('.search_filter')
        .querySelectorAll('.menu')
      Array.from(allMenuEl)
        .filter((el) => el !== me)
        .forEach((menu) => {
          const checkbox = menu.querySelector('img')
          checkbox.src = unchecked
        })
    }
    li.addEventListener('click', (event) => {
      console.log(event.currentTarget.textContent.trim())
      event.preventDefault()
      event.stopPropagation()
      toggleSiblings(li)
    })

    if (submenus) {
      appendMenu(li, submenus, false, true)
      li.className = 'menu'
    }

    ul.appendChild(li)
  }

  container.appendChild(ul)
  
}
```


## 문제3 : 다른 상위 메뉴를 선택했을 때 이전에 고른 메뉴의 세부메뉴를 숨기고 싶다.
- 이제 하나씩만 체크되지만, 다른 메뉴를 선택했을때, 보여졌던 세부메뉴가 그대로 남아있는 현상이 있었다.

## 해결 : 배열화 시겼던 allMEnuEl에 서브메뉴 display style 변경 코드 추가
- 선택된 요소를 제외한 나머지 엘리먼트의 이미지 src를 변경했듯이, 나머지 세부메뉴들을 전부 선택해서 display 속성을 변경함
- menu의 서브메뉴가 존재하고, 선택된 엘리먼트의 세부메뉴가 아니라면, 해당 display 속성을 none으로 변경함
`toggleSiblings -> toggleOnly`로 이름 변경

```
function toggleOnly(me) {
      const checkbox = me.querySelector('img')
      checkbox.src = checked
      const subEls = me.querySelector('.menu_list.sub')
      if (subEls) {
        subEls.style.display =
          subEls.style.display === 'block' ? 'none' : 'block'
      }
      const allMenuEl = document
        .querySelector('.search_filter')
        .querySelectorAll('.menu')
      Array.from(allMenuEl)
        .filter((el) => el !== me)
        .forEach((menu) => {
          const checkbox = menu.querySelector('img')
          checkbox.src = unchecked
          const subEls = menu.querySelector('.menu_list.sub')
          if (subEls && !menu.contains(me)) {
            subEls.style.display = 'none'
          }
        })
    }
```


# 결과

![ezgif com-video-to-gif (2)](https://user-images.githubusercontent.com/56331400/220155892-9a0f1f80-0715-4f78-ba0d-2bd804dd922c.gif)

원하는 방식대로 동작되는 것을 확인함

# TODO
- 체크박스에 해당하는 제품 렌더링
- 체크 필터가 같이 스크롤되어 따라오는 기능 구현

## OPTION
- 무한 스크롤
