---
layout: post
title: "Added wishlist, queryString"
categories: TeamProject
teags: javascript 
---

# 찜목록 기능 구현

## 제품 찜목록 클릭시 제품 id 값 로컬 스토리지에 저장
- 로드될때 로컬스토리지에 들어있는 wishlist값을 불러오는 배열 데이터를 생성함 (let wishlist). 현재의 wishlist 데이터에서 추가/제거를 진행하기 위해서
- 만약 wishlist 값이 공백일 경우에는 빈 배열로 지정하고 하나라도 제품 찜목록 데이터가 존재할 경우에는 해당 데이터들을 불러온다!
- 각각의 `product__item`엘리먼트의 찜목록 icon을 클릭할,때 icon의 이미지에 따라서 추가/제거 되도록 기능을 구현함
- 이때 제품 이미지를 클릭하는 이벤트와 겹치지 않게 버블링을 차단하기 위해서 `e.stopPropagation()`을 추가함
- 찜목록 추가를 하면 로컬스토리지에 제품 id가 추가되며, 찜목록에서 제거할 때는 filter메소드를 사용해서 선택된 제품 id를 제외한 나머지 데이터를 setItem한다.
`js/item.js`
```js
items.forEach((el, index) => {
  // 로드 되면서 위시리스트의 데이터를 가져온다. 만약 아무것도 없으면 빈 배열로 지정
  let wishlist =
    localStorage.getItem('wishlist') === null
      ? []
      : localStorage.getItem('wishlist').split(',')

   .
   .
   .

    wishicon.onclick = (e) => {
      // 찜목록 클릭시, 찜목록 이미지 src값에 따라 제품의 id값을 로컬 스토리지에 추가/제거
      if (wishicon.src == wishOn) {
        wishlist = wishlist.filter((e) => e !== el.getAttribute('data-id'))
        localStorage.setItem('wishlist', wishlist)
      } else {
        wishlist.push(el.getAttribute('data-id'))
        localStorage.setItem('wishlist', wishlist)
      }
      wishicon.src = wishicon.src === wishOff ? wishOn : wishOff
      e.stopPropagation()
    }
  })
```

## 제품 목록에 쿼리 스트링 추가
- 구매 페이지로 이동할 때, 해당 제품의 id값을 쿼리스트링으로 전송해서 해당 값을 사용하여 상세 제품 정보를 얻을수 있게 작성함
- 쿼리 스트링은 name에 제품 id를 담는 방식으로 구현함

```js
items.forEach((el, index) => {
    // 쿼리 스트링 추가
    const wishLink = el.querySelector('.product__item__inner')
    wishLink.href = '/products' + `?name=${el.getAttribute('data-id')}`
    wishLink.addEventListener('click', (e) => {
      e.stopPropagation()
    })
  
  })
```
## 문제 발생1 : 찜목록이 비어있는 상태에서 찜목록에 아이템을 추가하면 로컬스토리지 맨앞에 ,가 붙는 현상 발생
- 찜목록 추가 제거가 잘 작동하는듯 했으나, 찜목록 항목을 전부 제거하고 난 다음에 새로고침후 항목을 클릭하면 로컬스토리지 wishlist 값의 맨앞에 ,가 붙는 현상이 발생
- 조사해본 결과 새로고침할때 wishlist의 값이 null일 경우에는 빈 배열로 선언했었는데 console.log로 확인해보니 빈 배열이 아니라 `['']` 이렇게 공백 요소가 존재하는 배열이었다.

### 해결 방법
- 새로고침할때 `localStorage.getItem('wishlist')`를 실행하면서 아무것도 없을때는 그 값이 null이므로 조건에 따라 빈 배열이 될줄알았는데, 그게 아니었다.
- 따라서 변수 wishlist 를 결정하는 조건식을 변경했다.   
```js
//전
localStorage.getItem('wishlist') === null ? []
      : localStorage.getItem('wishlist').split(',')

// 후
localStorage.getItem('wishlist').length === 0 ? []
      : localStorage.getItem('wishlist').split(',')
```
- 찜목록에 아무것도 없는 상태에서 wishlist를 불러왔을때 길이가 0이라면 빈 배열을 출력하게 변경한 다음에는 이런 현상이 발생하지 않았다.


![ezgif com-video-to-gif (1)](https://user-images.githubusercontent.com/56331400/218789476-a09eb088-1294-438b-9a39-d16db7780f5d.gif)
