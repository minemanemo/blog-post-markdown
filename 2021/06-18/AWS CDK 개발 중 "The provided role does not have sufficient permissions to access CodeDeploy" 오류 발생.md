# πΎ κ°μ λ° μ€λ₯ λ©μΈμ§

AWS CDKλ‘ CodePipelineμ μμ€μ½λν μν€λ μμ μ€ μλμ κ°μ μλ¬κ° λ°μνμλ€.

> The provided role does not have sufficient permissions to access CodeDeploy

![screensh](./img1.png)

λ€μμ μ€λ₯κ° λ°μν cdk μ½λμ΄λ€. (TypeScript)

# ππΌββοΈ μμ€μ½λ

<details>
<summary>μ κΈ°/νΌμΉκΈ°!!</summary>
<div markdown="1">

```typescript
import * as cdk from "@aws-cdk/core";
import * as iam from "@aws-cdk/aws-iam";
import * as s3 from "@aws-cdk/aws-s3";
import * as ec2 from "@aws-cdk/aws-ec2";
import * as codepipeline from "@aws-cdk/aws-codepipeline";
import * as codepipeline_actions from "@aws-cdk/aws-codepipeline-actions";
import * as codebuild from "@aws-cdk/aws-codebuild";
import * as codedeploy from "@aws-cdk/aws-codedeploy";
import * as cloudwatch from "@aws-cdk/aws-logs";

const POSTFIX_STR = "_by-cdk";

export class PlatformPipelineStack extends cdk.Stack {
  constructor(scope: cdk.Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

    const role = iam.Role.fromRoleArn(
      this,
      "Role",
      process.env.CODEPIPELINE_ROLE_ARN || ""
    );

    const artifactBucket = s3.Bucket.fromBucketName(
      this,
      "ArtifactStore",
      "motov-artifact-store"
    );

    const sourceOutput = new codepipeline.Artifact("Source");
    const sourceAction =
      new codepipeline_actions.CodeStarConnectionsSourceAction({
        actionName: "Get_from_bitbucket",
        owner: "motov-sw",
        repo: "dsp-ad-app",
        branch: "develop",
        output: sourceOutput,
        codeBuildCloneOutput: false,
        triggerOnPush: false,
        connectionArn: process.env.CODESTAR_CONN_ARN || "",
      });

    const buildRole = iam.Role.fromRoleArn(
      this,
      "BuildRole",
      process.env.CODEBUILD_ROLE_ARN || ""
    );
    const project = new codebuild.PipelineProject(this, "BuildApp", {
      projectName: `build_dsp-ad-app${POSTFIX_STR}`,
      buildSpec: codebuild.BuildSpec.fromSourceFilename(
        "configuration/buildspec_develop.yml"
      ),
      role: buildRole,
      environment: {
        buildImage: codebuild.LinuxBuildImage.STANDARD_5_0,
        computeType: codebuild.ComputeType.MEDIUM,
        privileged: true,
      },
      logging: {
        cloudWatch: {
          logGroup: new cloudwatch.LogGroup(this, "BuildLog"),
          enabled: true,
        },
      },
      vpc: ec2.Vpc.fromLookup(this, "VPC", { vpcName: "Platform" }),
      subnetSelection: {
        subnets: [
          ec2.Subnet.fromSubnetId(this, "DevelopA", "subnet-aaaaaaaaaaaaaaaaa"),
          ec2.Subnet.fromSubnetId(this, "DevelopC", "subnet-ccccccccccccccccc"),
        ],
      },
      securityGroups: [
        ec2.SecurityGroup.fromLookup(
          this,
          "DefaultSecurityGroup",
          "sg-00000000000000000"
        ),
      ],
    });
    const buildOutput = new codepipeline.Artifact("Build");
    const buildAction = new codepipeline_actions.CodeBuildAction({
      actionName: "BuildAll",
      project,
      input: sourceOutput,
      outputs: [buildOutput],
    });

    const deployToDevAction =
      new codepipeline_actions.CodeDeployServerDeployAction({
        actionName: "DeployToDevelop",
        input: buildOutput,
        deploymentGroup:
          codedeploy.ServerDeploymentGroup.fromServerDeploymentGroupAttributes(
            this,
            "DeployGroup",
            {
              application:
                codedeploy.ServerApplication.fromServerApplicationName(
                  this,
                  "DeployApplication",
                  "Platform"
                ),
              deploymentGroupName: "develop_app",
            }
          ),
      });

    const pipeline = new codepipeline.Pipeline(this, "AppPipeline", {
      pipelineName: `app${POSTFIX_STR}`,
      role,
      artifactBucket,
    });

    pipeline.addStage({ stageName: "Source", actions: [sourceAction] });
    pipeline.addStage({ stageName: "Build", actions: [buildAction] });
    pipeline.addStage({ stageName: "Deploy", actions: [deployToDevAction] });
  }
}
```

</div>
</details>

- κ°μ’ ID λ€μ μμλ‘ λ°κΏλμμ΅λλ€!
- μ ν¨νμ§ μμ IDλ€μ΄μμ

# π₯Έ μμΈ λΆμ λ° μ‘°μΉ λ°©λ²

- μ€λ₯ λ©μΈμ§μμ μ μ μλ―μ΄ μμΈμ CodeDeployλ₯Ό μ€ννλλ° κΆνμ΄ λΆμ‘±νλ€λ μλ―Έ μλλ€.
- μ²μμ μ½μλ‘ μ€μ ν λλ κΆν(IAM Role)μ΄ κ°λ€κ³  μκ°νλλ°
- CDKμμ CodePipeline κ°λ° μμλ Pipeline Actionμ roleλ μ κ²½ μ¨μ€μΌ νλ€λ κ²μ μμμ΅λλ€.
- λ°λΌμ μλμ κ°μ΄ Deploy Action μμ νμμ΅λλ€.

  - CodePipeline Actionμ CodePipelineμμ μ°λ Roleμ κ°μ΄ μΆκ°ν΄μ£Όλ ν΄κ²° λμμ΅λλ€.!

```typescript
// ...
const deployToDevAction = new codepipeline_actions.CodeDeployServerDeployAction({
  // ...
  deploymentGroup: codedeploy.ServerDeploymentGroup.fromServerDeploymentGroupAttributes(
    this,
    role: iam.Role.fromRoleArn(this, "Role", "ARN ID!!!");
    // ...
  ),
});
// ...
```
