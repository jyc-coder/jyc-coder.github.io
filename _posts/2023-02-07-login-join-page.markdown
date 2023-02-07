---
layout: post
title: "회원 가입 페이지 닉네임 입력 추가"
categories: TeamProject
teags: javascript html css
---

# 회원가입 페이지에서 닉네임 입력 항목 추가

거래 API의 회원가입 기능에서 필수적으로 입력해야하는 항목중에 닉네임이 있기때문에 닉네임 입력 공간을 추가함
`js/join.js`
```
<div class="login__input_box">
            <h3>닉네임</h3>
            <div class="login__input__item">
              <input
                class="login__input__displayname"
                type="text"
                autocomplete="off"
              />
            </div>
            <p class="displayname-error">
              닉네임은 3~10자로 입력해주세요. 특수문자는 허용되지 않습니다.
            </p>
          </div>
          <div class="join__btn">
            <a class="join__btn__link disable" href="#" >회원가입</a>
          </div>
           <div class="join__btn">
            <a class="join__btn__link join" href="/">메인 화면</a>
          </div>
        </div>
      </div>
```

닉네임 유효성 검사는 특수문자를 제외한 3~10자로 정규표현식을 통해 설정했다.
```js
const displayNameValidation = new RegExp(
    '^[A-Za-z0-9ㄱ-ㅎ|ㅏ-ㅣ|가-힣]{3,10}$'
  )
```

입력한 항목들의 유효성 검사가 통과 된경우에만 회원가입 버튼을 활성화시키게 설정했다
```js
document
    .querySelector('.login__input__displayname')
    .addEventListener('input', (e) => {
      !displayNameValidation.test(e.target.value)
        ? [
            (displayNameErrorText.style.display = 'block'),
            (displayNameErrorText.dataset.validate = 'false'),
          ]
        : [
            (displayNameErrorText.style.display = 'none'),
            (displayNameErrorText.dataset.validate = 'true'),
          ]

      if (
        idErrorText.dataset.validate === 'true' &&
        pwErrorText.dataset.validate === 'true' &&
        pwCheckErrorText.dataset.validate === 'true' &&
        displayNameErrorText.dataset.validate === 'true'
      ) {
        joinBtnEl.classList.remove('disable')
      } else {
        joinBtnEl.classList.add('disable')
      }
    })
```


# api 호출 코드 추가

## 회원가입 
아이디(이메일) 비밀번호, 닉네임을 파라미터로 전달하면 회원가입이 진행되고 accesstoken과 함께 입력했던 정보를 반환한다.
`js/request`
```js

export async function createUser(id, pw, displayname) {
  const res = await fetch(
    'https://asia-northeast3-heropy-api.cloudfunctions.net/api/auth/signup',
    {
      method: 'POST',
      headers,
      body: JSON.stringify({
        email: id,
        password: pw,
        displayName: displayname,
      }),
    }
  )
  return res.json()
}
```

## 로그인
아이디와 비밀번호를 전달하면 엑세스 토큰을 반환한다.
`js/request`
```js
export async function login(id, pw) {
  const res = await fetch(
    'https://asia-northeast3-heropy-api.cloudfunctions.net/api/auth/login',
    {
      method: 'POST',
      headers,
      body: JSON.stringify({
        email: id,
        password: pw,
      }),
    }
  )
  return res.json()
}

```

## 기능 오류

다른 API기능들은 다른 곳에서 테스트를 진행하고 있는데 로그아웃, 인증 기능 테스트에서 오류가 발생하는데, 회원가입으로 생긴 토큰값이 24시간 뒤에 만료된다고 해서 24시간동안은 
고정값인줄 알앗는데, 로그인을 하면 엑세스토큰의 값이 변경되기 때문에 로그아웃과 인증 기능이 통과되지 않았다.
엑세스 토큰이 최신화되는 것을 파악하지 못했기 때문에 벌어진 실수


## 내일 할일
- [ ] 로그인 후 토큰을 스토리지에 넣고 메인 페이지로 이동
- [ ] 토큰의 값을 24시간 제한시간을 두고 데이터 사용기한을 두게 설정
- [ ] 상품 표시 엘리먼트 구현및 렌더링 구현

