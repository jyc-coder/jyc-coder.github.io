---
layout: post
title: "advancement search filter, skeleton UI"
categories: TeamProject
teags: javascript html css
---


# 고도화 작업1 스크롤을 따라오는 검색필터

이 작업은 그렇게 어렵지는 않았다. 처음에는 플러그인을 설치해서 작업하려고 했으나, 검색 필터 하나에 플러그인을 설치하는 것이 효율적이지는 않다고 판단했다.
따라서 스타일 속성 position:sticker를 사용해서 스크롤을 내릴때 필터가 따라오게 구현했다.

![GOMCAM 20230227_2152430606](https://user-images.githubusercontent.com/56331400/221581083-9b27b65b-f7d6-47d1-9fa1-8a04305dadef.gif)


# 고도화 작업2 스켈레톤 UI 구현

- 메인페이지 , 상점페이지에 제품 목록들을 렌더링하는동안 스켈레톤 ui를 렌더링하게 설정함
- 메인페이지, 상점페이지 구현 메소드내부의 컨테이너의 innerHTML에 loading이라는 dom을 미리 추가하여 로드 되자마자 UI가 보이게 설정
`js/products.js`의 appendProducts, appendSmallProducts 

```js
 productEl.innerHTML = /*html */ `
     <div class="product__title">
        <div>
          <div class="title">Just Dropped</div>
          <div class="sub_title">발매 상품</div>
        </div>
      </div>
      <div class="product__list">
       <div class="loading">
        <div class="loading__item ">
          <div class="loading__item__image blink_me"></div>
        <div class="loading__item__text blink_me"></div>
      </div>
       <div class="loading__item ">
          <div class="loading__item__image blink_me"></div>
        <div class="loading__item__text blink_me"></div>
      </div>
       <div class="loading__item ">
          <div class="loading__item__image blink_me"></div>
        <div class="loading__item__text blink_me"></div>
      </div>
       <div class="loading__item ">
          <div class="loading__item__image blink_me"></div>
        <div class="loading__item__text blink_me"></div>
      </div>
       </div>
        <div class="product__list__first none">
        </div>
      </div>
    `
```
```js
productEl.innerHTML = /*html */ `
     <div class="product__title">
        <div>
          <div class="title">Just Dropped</div>
          <div class="sub_title">발매 상품</div>
        </div>
      </div>
      <div class="product__list">
        <div class="loading">
        <div class="loading__item ">
          <div class="loading__item__image--sm blink_me"></div>
        <div class="loading__item__text--sm blink_me"></div>
      </div>
        <div class="loading__item ">
          <div class="loading__item__image--sm blink_me"></div>
        <div class="loading__item__text--sm blink_me"></div>
      </div>
        <div class="loading__item ">
          <div class="loading__item__image--sm blink_me"></div>
        <div class="loading__item__text--sm blink_me"></div>
      </div>
        <div class="loading__item ">
          <div class="loading__item__image--sm blink_me"></div>
        <div class="loading__item__text--sm blink_me"></div>
      </div>
       <div class="loading__item ">
          <div class="loading__item__image--sm blink_me"></div>
        <div class="loading__item__text--sm blink_me"></div>
      </div>
      <div class="loading__item ">
          <div class="loading__item__image--sm blink_me"></div>
        <div class="loading__item__text--sm blink_me"></div>
      </div>
      <div class="loading__item ">
          <div class="loading__item__image--sm blink_me"></div>
        <div class="loading__item__text--sm blink_me"></div>
      </div>
      <div class="loading__item ">
          <div class="loading__item__image--sm blink_me"></div>
        <div class="loading__item__text--sm blink_me"></div>
      </div>
       </div>
        <div class="product__list__first none">
    </div>
      </div>
    `
```
- 스켈레톤 ui를 보여주는 동안 제품 목록 엘리먼트를 담고 있는 product__list__first 에 none 클래스를 미리 추가해서 보이지 않게 설정함
- 아이템을 append 하는 `appendItem` `appendSmallItem` 내부에 동작 완료 유무를 파악하기 위해서 `isdone`이라는 boolean 변수를 추가함
- innerHTML 설정, container에 append, 기능 추가 까지 모두 완료한 다음 isdone을 true로 변경하며, isdone이 true가 되었을때, loading 엘리먼트를 제거함과 시에  product__list__first 에 none 클래스를 제거한다.
`js/appendItem, appendSmallItem`
```
isdone = true
  const loading = document.querySelector('.loading')
  //productListFirstEl에서 근처 loading div 찾기
  if (isdone) {
    productListFirstEl.classList.remove('none')
    if (loading) {
      loading.remove()
    }
  }
```
- 깜빡이는 애니메이션은 blinker애니메이션으로 products.scss에 작성했으며 opacity의 값을 변동하여 깜빡이는 듯한 효과를 표현했음
```css
@keyframes blinker {
  50% {
    opacity: 0;
  }
}
```


![GOMCAM 20230227_2150560831](https://user-images.githubusercontent.com/56331400/221586555-7f812b75-e05f-4df6-a804-af0adb17da14.gif)
![GOMCAM 20230227_2151240854-min](https://user-images.githubusercontent.com/56331400/221587120-00b3d8cb-7d0d-483c-8270-7311b6c821c5.gif)

