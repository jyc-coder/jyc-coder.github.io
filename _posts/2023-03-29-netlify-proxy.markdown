---
layout: post
title: "Dust service netlify"
categories: netlify
teags: netlify proxy
---


# 프록시 설정을 해서 api를 호출하는 경우, netlify에서는 어떻게 설정해야할까?

프록시 설정 때문에 배포를 해도 오류가 발생해서 프록시를 지우고 했었는데, 마냥 그럴수만은 없을때가 있을테니 찾아봤다.
root 경로에다가 netlify.toml을 생성해서 설정 코드를 작성한다.

```jsx
[[redirects]]
from = "/api/*"
  to = "https://apis.data.go.kr/:splat"   
  status = 200
  force = true
```

이렇게 작성하면 netlify에서 proxy를 인식하고, api url의 경로를 읽을수 있게 된다.
from에 해당하는 값은 원하는 것으로 작성하되 api를 호출하는 url에서 사용하는 프록시 변수와 일치해얗나다.
:splat 의 의미는 *와 같은 의미로 전체를 뜻한다.
