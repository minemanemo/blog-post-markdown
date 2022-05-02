# 👋 개요

```
현 직장에서 일을 한지 벌써 9개월 정도 되었고

프로젝트가 바쁘다는 핑계로 미루어 두었던 포스팅을 해보려고 합니다.
```

현재 개발 중인 FE 서비스는 NextJS와 TypeScript를 기반으로 개발 중 입니다.

TypeScript에서는 특정 상수를 타입으로 정의 하는 방법이 몇가지 있고

그 중 다른 언어들과 같이 열거형(ENUM)을 지원해 주지만

저는 [Literal Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#literal-types) Union을 사용하는 것을 선호합니다.

(그 이유에 관한 글 첨부합니다. [TypeScript enum을 사용하지 않는 게 좋은 이유를 Tree-shaking 관점에서 소개합니다.](https://engineering.linecorp.com/ko/blog/typescript-enum-tree-shaking/))


# 🤨 Literal Types을 Union하여 사용하는 경우

```typescript
type UserStatus = 'NORMAL' | 'WITHDRAW' | 'DOMANT' | 'KNOWN';

interface UserInfomation {
  name: string;
  age: number;
  status: UserStatus;
}
```

로그인 시스템을 개발한다고 하였을 때 로그인 API를 만들 것이고 

BE로부터 사용자 정보를 받아오는 Response Body의 타입을 위와 같이 정의 할 수 있습니다.

# 🧐 문제 발생 포인트

하지만 이때 작은 문제점이 발생할 수 있습니다.

FE와 BE가 서로 약속을 하고 


// 예시 변경, Next Router 사용으로 바꿔보자
