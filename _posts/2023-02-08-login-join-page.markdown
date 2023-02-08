---
layout: post
title: "login page, logout"
categories: TeamProject
teags: javascript html css
---

# 로그인, 회원가입 페이지 기능 구현 완료!
로그인과 회원가입을 시도했을때, login(), createUser() 메소드를 호출해서 토큰값이 있다면 로그인, 회원가입 완료를 알리고 메인 페이지로 이동한다.
- 회원가입을 하는 순간 로그인까지 완료되며, 이에 대한 토큰이 생성됨
- 로그인, 회원가입을 완료할때 사용자의 정보가 로컬스토리지에 저장된다! 로그인 - email, displayName, token  회원가입 - token,email,displayName

`js/login`
```js
 if (
        idErrorText.dataset.validate === 'true' &&
        pwErrorText.dataset.validate === 'true'
      ) {
        const result = await login(idInput.value, pwInput.value)
        if (result.accessToken) {
          alert(`로그인 완료! 환영합니다`)
          localStorage.setItem('email', result.user.email)
          localStorage.setItem('displayName', result.user.displayName)
          localStorage.setItem('token', result.accessToken)
          location.replace('/')
        } else {
          window.alert(`${result}`)
          return
        }
      } else {
        window.alert('회원 정보를 입력하세요!')
        return
      }
```

`js/join`
```
if (
        idErrorText.dataset.validate === 'true' &&
        pwErrorText.dataset.validate === 'true' &&
        pwCheckErrorText.dataset.validate === 'true' &&
        displayNameErrorText.dataset.validate === 'true'
      ) {
        const userData = await createUser(
          idInput.value,
          pwInput.value,
          displayNameInput.value
        )
        if (userData.acessToken) {
          window.alert('가입이 완료되었습니다!!')
          localStorage.setItem('token', userData.accessToken)
          localStorage.setItem('email', userData.user.email)
          localStorage.setItem('displayName', userData.user.displayName)
          location.replace('/')
        } else {
          window.alert(`${userData}`)
          return
        }
      } else {
        window.alert('회원 정보를 입력하세요!')
        return
      }
```
# 로그아웃 기능 구현 
현재 헤더에 로그인 관련 엘리먼트를 구현해야하는데 헤더를 구현하지 않아서 일단 index.js 내부에 테스트용으로 버튼만 만들어놨음
- 로그아웃 버튼을 클릭하면 logout 메소드가 호출되어서 api상에서 로그아웃 처리를 완료하고, 로컬 스토리지에 남아있는 토큰,이메일,비밀번호,유저이름을 제거함
- 로그인 되었는지의 여부를 파악하기 위해서 로그인 한 다음에는 메인 화면에 `유저이름`님 환영합니다 라는 텍스트가 표시되게 구현함
`js/index.js`
```js
const logoutBtnEl = document.querySelector('.logout')
const token = localStorage.getItem('token')
logoutBtnEl.addEventListener('click', () => {
  logout(token)
  window.alert('로그아웃 완료!')
  localStorage.removeItem('token')
  localStorage.removeItem('email')
  localStorage.removeItem('password')
  localStorage.removeItem('displayName')
  location.replace('/login')
})
// 사용자 이름 표시
const displayName = document.querySelector('.dpname')
if (localStorage.getItem('displayName')) {
  displayName.textContent = `${localStorage.getItem(
    'displayName'
  )}님 환영합니다!!!`
}
```
![image](https://user-images.githubusercontent.com/56331400/217476529-56b561f5-05b0-4256-8248-be8f81333f83.png)

