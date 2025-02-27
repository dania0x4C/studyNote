### 🚀 **AWS Lambda + ECR + GitHub Actions을 활용한 CI/CD 구축 (TypeScript + Docker 기반)**

👉 **AWS Lambda에서 컨테이너 기반 Node.js 실행**  
👉 **GitHub Actions으로 코드 변경 시 자동 배포 (ECR → Lambda)**  
👉 **`.env` 없이 환경 변수는 AWS Lambda에서 직접 설정**  
👉 **최종적으로 `git push`만 하면 자동 배포되는 구조 완성!**

---

## **✅ 1️⃣ AWS CLI 설정 (Root 계정 로그인)**

🚩 **어디서? → VSCode 터미널**

📌 **AWS 계정 로그인 확인**
```
aws sts get-caller-identity
```

✅ **결과 (`051826704817` 계정으로 로그인됨)**

```
{   "UserId": "051826704817",     
	"Account": "051826704817",    
	"Arn": "arn:aws:iam::051826704817:root" 
}
```

## **✅ 2️⃣ AWS ECR 저장소 생성**

🚩 **어디서? → VSCode 터미널**

📌 **ECR 저장소 생성**
```
aws ecr create-repository --repository-name lambda-docker-ts --region ap-northeast-2
```

📌 **ECR 저장소 확인**
```
aws ecr describe-repositories --repository-name lambda-docker-ts --region ap-northeast-2
```

✅ **ECR 저장소 URI (`051826704817.dkr.ecr.ap-northeast-2.amazonaws.com/lambda-docker-ts`) 확인**

---

## **✅ 3️⃣ TypeScript Lambda 함수 작성 (`src/index.ts`)**

🚩 **어디서? → `src/index.ts` 생성**

```
import { APIGatewayEvent, Context } from "aws-lambda";  export const handler = async (event: APIGatewayEvent, context: Context) => {     console.log("Received event:", JSON.stringify(event));      return {         statusCode: 200,         body: JSON.stringify({ message: "🚀 Lambda 자동 배포 성공!" }),     }; };
```


✅ **Lambda가 실행될 때 메시지를 반환하도록 설정!**

---

## **✅ 4️⃣ Dockerfile 작성**

🚩 **어디서? → 프로젝트 루트 (`Dockerfile` 생성)**
```
FROM public.ecr.aws/lambda/nodejs18.x  WORKDIR /var/task  COPY package.json package-lock.json ./ RUN npm install --only=production  COPY dist/ ./dist/  CMD ["dist/index.handler"]
```


✅ **AWS Lambda에서 실행할 Node.js 환경을 Docker 컨테이너로 구성**

---

## **✅ 5️⃣ GitHub Actions을 활용한 CI/CD 구축**

🚩 **어디서? → `.github/workflows/docker-image.yml` 생성**

```
name: Deploy Lambda Docker TS (ECR 사용)  on:   push:     branches:       - main  jobs:   deploy:     runs-on: ubuntu-latest      steps:       - name: Checkout 코드 가져오기         uses: actions/checkout@v3        - name: AWS 인증         uses: aws-actions/configure-aws-credentials@v2         with:           aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}           aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}           aws-region: ${{ secrets.AWS_REGION }}        - name: Docker 로그인 to AWS ECR         run: |           aws ecr get-login-password --region ${{ secrets.AWS_REGION }} | \           docker login --username AWS --password-stdin ${{ secrets.ECR_REPO_URI }}       - name: Docker 빌드 및 태깅         run: |           docker build -t lambda-docker-ts .           docker tag lambda-docker-ts ${{ secrets.ECR_REPO_URI }}:latest       - name: Docker 이미지 푸시 to ECR         run: |           docker push ${{ secrets.ECR_REPO_URI }}:latest       - name: Lambda 업데이트         run: |           aws lambda update-function-code --function-name ${{ secrets.LAMBDA_FUNCTION_NAME }} \             --image-uri ${{ secrets.ECR_REPO_URI }}:latest

```

✅ **GitHub Actions이 코드 변경 시 자동으로 Docker 빌드 → ECR 푸시 → Lambda 업데이트 수행!**

---

## **✅ 6️⃣ GitHub Secrets 설정 (AWS 인증 정보 저장)**

🚩 **어디서? → GitHub 리포지토리 (`Settings → Secrets and variables → Actions → New repository secret`)**

📌 **GitHub Secrets에서 아래 값 추가**

|Key|Value|
|---|---|
|`AWS_ACCESS_KEY_ID`|(AWS IAM 콘솔에서 발급한 Access Key)|
|`AWS_SECRET_ACCESS_KEY`|(AWS IAM 콘솔에서 발급한 Secret Key)|
|`AWS_REGION`|`ap-northeast-2`|
|`AWS_ACCOUNT_ID`|`051826704817`|
|`ECR_REPO_URI`|`051826704817.dkr.ecr.ap-northeast-2.amazonaws.com/lambda-docker-ts`|
|`LAMBDA_FUNCTION_NAME`|`my-lambda`|

✅ **Secrets 설정 완료 후, GitHub Actions 실행 준비 완료!**

---

## **✅ 7️⃣ AWS Lambda 함수 생성**

🚩 **어디서? → AWS Lambda 콘솔**

1. **AWS Lambda 콘솔 이동** ([AWS Lambda 관리](https://console.aws.amazon.com/lambda))
2. **"함수 생성" 클릭**
    - **"컨테이너 이미지"** 선택
    - **"이미지 URI"** 입력:
		```
        051826704817.dkr.ecr.ap-northeast-2.amazonaws.com
        /lambda-docker-ts:latest
		```
        
    - 기타 설정은 기본값
3. **Lambda 함수 생성 완료 후, "배포" 클릭**

✅ **이제 Lambda가 ECR에서 이미지를 가져와 실행 가능!**

---

## **✅ 8️⃣ GitHub Actions 실행 (코드 푸시)**

🚩 **어디서? → VSCode 터미널**

📌 **GitHub Actions 실행을 위해 코드 푸시**
```
git add . git commit -m "Set up GitHub Actions for Lambda deployment" git push origin main
```

✅ **코드 푸시 후, GitHub → Actions 탭에서 진행 상태 확인 가능!**

---

## **✅ 9️⃣ 최종 테스트 (Lambda 업데이트 확인)**

🚩 **어디서? → AWS Lambda 콘솔 & 터미널**

📌 **AWS Lambda 콘솔에서 실행**

1. AWS Lambda 콘솔 이동
2. `my-lambda` 함수 선택
3. "테스트" 실행 → 정상 응답 `{ "message": "🚀 Lambda 자동 배포 성공!" }` 확인
