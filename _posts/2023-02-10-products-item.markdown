---
layout: post
title: "products-item-rendering"
categories: TeamProject
teags: javascript html css
---

# products.js 내부에 반복적으로 item이 렌더링되는 appendItem() 구현

`product__list`내부의 `product__list__first`안에 item이 여러가 렌더링 되는 메소드를 구현하기 위해서 item.js 파일을 따로 만들어서 아이템을 반복 렌더링하는 메서드를 만들었다.
다른 조원이 추가한 제품 데이터를 검색요청하는 api 메서드 `searchByTag`메서드를 사용해서 데이터 배열을 가져오고, 그 배열만큼 `createElement`를 호출하는 방식으로 구현했다.
제품의 이미지(e.thumbnail) , 제품이름(e.title), 제품 설명(e.description) 제품 가격(e.price) 를 추가해서 각각의 제품들을 나열했다.

더보기 버튼의 경우는 하나만 렌더링 되면 되므로 마지막에 append시켰음.

`js/item.js`
```js
/** 제품 아이템을 렌더링 하는 메소드 */
export async function appendItem() {
  const maleData = await searchByTag(' 남성')
  console.log(maleData)

  const productListEl = document.querySelector('.product__list')
  // 제품 아이템이 들어있는 리스트 엘리먼트
  const productFirstListEl = document.createElement('div')
  productFirstListEl.className = 'product__list__first'
  productListEl.append(productFirstListEl)
  // 제품 아이템 엘리먼트
  maleData.map((e) => {
    const productItemEl = document.createElement('div')
    productItemEl.classList = 'product__item'
    productItemEl.innerHTML = /*html */ `
            <a class="product__item__inner" href="#">
              <div class="thumb_box">
                <div class="item">
                  <picture class="item__img">
                    <img
                      src=${e.thumbnail}
                      alt="제품이미지"
                    />
                  </picture>
                  <span aria-label="관심상품" role="button" class="btn_wish">
                    <img src="${wishOff}" alt="찜" />
                  </span>
                </div>
              </div>
              <div class="info_box">
                <div class="brand">
                  <p class="brand__text">${e.title}</p>
                </div>
                <p class="name">${e.description}</p>
                <div class="price">
                  <div class="amount">
                    <em class="amount_num">${e.price}</em>
                  </div>
                </div>
              </div>
            </a>
  `
  // 반복해서 추가한다
    productFirstListEl.append(productItemEl)
  })

  // 여기는 한번만 렌더링
  const moreListEl = document.createElement('div')
  moreListEl.className = 'product__list__more'
  moreListEl.innerHTML = /*html*/ `
  <a href="#" class="morebtn">더보기</a>
  `
  productFirstListEl.after(moreListEl)
}

```

![image](https://user-images.githubusercontent.com/56331400/218106856-cfe992dd-d5a7-4e61-b711-03da1e642c0a.png)


## TODO
- 렌더링 메소드의 파라미터에 따라서 원하는 조건의 데이터를 렌더링할수 있게 설정하기
- 더보기 기능 구현
- 메인페이지 탭에 렌더링 목록 구현하기
