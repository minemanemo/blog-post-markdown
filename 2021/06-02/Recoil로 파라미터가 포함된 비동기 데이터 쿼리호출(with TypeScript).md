# 개요

- Recoil로 비동기 데이터 쿼리를 호출 할 때 파라미터 유무에 따른 예제를 공유합니다.
- 전체적으로 코드에서 import 는 제외 하였습니다

---

# 🎃 파라미터가 필요 없는 경우

recoil의 selector를 이용해서 아래와 같이 비동기 데이터 쿼리를 호출 할 수 있습니다.

## 💁🏼‍♂️ 코드 예제

```typescript
interface Data {
  data1: string;
  data2: number;
  data3: boolean;
}

type Response = AxiosResponse<Data>;

const getData = (): Promise<Response> => axios.get("example.com/data");

const asyncDataQuery = selector<Data>({
  key: "asyncDataQuery",
  get: async () => {
    const response = await getData();
    return response.data;
  },
});
```

## 💁🏼 코드 설명

- `interface Data`, `type Response`는 반환 타입을 선언한 것입니다.
- `getData`는 axios로 example API를 호출하는 코드 입니다.
- `asyncDataQuery`는 Recoil의 selector를 이용하여 비동기 데이터 호출하는 코드 입니다.
  - 참고로 selector의 제네릭 타입은 Return 데이터 타입을 의미합니다.

---

# 🤖 파라미터가 필요한 경우

API 호출 코드에서 위와 같이 파라미터가 필요없을 수 있지만 아래와 같이 파라미터가 필요할 떄 어찌 해결할까요?

```typescript
const getData = (id: number): Promise<Response> =>
  axios.get("example.com/data", { params: { id } });
```

크게 두 가지 방법이 있습니다.

- 1️⃣ Recoil Atom + Recoil Selector
- 2️⃣ React Component State + Recoil SelelctorFamily

## 방법 1️⃣ : 일반적으로 생각할 수 있는 코드 예제

> Sometime so so...

- 처음 Recoil을 사용한다면 아래와 같이 풀어낼 수 있습니다.
- ID가 없을 때의 예외 사항은 고려하지 않았습니다 😅
- 데이터 타입들은 가독성을 위해서 코드에서 제외 하였습니다 😅

```typescript
const getData = (id: number): Promise<Response> =>
  axios.get("example.com/data", { params: { id } });

const userIdState = atom<number>({
  key: "userIdState",
  default: 0,
});

const asyncDataQuery = selector<Data>({
  key: "asyncDataQuery",
  get: async ({ get }) => {
    const userId = get(userIdState);
    const response = await getData(userId);
    return response.data;
  },
});

const App = () => {
  const [userId, setUserId] = useRecoilState(userIdState);
  const data = useRecoilValue(asyncDataQuery);

  useEffect(() => {
    setUserId(1);
  }, []);

  return (
    <div>
      userId : <span>{userId}</span>
      data1 : <span>{data.data1}</span>
      data2 : <span>{data.data2}</span>
      data3 : <span>{data.data3}</span>
    </div>
  );
};
```

## 🥲 코드 설명

- 아무래도 처음 생각하기에는 UserId를 atom으로 선언 후 컴포넌트 단에서 userId를 setter 함수를 이용하여 핸들링 하는 방법을 생각 할 수 있습니다.
- 그렇게 핸들링한 함수는 selector 안에서 get을 이용하여 구독하여 userId 값이 바뀔때마다 비동기 호출을 할 수 있습니다.
- 이러한 경우는 userId라는 Recoil에 종속된 state가 생성됩니다.
- Recoil State의 경우 일반적인 컴포넌트의 State와 생명주기가 다르기 때문에 관리 포인트가 추가됩니다.

## 방법 2️⃣ : 상황에 따라 좀더 좋은 코드 예제 🤔

> Best? 음... Better!

- 이런 파라미터에 따라 비동기 호출이 필요한 경우는 Recoil의 selectorFamily를 이용하면 됩니다.
- 데이터 타입들은 가독성을 위해서 코드에서 제외 하였습니다 😅

```typescript
const getData = (id: number): Promise<Response> =>
  axios.get("example.com/data", { params: { id } });

const asyncDataQuery = selectorFamily<Data, number>({
  key: "asyncDataQuery",
  get:
    (userId: number) =>
    async ({ get }) => {
      const response = await getData(userId);
      return response.data;
    },
});

const App = () => {
  const [userId, setUserId] = useState(0);
  const data = useRecoilValue(asyncDataQuery);

  useEffect(() => {
    setUserId(1);
  }, []);

  return (
    <div>
      userId : <span>{userId}</span>
      data1 : <span>{data.data1}</span>
      data2 : <span>{data.data2}</span>
      data3 : <span>{data.data3}</span>
    </div>
  );
};
```

## 👾 코드 설명

- 사실상 코드상으로 바뀐게 크게 없어 보입니다.
- Recoil atom으로 생성했던 state를 useState를 이용하여 컴포넌트에 종속되도록 변경하였습니다.
- Recoil을 사용하다 보면 Recoil state의 생명 주기에 관하여 고려를 안할 수 없기 때문에 때에 따라서 값이 변하는 API 파라미터의 경우는 Recoil을 이용하기 보다 useState로 컴포넌트에 종속적인 state로 사용하는 것이 보다 좋다는 판단을 하였습니다.
- 물론 때에 따라서는 다르겠지만요

---

# 🤠 결론

- Recoil State와 일반적인 React Component State와는 생명 주기가 다름
- 이러한 이유로 비동기 호출 하는 방법은 두가지로 나뉨
  - 1️⃣ Recoil Atom + Recoil Selector
  - 2️⃣ React Component State + Recoil SelelctorFamily
- 전자가 워스트라고 단정 지을 수 없지만 파라미터가 필요한 API 호출의 경우는 그때그때 상황에 따라 변경될 포인트가 많을 것이라고 판단됨
- 하지만 상황에 따라 다릅니다! 상황에 알맞게 1️⃣, 2️⃣ 방법을 사용하면 좋을 듯합니다!
