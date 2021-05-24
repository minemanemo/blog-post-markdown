# 🥸 Recoil은 왜 개발 했을까?

- 호환성 & 단순함 측면을 봤을때는 다른 상태 관리 라이브러리 보다 React 자체에서 제공해주는 상태 관리 기능을 사용하는 것이 좋습니다. (`useState` 라던지...)
- 하지만 React도 한계가 있습니다. (미친듯이 거대한 상태 트리라던가... 미친 랜더링을 야기할 수 있다... 무서웡...)
- 그럼 어떤 장점이 있을까?
  - Native하다! (동시성 모드 같은 새로운 React 기능들과 호환 가능!... 내가 동시성 모드를 잘모른다는건 함정;;)
  - 사용하는데 간편하다 (hooks 사용자라면 더더욱!)

# ⚙️ 설치/세팅 방법

## 1. 설치

`잘쓰는게 어렵지! 설치 방법은 쉽다!` 🥲

당연히 프로젝트 Root 디렉토리로 이동한 후 아래 명령어를 실행해준다
Recoil은 계속 개발 중입니다! `nightly` 브랜치에 하루 단위로 배포 중이라고 하니 최신 버전으로 받을 수도 있습니다~

> npm을 쓴다면?

```bash
# Release 버전 설치
npm install recoil

# nightly(최신버전) 설치
npm install https://github.com/facebookexperimental/Recoil.git#nightly
```

> yarn을 쓴다면?

```bash
# Release 버전 설치
yarn add recoil

# nightly(최신버전) 설치
yarn add https://github.com/facebookexperimental/Recoil.git#nightly
```

## 2. ESLint 세팅

- `eslint-plugin-react-hooks`을 사용하는 경우에 아래와 같이 수정합니다.
- `useRecoilCallback`을 사용할때 해결 방법을 eslint를 통하여 제시해줍니다.

> 변경 전

```json
{
  "plugins": ["react-hooks"],
  "rules": {
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn"
  }
}
```

> 변경 후

```json
{
  "plugins": ["react-hooks"],
  "rules": {
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": [
      "warn",
      {
        "additionalHooks": "useRecoilCallback"
      }
    ]
  }
}
```

# 🤖 사용 방법

## 1. RecoilRoot 컴포넌트 선언

```javascript
import * as React from "react";
import { RecoilRoot } from "recoil";

function App() {
  return (
    <RecoilRoot>
      <div>Children...</div>
    </RecoilRoot>
  );
}
```

- 사용하기 쉽게 HoC로 이뤄져있기 때문에 Redux에서 Provider를 만들어 주는 것과 같이 `RecoilRoot`를 적적한 위치에 추가해 주면 된다.
- Recoil 공식 문서에서는 Root 컨포넌트가 `RecoilRoot` 넣기에 가장 좋은 장소라고 말하고 있다. ([바로가기](https://recoiljs.org/ko/docs/introduction/getting-started#recoilroot))
- 무조건 `RecoilRoot`라는 Store를 하나만 사용할 수 있다는 것은 아니다!
- 소스를 보면 Store가 계층 구조를 가질 수 있으며 override 옵션을 통해서 상태를 상속 받을 수도있다. ([바로가기](https://github.com/facebookexperimental/Recoil/blob/master/src/core/Recoil_RecoilRoot.react.js#L482))
- 초기화도 가능하다!

## 2-1. Atom

`컴포넌트끼리 공유 가능한 가장 작은 단위의 State, atom!!!`

**[ atom으로 상태 정의 기본 Code ]**

```typescript
import { atom } from "recoil";

const postingId = atom<string>({
  key: "posting/id",
  default: "",
});
```

- Recoil로 상태를 구성하는 제일 작은 단위입니다.
- `useState`로 만든 상태와 개념적으로 비슷하지만 **☆다른 컴포넌트와 상태를 공유☆**할 수 있다는 점이 다릅니다.
- typescript에서 `atom` 키워드 뒤에 오는 제네릭 타입은 atom의 값의 타입을 의미한다.
- `key`는 atom마다 고유하게 만들어 줘야한다. (key 생성 규칙 정도 정하는게 좋겠죠?!)
- `default`는 기본 상태 값을 말합니다.
- `effects_UNSTABLE` 옵션은 추후에 다루겠습니다.
- atom으로 만든 상태를 읽는 모든 컴포넌트는 atom의 상태가 변경되면 rerendering이 된다. (구독 개념과 같다)

## 2-2. Selector

`atom 또는 다른 selector로 구성한 순수함수`

**[ 기본 선언 코드 ]**

```typescript
import { atom, selector } from "recoil";

const givenName = atom<string>({
  key: "givenName",
  default: "",
});
const familyName = atom<string>({
  key: "familyName",
  default: "",
});

const displayName = selector<string>({
  key: "displayName",
  get: ({ get }) => {
    const givenName = get(givenName);
    const familyName = get(familyName);
    return `${familyName} ${givenName}`;
  },
});
```

- `atom` 또는 `selector를` 기반으로 새롭게 결과를 구성해주는 순수함수 입니다.
  - 순수함수란?! : 하나 이상의 인자를 받고 그 인자만으로 결과물을 만들어 내는 함수
- 구독중인 `atom` 또는 `selector가` 업데이트 되면 selector도 업데이트 됩니다.
- `get`을 이용하여 다른 `atom`을 구독 할 수 있습니다.
- 컴포넌트 입장에서는 atom, selector 둘다 동일한 인터페이스를 가지기 때문에 서로 대체가 가능합니다.
- `set`을 이용하여 구독하는 상태의 값들을 업데이트 할 수 있습니다. (추후에 다른 글에서 자세히 다루겠습니다.)

---

**Reference**

https://recoiljs.org/ko/docs/introduction/motivation
https://recoiljs.org/ko/docs/introduction/core-concepts
https://recoiljs.org/ko/docs/introduction/installation
https://recoiljs.org/ko/docs/introduction/getting-started
https://github.com/facebookexperimental/Recoil
