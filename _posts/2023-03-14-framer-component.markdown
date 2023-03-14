---
layout: post
title: "Let's make component with Framer code"
categories: UI
tags: Prototype UI
---


# Framer 에서 code를 작성하여 컴포넌트 제작

Framer는 다른 프로토타입 제작 도구와는 다르게 코드를 작성해서 컴포넌트를 만들수 있다. 유튜브를 검색해서 나온 강의를 따라해서 컴포넌트를 제작해봤다.


## Line Item 
<div align=center>
<img src =https://user-images.githubusercontent.com/56331400/225019274-914802db-823f-4c3a-a4c2-df2b7a00910a.gif />
</div>



좌측의 아이콘과 가운데에 이름, 그리고 우측에 가격이 표시되어있는 아이템

```jsx
import * as React from "react"

const containerStyle = {
    padding: 10,
    width: "100%",
    height: 70,
    display: "flex",
    alignItems: "center",
    justifyContent: "space-between",
}
const nameStyle = { flex: 1, marginLeft: 10, fontSize: 16 }
const iconStyle = { width: 48, height: 48, borderRadius: 10 }

export function LineItem({ iconColor, name, price }) {
    const [pounds, pence] = price.toString().split(".")
    return (
        <div style={containerStyle}>
            <div style={% raw %}{{ ...iconStyle, backgroundColor: iconColor }}{% endraw %} />
            <div style={nameStyle}>{name}</div>
            <div>
                $<span style={% raw %}{{ fontSize: 20, fontWeight: 400 }}{% endraw %}>{pounds}</span>
                .
                <span style={% raw %}{{ fontSize: 14, fontWeight: 200 }}{% endraw %}>
                    {pence || "00"}
                </span>
            </div>
        </div>
    )
}

LineItem.defaultProps = {
    name: "Youtube",
    price: "4.23",
    iconColor: "midnightblue",
}

```

## Barchart 
무작위 높위의 bar를 원하는 갯수만큼 렌더링하는 아이템 

![image](https://user-images.githubusercontent.com/56331400/225017663-0c5156f7-69b5-40f1-92c5-be72db49dc99.png)

`Util.ts`

`getBarHeight` : height의 값을 무작위로 지정하기 위해서 호출하면 Math.random()로 생성된 무작위의 숫자에서 100을 곱한 값 + "%"를 리턴하는 메소드


`gatBars(numberOfBars: Number)` : 프로퍼티에 작성된 숫자만큼 bars라는 배열에 getBarHeight()를 호출하여 생성된 데이터를 반복적으로 저장해서 bars를 리턴하는 메소드

```jsx
export function getBarsHeight() {
    return Math.random() * 100 + "%"
}

export function getBars(numberOfBars: Number) {
    const bars = []

    for (let i = 0; i < numberOfBars; i++) {
        bars.push(getBarsHeight())
    }
    return bars
}

```
`getBars`를 import 해서 bars의 길이만큼 반복적으로 bar를 렌더링하는 방식으로 구현했다.
```jsx
import * as React from "react"
import { getBars } from "./Utils.ts"
const containerStyle = {
    width: "100%",
    height: "100%",
    display: "flex",
    alignItems: "flex-end",
    justifyContent: "space-around",
}

export function BarChart({ numberOfBars, thickness, color }) {
    const bars = getBars(numberOfBars)
    const barRadius = thickness / 2
    return (
        <div style={containerStyle}>
            {bars.map((barHeight) => (
                <div
                    style={{
                        width: thickness,
                        height: barHeight,
                        backgroundColor: color,
                        borderRadius: `${barRadius}px ${barRadius}px 0 0 `,
                    }}
                />
            ))}
        </div>
    )
}

BarChart.defaultProps = {
    numberOfBars: 10,
    thickness: 10,
    color: "pink",
}

```

## Transaction
![image](https://user-images.githubusercontent.com/56331400/225019062-6b7b1cc1-fc2b-47bf-b678-cb602233ae21.png)


이전에 만들었던 LineItem을 반복적으로 생성하는 컴포넌트
LineItem을 import 해서 transactionData 만큼 반복적으로 LineItem을 생성한다.

```jsx
import * as React from "react"
import { LineItem } from "./LineItem.tsx"
const transactionData = [
    {
        name: "Youtube",
        price: 4.99,
        iconColor: "#e52d27",
    },
    {
        name: "Netflix",
        price: 5.22,
        iconColor: "#5ba525",
    },
    {
        name: "Evernote",
        price: 4.87,
        iconColor: "#e50914",
    },
    {
        name: "Apple",
        price: 2.99,
        iconColor: "#a3aaae",
    },
]

export function Transactions() {
    return (
        <div>
            {transactionData.map((transaction) => (
                <LineItem
                    name={transaction.name}
                    price={transaction.price}
                    iconColor={transaction.iconColor}
                />
            ))}
        </div>
    )
}

```

