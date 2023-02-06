---
layout: post
title: "login page"
categories: TeamProject
teags: javascript html css
---

# 팀프로젝트 시작 

강사님이 제작한 거래 api를 사용해서 팀프로젝트를 구현하기!
전반적인 디자인과 기능은 kream 쇼핑 사이트를 기반으로 제작할 것!
kream : https://kream.co.kr/
사다리타기로 조장이 되어서 조원들의 역할분담 진행했고 그전에 클론하기로한 쇼핑 사이트의 구성을 ppt로 작성해서 발표 진행함
[4차 과제 프로젝트.pdf](https://github.com/jyc-coder/jyc-coder.github.io/files/10659927/4.pdf)


## 페이지 구현 방식

dom 을 생성하여 렌더링 하는 메소드를 작성해서 index.js로 import 하여 컴포넌트를 조립하는 방식으로 접근한다.


## 현재 내 역할
로그인 페이지, 회원가입 페이지 기능 구현 , 물건 렌더링 페이지 


## 로그인 페이지 

`js/login.js`
```js
import Navigo from 'navigo'
import appendJoin from './join'

export default function appendLogin() {
  const loginEl = document.createElement('div')
  loginEl.className = 'login'

  loginEl.innerHTML = /*html */ `
      <div class="content lg">
        <div class="login__area">
          <h2 class="login__title"></h2>
          <div class="login__input_box">
            <h3>이메일 주소</h3>
            <div class="login__input__item">
              <input
                class="login__input__id"
                type="email"
                placeholder="예)korea@korea.co.kr"
                autocomplete="off"
              />
            </div>
            <p class="id-error">이메일 주소를 정확히 입력해주세요</p>
          </div>
          <div class="login__input_box">
            <h3>비밀번호</h3>
            <div class="login__input__item">
              <input
                class="login__input__pw"
                type="password"
                placeholder="예)korea@krea.co.kr"
                autocomplete="off"
              />
            </div>
            <p class="pw-error">
              영문, 숫자, 특수문자를 조합해서 입력해주세요. (8-16자)
            </p>
          </div>
          <div class="login__btn">
            <a class="login__btn__link disable" href="#" >로그인</a>
          </div>
          <div class="join__btn">
            <a class="join__btn__link join" href="/join">회원 가입</a>
          </div>
           <div class="join__btn">
            <a class="join__btn__link join" href="/">메인 화면</a>
          </div>
        </div>
      </div>
  `
  document.body.append(loginEl)

  const idErrorText = document.querySelector('.id-error')
  const pwErrorText = document.querySelector('.pw-error')
  const loginBtnEl = document.querySelector('.login__btn__link')
  // 유효성 검사 ID, password
  const idValidation = new RegExp('^[a-z0-9]+@[a-z]+.[a-z]{2,3}$')
  const pwValidation = new RegExp(
    '^(?=.*?[a-z])(?=.*?[0-9])(?=.*?[#?!@$%^&*-]).{8,16}$'
  )
  document.querySelector('.login__input__id').addEventListener('input', (e) => {
    !idValidation.test(e.target.value)
      ? [
          (idErrorText.style.display = 'block'),
          (idErrorText.dataset.validate = 'false'),
        ]
      : [
          (idErrorText.style.display = 'none'),
          (idErrorText.dataset.validate = 'true'),
        ]
  })

  document.querySelector('.login__input__pw').addEventListener('input', (e) => {
    !pwValidation.test(e.target.value)
      ? [
          (pwErrorText.style.display = 'block'),
          (pwErrorText.dataset.validate = 'false'),
        ]
      : [
          (pwErrorText.style.display = 'none'),
          (pwErrorText.dataset.validate = 'true'),
        ]

    if (
      idErrorText.dataset.validate === 'true' &&
      pwErrorText.dataset.validate === 'true'
    ) {
      loginBtnEl.classList.remove('disable')
    } else {
      loginBtnEl.classList.add('disable')
    }
  })

  document.querySelector('.login__btn__link').addEventListener('click', () => {
    if (
      idErrorText.dataset.validate === 'true' &&
      pwErrorText.dataset.validate === 'true'
    ) {
      console.log('로그인 시작')
    } else {
      window.alert('로그인 정보를 입력하세요!')
      return
    }
  })

  // 회원가입 페이지로 넘어가기
  const router = new Navigo('/')
  router
    .on('/join', function () {
      appendJoin()
    })
    .resolve()
}

```

- 먼저 레이아웃 html dom 을 innerHTML을 사용해서 생성후 append를 호출하여 body에 추가한다
- 그 다음 각각의 엘리먼트를 선택하여 기능을 부여
- id와 pw를 입력하는데 조건을 만족하지 않으면 로그인 버튼을 활성화하지 않게 구현했다. id는 이메일 방식으로 작성하고, pw는 영어,숫자,특수문자 조합으로 8~16자 사이의 길이여야한다.
- 현재 로그인 기능 구현은 만들지 않음

## 회원가입 페이지

`js/join.js`
```js
export default function appendJoin() {
  const loginEl = document.createElement('div')
  loginEl.className = 'login'
  loginEl.innerHTML = `
       <div class="content lg">
        <div class="login__area">
          <h2 class="login__title"></h2>
          <div class="login__input_box">
            <h3>이메일 주소*</h3>
            <div class="login__input__item">
              <input
                class="login__input__text"
                type="email"
                placeholder="예)korea@korea.co.kr"
                autocomplete="off"
              />
            </div>
          </div>
          <div class="login__input_box">
            <h3>비밀번호*</h3>
            <div class="login__input__item">
              <input
                class="login__input__text"
                type="email"
                placeholder="예)korea@krea.co.kr"
                autocomplete="off"
              />
            </div>
          </div>
          <div class="login__input_box">
            <h3>비밀번호 확인*</h3>
            <div class="login__input__item">
              <input
                class="login__input__text"
                type="email"
                placeholder="예)korea@krea.co.kr"
                autocomplete="off"
              />
            </div>
          </div>
          <div class="login__btn">
            <a class="login__btn__link disable" href="#">가입하기</a>
          </div>
          <div class="cancel__btn">
            <a class="cancel__btn__link " href="/login">취소</a>
          </div>
        </div>
      </div>
        `

  document.body.append(loginEl)

  document.querySelector('.login__btn').addEventListener('click', () => {
    window.alert('로그인 검사!')
  })
}

```

- 현재 레이아웃만 만들어놓고 기능 구현은 하지 않음!
