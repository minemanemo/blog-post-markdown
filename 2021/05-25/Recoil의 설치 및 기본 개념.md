# ๐ฅธ Recoil์ ์ ๊ฐ๋ฐ ํ์๊น?

- ํธํ์ฑ & ๋จ์ํจ ์ธก๋ฉด์ ๋ดค์๋๋ ๋ค๋ฅธ ์ํ ๊ด๋ฆฌ ๋ผ์ด๋ธ๋ฌ๋ฆฌ ๋ณด๋ค React ์์ฒด์์ ์ ๊ณตํด์ฃผ๋ ์ํ ๊ด๋ฆฌ ๊ธฐ๋ฅ์ ์ฌ์ฉํ๋ ๊ฒ์ด ์ข์ต๋๋ค. (`useState` ๋ผ๋์ง...)
- ํ์ง๋ง React๋ ํ๊ณ๊ฐ ์์ต๋๋ค. (๋ฏธ์น๋ฏ์ด ๊ฑฐ๋ํ ์ํ ํธ๋ฆฌ๋ผ๋๊ฐ... ๋ฏธ์น ๋๋๋ง์ ์ผ๊ธฐํ  ์ ์๋ค... ๋ฌด์์ก...)
- ๊ทธ๋ผ ์ด๋ค ์ฅ์ ์ด ์์๊น?
  - Nativeํ๋ค! (๋์์ฑ ๋ชจ๋ ๊ฐ์ ์๋ก์ด React ๊ธฐ๋ฅ๋ค๊ณผ ํธํ ๊ฐ๋ฅ!... ๋ด๊ฐ ๋์์ฑ ๋ชจ๋๋ฅผ ์๋ชจ๋ฅธ๋ค๋๊ฑด ํจ์ ;;)
  - ์ฌ์ฉํ๋๋ฐ ๊ฐํธํ๋ค (hooks ์ฌ์ฉ์๋ผ๋ฉด ๋๋์ฑ!)

# โ๏ธ ์ค์น/์ธํ ๋ฐฉ๋ฒ

## 1. ์ค์น

`์์ฐ๋๊ฒ ์ด๋ ต์ง! ์ค์น ๋ฐฉ๋ฒ์ ์ฝ๋ค!` ๐ฅฒ

๋น์ฐํ ํ๋ก์ ํธ Root ๋๋ ํ ๋ฆฌ๋ก ์ด๋ํ ํ ์๋ ๋ช๋ น์ด๋ฅผ ์คํํด์ค๋ค
Recoil์ ๊ณ์ ๊ฐ๋ฐ ์ค์๋๋ค! `nightly` ๋ธ๋์น์ ํ๋ฃจ ๋จ์๋ก ๋ฐฐํฌ ์ค์ด๋ผ๊ณ  ํ๋ ์ต์  ๋ฒ์ ์ผ๋ก ๋ฐ์ ์๋ ์์ต๋๋ค~

> npm์ ์ด๋ค๋ฉด?

```bash
# Release ๋ฒ์  ์ค์น
npm install recoil

# nightly(์ต์ ๋ฒ์ ) ์ค์น
npm install https://github.com/facebookexperimental/Recoil.git#nightly
```

> yarn์ ์ด๋ค๋ฉด?

```bash
# Release ๋ฒ์  ์ค์น
yarn add recoil

# nightly(์ต์ ๋ฒ์ ) ์ค์น
yarn add https://github.com/facebookexperimental/Recoil.git#nightly
```

## 2. ESLint ์ธํ

- `eslint-plugin-react-hooks`์ ์ฌ์ฉํ๋ ๊ฒฝ์ฐ์ ์๋์ ๊ฐ์ด ์์ ํฉ๋๋ค.
- `useRecoilCallback`์ ์ฌ์ฉํ ๋ ํด๊ฒฐ ๋ฐฉ๋ฒ์ eslint๋ฅผ ํตํ์ฌ ์ ์ํด์ค๋๋ค.

> ๋ณ๊ฒฝ ์ 

```json
{
  "plugins": ["react-hooks"],
  "rules": {
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn"
  }
}
```

> ๋ณ๊ฒฝ ํ

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

# ๐ค ์ฌ์ฉ ๋ฐฉ๋ฒ

## 1. RecoilRoot ์ปดํฌ๋ํธ ์ ์ธ

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

- ์ฌ์ฉํ๊ธฐ ์ฝ๊ฒ HoC๋ก ์ด๋ค์ ธ์๊ธฐ ๋๋ฌธ์ Redux์์ Provider๋ฅผ ๋ง๋ค์ด ์ฃผ๋ ๊ฒ๊ณผ ๊ฐ์ด `RecoilRoot`๋ฅผ ์ ์ ํ ์์น์ ์ถ๊ฐํด ์ฃผ๋ฉด ๋๋ค.
- Recoil ๊ณต์ ๋ฌธ์์์๋ Root ์ปจํฌ๋ํธ๊ฐ `RecoilRoot` ๋ฃ๊ธฐ์ ๊ฐ์ฅ ์ข์ ์ฅ์๋ผ๊ณ  ๋งํ๊ณ  ์๋ค. ([๋ฐ๋ก๊ฐ๊ธฐ](https://recoiljs.org/ko/docs/introduction/getting-started#recoilroot))
- ๋ฌด์กฐ๊ฑด `RecoilRoot`๋ผ๋ Store๋ฅผ ํ๋๋ง ์ฌ์ฉํ  ์ ์๋ค๋ ๊ฒ์ ์๋๋ค!
- ์์ค๋ฅผ ๋ณด๋ฉด Store๊ฐ ๊ณ์ธต ๊ตฌ์กฐ๋ฅผ ๊ฐ์ง ์ ์์ผ๋ฉฐ override ์ต์์ ํตํด์ ์ํ๋ฅผ ์์ ๋ฐ์ ์๋์๋ค. ([๋ฐ๋ก๊ฐ๊ธฐ](https://github.com/facebookexperimental/Recoil/blob/master/src/core/Recoil_RecoilRoot.react.js#L482))
- ์ด๊ธฐํ๋ ๊ฐ๋ฅํ๋ค!

## 2-1. Atom

`์ปดํฌ๋ํธ๋ผ๋ฆฌ ๊ณต์  ๊ฐ๋ฅํ ๊ฐ์ฅ ์์ ๋จ์์ State, atom!!!`

**[ atom์ผ๋ก ์ํ ์ ์ ๊ธฐ๋ณธ Code ]**

```typescript
import { atom } from "recoil";

const postingId = atom<string>({
  key: "posting/id",
  default: "",
});
```

- Recoil๋ก ์ํ๋ฅผ ๊ตฌ์ฑํ๋ ์ ์ผ ์์ ๋จ์์๋๋ค.
- `useState`๋ก ๋ง๋  ์ํ์ ๊ฐ๋์ ์ผ๋ก ๋น์ทํ์ง๋ง **โ๋ค๋ฅธ ์ปดํฌ๋ํธ์ ์ํ๋ฅผ ๊ณต์ โ**ํ  ์ ์๋ค๋ ์ ์ด ๋ค๋ฆ๋๋ค.
- typescript์์ `atom` ํค์๋ ๋ค์ ์ค๋ ์ ๋ค๋ฆญ ํ์์ atom์ ๊ฐ์ ํ์์ ์๋ฏธํ๋ค.
- `key`๋ atom๋ง๋ค ๊ณ ์ ํ๊ฒ ๋ง๋ค์ด ์ค์ผํ๋ค. (key ์์ฑ ๊ท์น ์ ๋ ์ ํ๋๊ฒ ์ข๊ฒ ์ฃ ?!)
- `default`๋ ๊ธฐ๋ณธ ์ํ ๊ฐ์ ๋งํฉ๋๋ค.
- `effects_UNSTABLE` ์ต์์ ์ถํ์ ๋ค๋ฃจ๊ฒ ์ต๋๋ค.
- atom์ผ๋ก ๋ง๋  ์ํ๋ฅผ ์ฝ๋ ๋ชจ๋  ์ปดํฌ๋ํธ๋ atom์ ์ํ๊ฐ ๋ณ๊ฒฝ๋๋ฉด rerendering์ด ๋๋ค. (๊ตฌ๋ ๊ฐ๋๊ณผ ๊ฐ๋ค)

## 2-2. Selector

`atom ๋๋ ๋ค๋ฅธ selector๋ก ๊ตฌ์ฑํ ์์ํจ์`

**[ ๊ธฐ๋ณธ ์ ์ธ ์ฝ๋ ]**

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

- `atom` ๋๋ `selector๋ฅผ` ๊ธฐ๋ฐ์ผ๋ก ์๋กญ๊ฒ ๊ฒฐ๊ณผ๋ฅผ ๊ตฌ์ฑํด์ฃผ๋ ์์ํจ์ ์๋๋ค.
  - ์์ํจ์๋?! : ํ๋ ์ด์์ ์ธ์๋ฅผ ๋ฐ๊ณ  ๊ทธ ์ธ์๋ง์ผ๋ก ๊ฒฐ๊ณผ๋ฌผ์ ๋ง๋ค์ด ๋ด๋ ํจ์
- ๊ตฌ๋์ค์ธ `atom` ๋๋ `selector๊ฐ` ์๋ฐ์ดํธ ๋๋ฉด selector๋ ์๋ฐ์ดํธ ๋ฉ๋๋ค.
- `get`์ ์ด์ฉํ์ฌ ๋ค๋ฅธ `atom`์ ๊ตฌ๋ ํ  ์ ์์ต๋๋ค.
- ์ปดํฌ๋ํธ ์์ฅ์์๋ atom, selector ๋๋ค ๋์ผํ ์ธํฐํ์ด์ค๋ฅผ ๊ฐ์ง๊ธฐ ๋๋ฌธ์ ์๋ก ๋์ฒด๊ฐ ๊ฐ๋ฅํฉ๋๋ค.
- `set`์ ์ด์ฉํ์ฌ ๊ตฌ๋ํ๋ ์ํ์ ๊ฐ๋ค์ ์๋ฐ์ดํธ ํ  ์ ์์ต๋๋ค. (์ถํ์ ๋ค๋ฅธ ๊ธ์์ ์์ธํ ๋ค๋ฃจ๊ฒ ์ต๋๋ค.)

---

**Reference**

https://recoiljs.org/ko/docs/introduction/motivation
https://recoiljs.org/ko/docs/introduction/core-concepts
https://recoiljs.org/ko/docs/introduction/installation
https://recoiljs.org/ko/docs/introduction/getting-started
https://github.com/facebookexperimental/Recoil
