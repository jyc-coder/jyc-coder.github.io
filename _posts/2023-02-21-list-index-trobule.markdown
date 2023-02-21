---
layout: post
title: "listIndex trouble"
categories: TeamProject
teags: javascript 
---


# 문제 1 : index.js 에 appendProducts를 복수로 호출하면 고정된 위치에서만 엘리먼트가 나타난다

- appendProducts는 작성된 순서에 따라서 body에 append되는 방식으로 구현했었는데, 막상 다른 위치에서 복수로 호출했는데도, 처음에 호출한 곳에서 중복으로 렌더링 되는 현상이 발견됨

## 해결 
-제품 목록이 들어가는 리스트 엘리먼트를 선택할때 document.queryselector로 선택을 해버리니까, 2번째 appendProduct에서도 해당 요소를 찾을때 가장 처음에 존재하는 요소를 선택하여 
렌더링 되어서 발생하는 현상이었음
- 따라서 document 대신에 createElement로 생성한 productEl을 선택자로 변경함
-  const productListEl = productEl.querySelector('.product__list')

`js/products.js`
```js
export function appendProducts(tag, dpnum, num, listIndex) {
  const productEl = document.createElement('div')
  productEl.className = 'product'
  productEl.innerHTML = /*html */ `
     <div class="product__title">
        <div>
          <div class="title">Just Dropped</div>
          <div class="sub_title">발매 상품</div>
        </div>
      </div>
      <div class="product__list">
        <div class="product__list__first">
    </div>
      </div>
    `
  document.body.append(productEl)
  appendItem(tag, dpnum, num, productEl, listIndex)
  // 여기는 한번만 렌더링
  const productListEl = productEl.querySelector('.product__list')
  const moreListEl = document.createElement('div')
 ...
}
```
- appendProducts메소드 내부에 존재하는 appendItem에서도 같은 방식으로 렌더링을 해야하므로 파라미터에 container를 추가함
`js/products.js`
```js
 appendItem(tag, dpnum, num, productEl)
  // 여기는 한번만 렌더링
  const productListEl = productEl.querySelector('.product__list')
  const moreListEl = document.createElement('div')
  moreListEl.className = 'product__list__more'
  moreListEl.innerHTML = /*html*/ `
  <a href="javascript:void(0)" class="morebtn">더보기</a>
  `
  productListEl.after(moreListEl)

  const morebtnEl = productEl.querySelector('.morebtn')
  morebtnEl.addEventListener('click', () => {
    listIndex++
    appendItem(tag, dpnum, num, productEl)
  })
```

# 문제 2 : listIndex 값이 공유된 상태에서 더보기 기능이 제대로 동작하지 않음
- 이전 방식 : listIndex 변수를 export 하여 각각의 appendProducts가 사용하는데, 이렇게 해버리면 첫번째로 더보기를 클릭한 다음 다른 곳에서 더보기를 클릭하면 오류가 발생함
- listIndex가 0에서 증가하여 다음 데이터를 불러오는데, `더보기`를 클릭한 다음 다른 product 엘리먼트의 더보기를 클릭하면, listIndex가 증가한 상태이기 때문에 오류가 생겨버린다.

## 해결
- 파라미터를 listIndex를 추가해서 독립적으로 index값이 적용되도록 변경했음
- appendProducts, appendItem, appendSamllProducts, appendSmallItem 의 파라미터에 모두 listIndex를 추가했음

![ezgif com-video-to-gif (3)](https://user-images.githubusercontent.com/56331400/220306665-d3db6e3c-8403-4f19-8d05-ac0922909a4a.gif)

