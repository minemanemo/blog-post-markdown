# 🤔 본문에 앞서

최근 토스 컨퍼런스에서 [_박서진_ 님의 "프론트엔드 웹 서비스에서 우아하게 비동기 처리하기"](https://www.youtube.com/watch?v=FvRtoViujGg&t=886s) 세션을 관심 있게 보았다.

Facebook에서 직접 만들었다는 Recoil에 관심은 있었지만 여느 블로그를 검색해도 나오는

막상 누구나 다해보는 Practice 정도만 해본 상태였다.

실 서비스에서 사용해보지 않아 부족했었으나 최근에 회사에서 개발중인 서비스에 Recoil을 점진적으로 도입하기로 하였고

약 1주일 정도 이리 저리 만져보면서 적용한 내용을 공유해보려고 한다.

# 😵‍💫 Google Trend로 보는 Redux vs MobX vs Recoil

- 재미로 Google Trend로 검색해보았다.
- 한국에서는 Redux, 미국에서는 Recoil의 검색량이 압도적으로 많다.

<script type="text/javascript" src="https://ssl.gstatic.com/trends_nrtr/2578_RC01/embed_loader.js"></script>

**대한민국**

<script type="text/javascript">
trends.embed.renderExploreWidget(
  "TIMESERIES",
  {
    "comparisonItem":[
      {"keyword":"redux","geo":"KR","time":"today 12-m"},
      {"keyword":"mobx","geo":"KR","time":"today 12-m"},
      {"keyword":"recoil","geo":"KR","time":"today 12-m"}
    ],
    "category":0,
    "property":""
  },
  {
    "exploreQuery":"geo=KR&q=redux,mobx,recoil&date=today 12-m,today 12-m,today 12-m",
    "guestPath":"https://trends.google.com:443/trends/embed/"
  }
);
</script>

---

**미국**

<script type="text/javascript">
trends.embed.renderExploreWidget(
  "TIMESERIES",
  {
    "comparisonItem":[
      {"keyword":"redux","geo":"US","time":"today 12-m"},
      {"keyword":"mobx","geo":"US","time":"today 12-m"},
      {"keyword":"recoil","geo":"US","time":"today 12-m"}
    ],
    "category":0,
    "property":""
  },
  {
    "exploreQuery":"geo=US&q=redux,mobx,recoil&date=today 12-m,today 12-m,today 12-m",
    "guestPath":"https://trends.google.com:443/trends/embed/"
  }
);
</script>

---
