
# 👋 개요

현 직장에서 일을 한지 벌써 9개월 정도 되었고

프로젝트가 바쁘다는 핑계로 미루어 두었던 포스팅을 해보려고 한다.

현재 개발 중인 FE 서비스는 NextJS와 TypeScript를 기반으로 개발 중 입니다.

TypeScript에서는 열거형(ENUM)을 지원해 주지만

저는 [Literal Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#literal-types)을 Union하여 사용하는 것을 선호합니다.
(그 이유에 관한 글 첨부합니다. [TypeScript enum을 사용하지 않는 게 좋은 이유를 Tree-shaking 관점에서 소개합니다.](https://engineering.linecorp.com/ko/blog/typescript-enum-tree-shaking/))

```typescript

type PlayerAction = 'START' | 'STOP' | 'NEXT' | 'PREV' | 'VOLUME_UP' | 'VOLUME_DOWN';

```

예시는 아래와 같은 음악 감상 프로그램을 만든다고 했을 때 필요한 ACTION을 타입으로 정의했습니다.

// TODO: 그림

