# ๐ฅฒ ๋ฌธ์  ๋ฐ์ ์ฝ๋

```typescript
interface ListParam {
  page?: number;
  pageSize?: number;
  orderBy?: "DESC" | "ASC";
  sortBy?: string;
  total?: boolean;
}

type Param = ListParam;

export const listQuery = selectorFamily<Data[], Param>({
  key: "listQuery",
  get: (param) => async () => {
    const { data } = await getList(param);
    return data;
  },
});
```

- Data๋ API ์์ฒญ ํ ๋ฐํ๋๋ ๋ฐ์ดํฐ ํ์์๋๋ค.
- Param์ Recoil์ ํ๋ผ๋ฏธํฐ์ ๋ค์ด๊ฐ ํ์ ์๋๋ค.
- getList ํจ์๋ api๋ฅผ ํธ์ถํ๋ ํจ์ ์๋๋ค.

# ๐คจ ์๋ฌ ๋ฉ์ธ์ง

![screensh](./img1.png)

- "์ธ๋ฑ์ค ์๊ทธ๋์ฒ๊ฐ ์์ต๋๋ค." ๋ผ๋ ์๋ฌ ๋ฉ์ธ์ง ๋ฐ์

```typescript
type Primitive = undefined | null | boolean | number | symbol | string;
```

- Recoil์ ํ์ ์ค [Primitive](https://github.com/facebookexperimental/Recoil/blob/master/typescript/index.d.ts#L292)์ ํด๋นํ๋ ๋จ์ํ ํ๋ผ๋ฏธํฐ๋ ์๊ด ์์ง๋ง ์์ ์์ ์ `Param`๊ณผ ๊ฐ์ด object ํ์์ ํ์์ ์๋ฌ๊ฐ ๋ฐ์ํ๋ค.

# ๐ง ์์ธ

```typescript
type Primitive = undefined | null | boolean | number | symbol | string;

export type SerializableParam =
  | Primitive
  | { toJSON: () => string }
  | ReadonlyArray<SerializableParam>
  | Readonly<{ [key: string]: SerializableParam }>;
```

[Recoil github ์ฐธ๊ณ  ๋งํฌ](https://github.com/facebookexperimental/Recoil/blob/master/typescript/index.d.ts#L294)

- ์์ ์์ค์ฝ๋๋ selectorFamily์ ๋๋ฒ์งธ ์ ๋ค๋ฆญ ํ์์ ํด๋นํ๋ Parameter์ ๊ด๋ จ๋ ํ์์ด๋ค.
- ํด๋น ํ์์ผ๋ก ์ธํ์ฌ ์ฐ๋ฆฌ์ ์ฝ๋์์ ์๋ฌ๊ฐ ๋ฐ์ํ์๋ค
- orderBy์ ๊ฒฝ์ฐ๋ SerializableParam์ ์๋ง์ง ์๋ค. (Primitive ํ์์ ๋ณด๋ฉด `"DESC" | "ASC"` ๊ฐ์ ํ์์ ์๋ค.)

# ๐ฅธ ์กฐ์น ๋ฐฉ๋ฒ

```typescript
interface Param extends ListParam {
  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  [key: string]: any;
}
```

- ํด๋น ์ค๋ฅ๋ฅผ ํผํด๊ฐ๊ธฐ ์ํด ์์ ๊ฐ์ด `ListParam`์ ์์๋ฐ์ ์ธ๋ฑ์ค ์๊ทธ๋์ฒ ์์ฑ์ ์ถ๊ฐํด์ฃผ์์ต๋๋ค.
- ESLint์ any ์ค๋ฅ๋ฅผ ํผํด๊ฐ๊ธฐ ์ํด ignore ์ฃผ์์ ์ถ๊ฐํด์ฃผ์์ต๋๋ค.

### ์ฐธ๊ณ 

https://yamoo9.gitbook.io/typescript/interface/index-signature
