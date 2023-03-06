---
layout: post
title: "Defeating Netlify Deploy Trouble"
categories: TeamProject
teags: netlify
---

# netlify deploy is not working...

- 팀프로젝트를 마무리하고 이제 netlify를 사용해서 배포를 진행하려고 했으나 오류가 발생함

![스크린샷 2023-03-03 160316](https://user-images.githubusercontent.com/56331400/222944042-d3e2a7e7-923f-45e5-ba66-2631f93d60ec.png)


## 해결 방법 : package.json에 해당 패키지 설치
- 말그대로 package.json에 lightning 패키지가 추가되어있지 않았다. 따라서 그냥 설치를 진행하고 다시 배포를 진행했더니 성공했다.


# 배포는 됐는데... 왜 메인페이지만 나와..?

- 상점 페이지, 마이페이지를 들어갔는데 404 에러가 나오고 아무런 화면이 나오지 않는다..?

## 해결 방법 : _redirects 설정 파일을 추가함

- netlify에서 deploy를 진행할때 index.html과 다른 전체 링크에 대해서 상태 값을 200으로 설정하는 파일을 추가했다. 
순서
1. parcel-reporter-static-files-copy 패키지 설치
2. 루트 경로에 parcelrc파일을 생성하여 해당 내용을 추가한다.
```json
{
  "extends": [
    "@parcel/config-default"
  ],
  "reporters": [
    "...",
    "parcel-reporter-static-files-copy"
  ]
}

```
3. build 하면서 추가될 파일을 생성한다. 내 경우는 static 폴더 안에 .env와 _redirects를 생성했다.
4. package.json에 "staticFiles" 항목을 추가한다. 이렇게 추가하고 빌드를 실행하면 빌드 폴더 내부에 static 폴더 내부 파일이 들어있을 것이다.
5. netlify 설정에서 환경변수를 추가해준다.

```json
"staticFiles": {
    "staticPath": "static"
  },
```
![image](https://user-images.githubusercontent.com/56331400/222946080-8509944c-c49d-4921-9a46-d93156d6d732.png)

![ezgif com-video-to-gif (11)](https://user-images.githubusercontent.com/56331400/222948467-2f32751b-9970-484c-9cd7-176edc2b4025.gif)

제대로 동작되는 것을 확인했다.

# percel 경고 : Mutating process.env is not supported 

![image](https://user-images.githubusercontent.com/56331400/222949884-7dcd19fe-5c49-46ed-bfcd-21aa13e3199a.png)

## 해결 방법 : dotenv 패키지 제거후 dev, 및 빌드 실행

mutation : 새로운 변수를 생성하거나 기존 변수를 재할당하지 않고 JavaScript의 객체 또는 배열을 변경할때 일어나는 현상

여기서부터는 작성자의 추론이다. 

-  parcel 측에서 mutating 관련 warning을 날렸으므로 이전에 이미 생성된 객체가 존재한다는 뜻이 되며, 이는 개발자가 루트 경로에 생성한 .env 파일이다.
-  dotenv에서 추가적으로 parsed[key]를 process.env[key]에 대입하는 작업에서 재할당을 진행하지 않고 객체의 값을 변경하려는 작업을 수행했기 때문에 발생한 경고로 추정됨

관련 경고 발생에 대한 검색을 진행했지만 저와 같은 현상이 있는 사람만 있고 이렇다 할 해답이 없어서 조사한 정보를 통해 추론했다.
dotenv 제거후 deploy된 사이트를 테스트에서 오류는 발생하지 않았습니다. 

