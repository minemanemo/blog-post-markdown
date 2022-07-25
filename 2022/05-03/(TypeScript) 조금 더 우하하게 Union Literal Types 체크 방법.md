# 👋 개요

> 현직장에서 프로젝트가 바쁘다는 핑계로 미루어 두었던 포스팅을 해보려고 합니다. (벌써 1년이나;;)

현재 개발 중인 FE 서비스는 NextJS와 TypeScript를 기반으로 개발 중 입니다.

TypeScript에서는 특정 상수를 타입으로 정의 하는 방법이 몇가지 있고

그 중 다른 언어들과 같이 열거형(ENUM)을 지원해 주지만

저는 [Literal Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#literal-types) Union을 사용하는 것을 선호합니다.

(그 이유에 관한 글 첨부합니다. [TypeScript enum을 사용하지 않는 게 좋은 이유를 Tree-shaking 관점에서 소개합니다.](https://engineering.linecorp.com/ko/blog/typescript-enum-tree-shaking/))


# 🤨 Literal Types을 Union하여 사용하는 경우

```typescript
type UserStatus = 'NORMAL' | 'WITHDRAW' | 'UNKNOWN'; // 일반회원 | 탈퇴회원 | 알수없는 회원

interface UserInfomation {
  name: string;
  age: number;
  status: UserStatus;
}
```

로그인 시스템을 개발한다고 하였을 때 로그인 API를 만들 것이고 

BE로부터 사용자 정보를 받아오는 Response Body의 타입을 위와 같이 정의 할 수 있습니다.

# 🧐 문제 발생 포인트


BE 개발자와 FE 개발자가 서로 API 명세를 최신화하면 좋지만 그렇지 않은 경우가 있습니다.

아래와 같이 API 서버 측에서는 휴면회원 상태를 추가로 정의하였지만 FE 측에서 최신화되지 않은 경우가 있습니다. 

```typescript
// 일반회원 | 탈퇴회원 | 알수없는 회원
type UserStatus = 'NORMAL' | 'WITHDRAW' | 'UNKNOWN';

interface UserInfomation {
  name: string;
  age: number;
  status: UserStatus;
}

// UserStatus에서 정의하지 않은 DORMANT
// { name: '김예제', age: 20, status: 'DORMANT' }
const res: UserInfomation = await fetch('/login'); 

if (res.status === 'NORMAL' || res.status === 'WITHDRAW' || res.status === 'UNKNOWN') {
  console.log('정의된 상태 입니다.');
} else {
  console.log('정의되지 않은 상태 입니다.');
}
```

위 코드에서는 `DORMANT` 회원은 정상적인 상태로 확인을 하지 못합니다.

예외 처리를 추가해보겠습니다.

```typescript
// 일반회원 | 탈퇴회원 | 알수없는 회원 | 휴면회원
type UserStatus = 'NORMAL' | 'WITHDRAW' | 'UNKNOWN' | 'DORMANT';

interface UserInfomation {
  name: string;
  age: number;
  status: UserStatus;
}

const res: UserInfomation = await fetch('/login'); 

if (res.status === 'NORMAL' || res.status === 'WITHDRAW' || res.status === 'UNKNOWN' || res.status === 'DORMANT') {
  console.log('정의된 상태 입니다.');
} else {
  console.log('정의되지 않은 상태 입니다.');
}
```


쉽게 조건문을 수정하여 예외 처리를 하였습니다.

근데 만약 이번과 다르게 `UserStatus`의 타입이 하나가 아닌 10개의 상태가 추가로 정의되면 10개의 조건을 추가해야할까요?

너무 불편하겠죠?

한번 우아하게 처리해봅시다

# 🤠 우아하게 Literal String Union Type 검사하기


```typescript
// 1️⃣
function generationUnionTypeChecker<UnionType extends string>(...values: UnionType[]) {
  return function (value: unknonwn): UnionType | false {
    if (typeof value !== 'string') return false;
    return values.includes(value as UnionType) ? value as UnionTYpe : false;
  }
}

// 2️⃣
const userStatus = ['NORMAL', 'WITHDRAW', 'UNKNOWN', 'DORMANT'] as const;
type UserStatus = typeof userStatus[number];
const isUserStatus = generationUnionTypeChecker(...userStatus);

// ...

const res: UserInfomation = await fetch('/login');

// 3️⃣
if (isUserStatus(res.status)) {
  console.log('정의된 상태 입니다.');
} else {
  console.log('정의되지 않은 상태 입니다.');
}
```

1️⃣ Union Type 배열을 입력받으면 String Union Type Check 생성하는 함수를 생성해줍니다.

2️⃣ as const 를 이용하여 String Union Type 지정을 지정해주고 `generationUnionTypeChecker`를 이용하여 타입 검사 함수를 생성합니다.

3️⃣ type 검사 합수 사용 

---

도움이 되셨으면 좋겠습니다.

