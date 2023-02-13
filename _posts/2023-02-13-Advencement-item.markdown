---
layout: post
title: "Advencement item.js"
categories: TeamProject
teags: javascript html css
---

# 더보기 버튼은 한번만 렌더링 되어야한다
따라서 더보기 엘리먼트 생성및 append 작업은 products.js에서 진행하도록 위치를 변경함 ( item.js -> products.js)

`js/produccts.js`
```js
  // 여기는 한번만 렌더링
  const productListEl = document.querySelector('.product__list')
  const moreListEl = document.createElement('div')
  moreListEl.className = 'product__list__more'
  moreListEl.innerHTML = /*html*/ `
  <a href="#" class="morebtn">더보기</a>
  `
  productListEl.after(moreListEl)

  const morebtnEl = document.querySelector('.morebtn')
  morebtnEl.addEventListener('click', () => {
    listIndex++
    appendItem(tag, num)
  })
```

# 처음과 나중의 렌더링 갯수를 다르게 하고싶다.
그래서 처음에는 4개만 렌더링 되고 더보기를 클릭하면 8개씩 렌더링 되게 구현했다.
- 빈 배열(chunk) 생성후 api요청을 통해 가져온 데이터를 slice해서 4개만 push한다
- 그 다음은 반복분을 사용해서 8개씩 요소를 push하는 방식으로 chunk 배열을 구현했다.
- chunk.map을 통해서 인덱스마다 배열의 길이만큼 반복적으로 아이템을 렌더링하면, 처음의 4개는 이미 보여진상태이므로 그 다음 요소의 길이인 8개가 렌더링되는것!

`js/item.js`
```js

const chunk = []
  const resultData = await searchByTag(tag)
  const chunkData = resultData.slice(0, num)
  chunk.push(chunkData.slice(0, 4))
  for (let i = 4; i < chunkData.length; i += 8) {
    chunk.push(chunkData.slice(i, i + 8))
  }
```
- 만약 제품목록을 다 보여줬다면 더보기 버튼을 제거하는 코드도 작성함 index는 0부터 시작하니까 1을 더한값과 chunk의 길이를 비교해서 같다면 그때는 더보기 엘리먼트를 제거하는 방식이다.
```
 // 모든 제품이 렌더링 되었을 경우 더보기 버튼 제거
  if (listIndex + 1 === chunk.length) {
    document.querySelector('.product__list__more').remove()
  }
```

# 파라미터를 추가해서 원하는 태그 검색결과를, 원하는 제품 갯수만큼 렌더링 하고 싶다. 
그래서 나는 원래의 appendITem()에 tag와 num 태그를 추가했다.
- tag : 검색어 태그, num: 제품 갯수(렌더링 아이템 갯수)
- 다른 사람들도 쉽게 적용시킬수 있도록, 두가지의 매개변수로 원하는 태그의 아이템을 원하는 갯수만큼 렌더링할수 있게 코드를 수정했다.
- searchByTag를 사용해서 나온 결과를 slice를 통해서 num만큼의 데이터만 사용하게 만들었다.
`js/item.js`
```js
 const resultData = await searchByTag(tag)
  const chunkData = resultData.slice(0, num)
```
예를 들어서 남성 태그에 해당하는 제품을 12개만 렌더링 하고 싶다면
`appendProducts(' 남성', 12)` 이렇게 해주면 된다.
![ezgif com-video-to-gif](https://user-images.githubusercontent.com/56331400/218380909-f26e2ab8-e6a7-4bc6-b88d-23b7d2b8a643.gif)




## TODO
- 더보기 버튼을 눌렀을때 맨위로 올라가지는 현상 제거
- 로딩 애니메이션 구현하기 
- 클릭했을때 상세 구매 페이지로 이동하는 경로 

