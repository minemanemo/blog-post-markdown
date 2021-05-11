조건에 따라서 태그 이름을 다르게 하고 싶었다.

일반적인 Bash Shell의 조건문을 사용하였고 실패하고 성공한 케이스를 썼다.

(참고로 나는 version 0.2를 사용하였다.)

(몇시간 삽질했네;)

## 실패 케이스

- 코드

```yml
version: 0.2

phases:
  build:
    commands:
      - |
        if [ ${CLUSTER_SERVICE} -eq "1" ]; then
          export TAG_NAME=${SERVICE}-${MAJOR}.${MINOR}.${PATCH}
        else
          export TAG_NAME=${MAJOR}.${MINOR}.${PATCH}
        fi
```

- AWS Codebuild 실행 결과

```bash
[Container] 2021/05/10 13:06:11 Running command if [[ "${CLUSTER_SERVICE}" -eq "" ]]; then
  export TAG_NAME=${MAJOR}.${MINOR}.${PATCH}
else
  export TAG_NAME=${SERVICE}-${MAJOR}.${MINOR}.${PATCH}
fi

/codebuild/output/tmp/script.sh: line 4: : command not found
```

## 성공 케이스

> 조건문의 조건식을 `[[ ]]` 꼭 괄호 두개로 감싸 주자

- 코드

```yml
version: 0.2

phases:
  build:
    commands:
      - |
        if [[ ${CLUSTER_SERVICE} -eq "1" ]]; then
          export TAG_NAME=${SERVICE}-${MAJOR}.${MINOR}.${PATCH}
        else
          export TAG_NAME=${MAJOR}.${MINOR}.${PATCH}
        fi
```

- AWS Codebuild 실행 결과

```bash
[Container] 2021/05/10 13:14:50 Running command if [[ "${CLUSTER_SERVICE}" -eq "" ]]; then
  export TAG_NAME=${MAJOR}.${MINOR}.${PATCH}
else
  export TAG_NAME=${SERVICE}-${MAJOR}.${MINOR}.${PATCH}
fi
```
