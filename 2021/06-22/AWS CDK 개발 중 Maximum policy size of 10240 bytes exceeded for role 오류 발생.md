# 💁🏼‍♂️ 이슈 발생 배경

- AWS CDK로 CodePipeline을 개발 중에 있었다.
- AWS CDK로 CodePipeline, CodeBuild 생성 시 따로 role을 설정하지 않으면 자동으로 생성된다.
- 이때 여러개의 Role이 생성되는게 지저분에 해여서 본인은 CodePipeline, CodeBuild 별로 사용하는 Role을 만들고 해당 Role을 불러와서 사용하는 방법을 채택 하였음.
  - mutable 설정을 하면서 (링크 추가) 계속 IAM Role에 Policy가 추가되었지만 아래와 같은 결국 에러가 발생함

```
Maximum policy size of 10240 bytes exceeded for role
```

https://aws.amazon.com/ko/premiumsupport/knowledge-center/iam-increase-policy-size/

나는 직접 공동 policy를 추가하여 문제 해결하였음
