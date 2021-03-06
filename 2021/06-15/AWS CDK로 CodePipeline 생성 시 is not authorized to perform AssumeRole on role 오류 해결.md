# ๐คก ์ฌ์  ์กฐ๊ฑด, ๋ฐฐ๊ฒฝ ๋ฐ ๋ชฉํ

- CodePipeline์์ ํตํฉํ์ฌ ์ฌ์ฉํ  IAM Role์ ์ด๋ฏธ ์์ฑ๋์ด ์์
- ๊ณต์ฉ Artifact Store๋ก ์ฌ์ฉํ  S3 ๋ฒํท ์์ฑ๋์ด ์์
- ์์์ ๋ฏธ๋ฆฌ ๋ง๋ค์ด์ง Role ๋ฐ S3 Bucket์ ์ฌ์ฉํ์ฌ Code Pipeline์ CDK๋ก ์์ฑ
- ์์ค๋ TypeScript๋ก ์์ฑ๋์์ต๋๋ค.
- ์์ค ์์ ARN์ ์์๋ก ๋ง๋  ๊ฐ์๋๋ค. (๊ณต๊ฐํ ์๋ ์์์์!)

# ๐น ์์ค์ฝ๋ ์ค๋ช

## 1. IAM Role ARN์ผ๋ก ๋ถ๋ฌ์ค๊ธฐ

```typescript
import * as iam from "@aws-cdk/aws-iam";

const CODEPIPELINE_ROLE_ARN =
  "arn:aws:iam::000000000000:role/service-role/AWSCodePipelineServiceRole";

export class AdPlatformPipelineStack extends cdk.Stack {
  constructor(scope: cdk.Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

    // ...
    const role = iam.Role.fromRoleArn(this, "Role", CODEPIPELINE_ROLE_ARN, {
      mutable: false,
    });
    // ...
  }
}
```

- `aws-iam` ํจํค์ง์ `fromRoleArn` ํจ์๋ IAM Role์ ARN ๊ฐ์ผ๋ก ์ด๋ฏธ ์กด์ฌํ๋ IAM Role์ ๊ฐ์ ๋ถ๋ฌ์ต๋๋ค.
- `mutable` ์์ฑ์ AWS Managed Service๊ฐ ํด๋น IAM Role์ ์์  ๊ฐ๋ฅํ์ง ์ฌ๋ถ์ ๋ํ ์ต์์๋๋ค. (default: true, true = ์์  ๊ฐ๋ฅ, false = ์์  ๋ถ๊ฐ๋ฅ)
  - [๋ฐ๋ก๊ฐ๊ธฐ](https://github.com/aws/aws-cdk/blob/master/packages/%40aws-cdk/aws-iam/lib/role.ts#L148)
  - ์๋ฅผ ๋ค๋ฉด CodePipeline CDK์์ IAM Role์ Policy๋ฅผ ์ถ๊ฐํ๋ ค๋๋ฐ false๋ก ๋์ด์๋ค๋ฉด ์์ ์ด ๋ถ๊ฐ๋ฅํด์ง๋๋ค.

## 2. Artifact Store(S3) ๋ถ๋ฌ์ค๊ธฐ

```typescript
import * as s3 from "@aws-cdk/aws-s3";

export class AdPlatformPipelineStack extends cdk.Stack {
  constructor(scope: cdk.Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

    // ...
    const artifactBucket = s3.Bucket.fromBucketName(this, "store", "artifact");
    // ...
  }
}
```

- `artifact`๋ผ๋ Bucket ๋ช์ ๊ฐ์ง S3์ ๊ฐ์ฒด๋ฅผ ๋ถ๋ฌ์ค๋ ์ฝ๋ ์๋๋ค.

## 3. CodePipeline Action ์์ฑ

```typescript
import * as codepipeline_actions from "@aws-cdk/aws-codepipeline-actions";
import * as codebuild from "@aws-cdk/aws-codebuild";

export class AdPlatformPipelineStack extends cdk.Stack {
  constructor(scope: cdk.Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

    // ...
    const sourceOutput = new codepipeline.Artifact("Source");
    const sourceAction =
      new codepipeline_actions.CodeStarConnectionsSourceAction({
        actionName: "Get_from_bitbucket",
        owner: "myOrgan",
        repo: "myRepo",
        branch: "develop",
        output: sourceOutput,
        codeBuildCloneOutput: false,
        connectionArn:
          "arn:aws:codestar-connections:ap-northeast-2:000000000000:connection/aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
      });

    const project = new codebuild.PipelineProject(this, "MyProject");
    const buildOutput = new codepipeline.Artifact("Build");
    const buildAction = new codepipeline_actions.CodeBuildAction({
      actionName: "CodeBuild",
      project,
      input: sourceOutput,
      outputs: [buildOutput],
      executeBatchBuild: true,
    });
    // ...
  }
}
```

- CodePipeline์ ๋ง๋ค๊ธฐ ์ํด์๋ 2๊ฐ์ ์ ํจํ Stage๊ฐ ํ์ํฉ๋๋ค.
- ์๋ ๊ฐ๊ฐ ์๋์ ๊ฐ์ ๊ณผ์ ์๋๋ค.
  - CodeStar ์๋น์ค๋ฅผ ์ด์ฉํ์ฌ Bitbucket์์ SourceArtifact(`sourceOutput`)๋ฅผ ๋ฐ์์ค๋ ๊ณผ์ 
  - SourceArtifact๋ก CodeBuild ํ๋ก์ ํธ๋ฅผ ๋ง๋๋ ๊ณผ์ 

## 4. CodePipeline ์์ฑ

```typescript
import * as codepipeline from "@aws-cdk/aws-codepipeline";

export class AdPlatformPipelineStack extends cdk.Stack {
  constructor(scope: cdk.Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

    // ...
    const pipeline = new codepipeline.Pipeline(this, "CdkPipeline", {
      pipelineName: "cdk-pipeline",
      role,
      artifactBucket,
    });

    pipeline.addStage({ stageName: "Source", actions: [sourceAction] });
    pipeline.addStage({ stageName: "Build", actions: [buildAction] });
    // ...
  }
}
```

- ์ด์ ์ ์์ฑํ Action๋ค์ ๊ฐ Stage์ ๋งค์นญ ํฉ๋๋ค.

# ๐บ ์๋ฌ ๋ฉ์ธ์ง

![screensh](./img.png)

```
์คํ 4:13:32 | CREATE_FAILED        | AWS::CodePipeline::Pipeline | CodePipeline
arn:aws:iam::000000000000:role/service-role/AWSCodePipelineServiceRole is not authorized to perform AssumeRole on role arn:aws:iam::000000000000:role/CodePipelineStack-CodePipelineSourceGetf-11JMISA1AX433 (Service: AWSCodePipeline; Status Code: 400; Error Code: InvalidStructureException; Request ID: 561
dc127-bde5-4f2f-8e49-f2356c299b78; Proxy: null)
```

- ์์ฝํ์๋ฉด CodePipeline์ ์ฐ๊ฒฐํ IAM Role์ CodePipeline์์ ๋ณ๊ฒฝ์ด ๋ถ๊ฐ๋ฅํ๋ค๋ ์ด์ผ๊ธฐ ์ด๋ค.

# ๐พ ํด๊ฒฐ ๋ฐฉ๋ฒ

## ๋ฐฉ๋ฒ 1: Props ์ต์ ์์ 

- ์ ๋ง ๊ฐ๋จํ๋ค... ๋ฌธ์ ๊ฐ ๋ ์ด์ ๋ ์์ ๋งํ๋ `fromRoleArn` ํจ์์ `mutable` ์์ฑ์ false๋ก ์ค์ ํ์ฌ ๋ฌธ์ ๊ฐ ๋์๋ค.
  - ์ด๋์ CDK์์ IAM Role์ Policy๋ฅผ ์ถ๊ฐํ  ์ ์์.
- ์๋์ ๊ฐ์ด `fromRoleArn` ํจ์์ `mutable` ์์ฑ์ ์ ๊ฑฐํด์ฃผ๋ฉด ๋๋ค. (default๊ฐ true ๋ผ์ ์ง์ฐ๊ธฐ๋ง ํ๋ฉด ๋๋ค.)

```typescript
const role = iam.Role.fromRoleArn(this, "Role", CODEPIPELINE_ROLE_ARN);
```

- ์ฑ๊ณต ๊ฒฐ๊ณผ์ด๋ค!
  ![screensh](./img2.png)

- Option๋ค ์ข ์์ธํ ์๋ณด์!!!
  - Example ์ฝ๋๋ฅผ ๋ณด๊ณ  ๊ทธ๋๋ก ๊ฐ์ ธ์๋๋ฐ mutable ์์ฑ์ ๋ํ ์ค๋ช์ ์ข ๋์ถฉ ๋ณด์๋ค...
  - ์ด๊ฑฐ๋๋ฌธ์ ํ 15๋ถ์ ๋ฒ๋ฆฐ๊ฑฐ ๊ฐ๋ค...

## ๋ฐฉ๋ฒ 2: Role ์์ 

- Role์ ์๋์ ๊ฐ์ ์ ์ฑ์ ์ถ๊ฐํฉ๋๋ค.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": "sts:AssumeRole",
      "Resource": "*",
      "Effect": "Allow"
    }
  ]
}
```
