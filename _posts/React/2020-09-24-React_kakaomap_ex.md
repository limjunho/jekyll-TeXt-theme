---
title: React kakaomap 연동(Hook)
tags: React
---

**React에서 kakaomap api를 사용해 지도를 띄우는 예제**

[React_kakaomap](https://github.com/limjunho/React/tree/master/map_ex)  
예제 소스코드(github)

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### kakaomap key발급

**1. [kakaomap](http://apis.map.kakao.com)에 들어가 로그인한 후 애플리케이션 추가**

![그림1](/assets/React/kakaomap/1.PNG)  
앱 이름과 사업자명은 적당히 적어서 애플리케이션 추가

**2. 생성한 애플리케이션을 클릭하여 설정에서 사이트 도메인 등록**

![그림2](/assets/React/kakaomap/2.PNG)  
테스트용이므로 localhost로 등록하였음.

**3. 발급받은 키 확인**

![그림3](/assets/React/kakaomap/3.png)

### kakaomap 사용

```jsx
import React, { useEffect } from "react";

//
const { kakao } = window;

const CreateMap = () => {
  useEffect(() => {
    // 지도를 담을 영역의 DOM 레퍼런스
    const container = document.getElementById("kakaomap");

    // center옵션은 지도를 생성하는데 반드시 필요하며 파라미터는 위경도좌표이다. (위도,경도 순서)
    // level옵션은 지도의 확대, 축소 정도이다.
    const options = {
      center: new kakao.maps.LatLng(33.450701, 126.570667),
      level: 3,
    };

    // 지도 생성 및 객체 리턴
    const map = new kakao.maps.Map(container, options);
  }, []);

  return (
    // 지도를 담을 영역
    <div
      id="kakaomap"
      style={{
        width: "300px",
        height: "300px",
      }}
    ></div>
  );
};

export default CreateMap;
```

**CreateMap.js**  
useEffect를 이용하여 마운트 시 지도 출력

**what is [useEffect](https://limjunho.github.io/2020/09/21/useEffect-Example.html)?**

<br />

```jsx
<script
  type="text/javascript"
  src="//dapi.kakao.com/v2/maps/sdk.js?appkey=JavaScript 키"
></script>
```

**index.html에 추가**  
API를 불러오는 코드

### 실행결과

![그림4](/assets/React/kakaomap/4.PNG)
