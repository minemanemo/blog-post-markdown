# ๐พ ๊ฐ์

- ์ด์ฌํ ๊ธฐ์กด์ ์ด์ํ๋ CodePipeline์ CDK๋ก ์ฎ๊ธฐ๋ ์์ ์ค ์๋์ ๊ฐ์ ์๋ฌ๊ฐ ๋ฐ์ํ๋ค.
  ![screensh](./img1.png)

```
Error calling startBuild: Cannot have more than 0 builds in queue for the account
(Service: AWSCodeBuild;
Status Code: 400;
Error Code: AccountLimitExceededException;
Request ID: f0eca668-cd8e-4a4a-8a93-ed28f2e24305;
Proxy: null)
```

- ์ ์ฌํญ์ด ๋ฐ์ํ๊ธฐ๊น์ง ์๋์ ๊ฐ์ ํ๊ฒฝ์ ๊ฐ์ง๊ณ  ์์๋ค.
- ์ต๊ทผ์ CDK๋ฅผ ํตํด์ CodePipeline์ ์์ค๋ก ์ ํ ์ค์ ์์.
- ์ต๊ทผ์ CodePipeline ์คํ ์๊ฐ ๋ง์.

# ๐ฆ ์์ธ ๋ถ์

- ์ค๋ฅ ๋ด์ฉ์ ๋ณด๋ฉด ์ ์ ์๋ฏ์ด AWS Account ๋ง๋ค CodeBuild ์คํ ์์ ๋ํ ์ ํ์ด ์๋ ๊ฒ์ผ๋ก ๋ณด์๋ค. ([ํด๋น ๋ฌธ์์ Build failed to start ์ฐธ๊ณ ](https://docs.aws.amazon.com/codebuild/latest/userguide/troubleshooting.html#troubleshooting-build-failed-to-start))
- ๋์ ๋น๋ ์ ํ ์๋ฅผ ๋๋ ค์ผํ๋ค! (Maximum number of concurrently running builds)

# ๐ค ์กฐ์น ๋ฐฉ๋ฒ

- [์ฐธ๊ณ  ๋งํฌ](https://docs.aws.amazon.com/codebuild/latest/userguide/limits.html)

1. Service Quotas ์ฝ์ ์ ์ (๋ฆฌ์  ์ ํ ์ฃผ์!!!)
   - [์ ์ ๋งํฌ](https://console.aws.amazon.com/servicequotas/home/services/codebuild/quotas/L-75822022)
   - [ํ๊ตญ ๋ฆฌ์ ์ผ๋ก ๋ฐ๋ก ์ ์ ๋งํฌ](https://ap-northeast-2.console.aws.amazon.com/servicequotas/home/services/codebuild/quotas/L-75822022)
2. ํ ๋น๋ ์ฆ๊ฐ ์์ฒญ
3. ๋ณ๊ฒฝํ  ๊ฐ ์๋ ฅ ํ ์์ฒญ (์ ๋ ๊ธฐ๋ณธ๊ฐ์ ๋๋ฐฐ์ธ 120์ผ๋ก ์์ฒญํ์)
4. ์๋ฃ ๊น์ง ๋๊ธฐ...
   - ์ ๋ ์ฝ ํ๋ฃจ ๊ฑธ๋ ธ์ต๋๋ค.
   - ์๋ฃ ์ ํ๋ฉด
     ![screensh](./img2.png)
