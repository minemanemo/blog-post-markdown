# 개요

- 블로그 포스팅을 Markdown으로 작성하여 누적
- TiStory로 비공개 포스팅 배포 (markdown-tistory: [참고](https://github.com/jojoldu/markdown-tistory))
- 이미지는 5M 이하
- 제목은 마크다운 파일명

# Usage

**Install**

```
npm install -g markdown-tistory
```

**블로그 정보 초기화**

```
markdown-tistory init code
```

**블로그 정보 보기**

```
markdown-tistory show 에디터
```

**토큰 받아오기 (1달에 1번)**

```
markdown-tistory token
```

**글 등록**

```
markdown-tistory write
markdown-tistory write [절대 경로]
markdown-tistory write [상대 경로]
```

**글 수정**

```
markdown-tistory update [파일위치] [포스팅ID]
markdown-tistory update [포스팅ID]
```

**애드 샌스 관련**

```
markdown-tistory ad cod
```

# 포스팅 주제

- AWS 네트워크 아키텍처 구성 및 구축
- AWS 네트워크 IoC (Terraform)
