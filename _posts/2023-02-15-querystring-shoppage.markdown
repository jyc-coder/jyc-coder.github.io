---
layout: post
title: "querystring error, make shop page"
categories: TeamProject
teags: javascript 
---

# a태그 href에 쿼리스트링을 추가한 다음 이벤트 전파로 인해서 찜목록을 클릭해도 상세페이지로 넘어가버린다.
- 쿼리스트링 추가 전에는 찜목록 기능이 제대로 작동하는 듯 했으나, 쿼리스트링을 추가한 후에는 이벤트 버블링을 막아내지 못하고 상세 페이지로 이동해버린다.
## 해결 방법
- 찜목록 아이콘의 onclick에 원래 기능을 막아버리는 e.preventDefault()를 호출한 다음 버블링 차단을 위해서 e.stopPropagation()을 호출했더니 해결되었다.
`js/item.js`
```js
wishicon.onclick = (e) => {
      // 찜목록 클릭시, 찜목록 이미지 src값에 따라 제품의 id값을 로컬 스토리지에 추가/제거
      if (wishicon.src == wishOn) {
        wishlist = wishlist.filter((e) => e !== el.getAttribute('data-id'))
        localStorage.setItem('wishlist', wishlist)
        console.log(wishlist)
      } else {
        wishlist.push(el.getAttribute('data-id'))
        localStorage.setItem('wishlist', wishlist)
        console.log(wishlist)
      }
      wishicon.src = wishicon.src === wishOff ? wishOn : wishOff

      e.preventDefault()
      e.stopPropagation()
    }
  })
```

# 상점 페이지 레이아웃 구현 
- 상점의 제품목록과 필터링 옵션 컴포넌트를 렌더링하는 메소드를 제작했음
- 상점 페이지의 제품 아이템의 이미지 크기가 살짝 작았으면 해서 이미지 크기를 줄인 버전의 메소드를 제작했다.appendSmallProducts(tag, dpnum, num, content)
- 4번째 파라미터는 append되는 위치로, 상점페이지에서는 body가 아니라 content라는 컨테이너 안에서 구현될 것이기 때문에 따로 메소드를 제작함!
- 현재 체크박스 필터는 제대로 구현되지않았음. 서로 다른 깊이의 데이터를 어떤 방식으로 선택창을 만들어야할지 고민해봐야겠음
`js/shop.js`
```js
export default function appendShopContent() {
  const catList = ['카테고리', '브랜드', '성별', '가격']
  const contentEl = document.createElement('div')
  contentEl.className = 'content'

  contentEl.innerHTML = /*html */ `
    <div class="search_filter">
        <div class="filter_status">
          <div class="status_box">
            <span class="status_text"> 필터 </span>
          </div>
        </div>
     `

  document.body.append(contentEl)
  const searchFilterEl = document.querySelector('.search_filter')
  const filterListEl = document.createElement('div')
  filterListEl.className = 'filter_list'

  catList.forEach((e) => {
    const filterListEl = document.createElement('div')
    filterListEl.className = 'filter_list'
    filterListEl.innerHTML = /*html */ `
     <div class="filter_title">
            <div class="title_box">
              <span class="main_title">${e}</span>
              <div class="placeholder">모든 ${e}</div>
            </div>
            <div class="icon_box"><img class="more_icon" src=${plus}  alt="확장" /></div>
          </div>
      `

    searchFilterEl.append(filterListEl)
  })
  appendSmallProducts(' 남성', 4, 12, contentEl)
}

```
![image](https://user-images.githubusercontent.com/56331400/219062486-085ca30c-f296-4a5d-9330-683bfa79b54a.png)


TODO
- 체크박스 필터 컴포넌트 제작하기
- 체크하면 검색 결과를 렌더링하는 기능 구현
