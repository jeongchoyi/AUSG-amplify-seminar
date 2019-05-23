### Install the CLI

```bash
$ npm install -g @aws-amplify/cli
$ amplify congifure
```



### amplify configure 후

* AWS 콘솔 창이 열리면, AWS 콘솔 로그인

* 터미널로 돌아와서 

  * `Enter`
  * 리전 `ap-northeast-2` (서울)로 지정
  * IAM 유저 생성
    * `user name` : **ausg-amplify** 입력

* AWS 콘솔에서 다음과 같이 설정

  * ![](./img/1.png)
    

  * ![](./img/2.png)

    

  * **다음** - **다음** - **사용자 만들기** 클릭

  

  * 빨간 네모 안의 내용들을 `.csv 다운로드` 하거나, 창을 그대로 놔두고 터미널로 이동합니다!
    ![](./img/3.png)

    

* 터미널에서

  * `Enter`
  * ![](./img/4.png)
    *  `accessKeyId`에는  `액세스 키 ID`, `secretAccessKey`에는 `비밀 액세스 키`를 넣어주세요.
    * 새 유저가 만들어졌습니다!



### IAM 권한 설정

* ![](./img/3.png)

  * 이 창을 그대로 열어두셨다면, `닫기` 버튼을 눌러주세요.

  * 이 창을 닫으셨다면,

    **콘솔** - **서비스** - **IAM 검색** - **사용자** 

* ![](./img/5.png)

* ![](./img/6.png)



#### CloudFormation 정책 추가하기

1. ![](./img/7.png)
2. ![](./img/8.png)
3. ![](./img/9.png)
4. ![](./img/10.png)
5. **권한 추가**



* 1~5번을 아래와 같이 5번 더 반복합니다!

  * Cognito Identity![](./img/11.png)

  * Cognito User Pools

    ![](./img/12.png)

  * IAM

    ![](./img/13.png)

  * Lambda

    ![](./img/14.png)

  * S3

    ![](./img/15.png)

* **정책 검토** 버튼을 클릭합니다

  * ![](./img/16.png)
    * 정책의 **이름을 입력** 하고, `요약` 의 내용이 스크린샷과 동일한지 확인합니다.

* **정책 생성** 버튼을 클릭합니다



### create a new app

* 터미널에서

  * ```bash
    $ npm install -g create-react-app
    $ create-react-app ausgapp && cd ausgapp
    $ npm start
    ```

* ![](./img/17.png)



### install Amplify

* `command + C`

* react app의 root directory에서

* ```bash
  $ npm install --save aws-amplify
  $ npm install --save aws-amplify-react
  ```



### Set Up the AWS Backend

* amplify init 하기

  ```bash
  $ amplify init
  ```

* 아래와 같이 작성해줍니다 
  ( `environment : dev`, `editor : Visual Studio Code` 를 제외하고는 다 엔터! )

  ![](./img/18.png)
  

* **? Do you want to use an AWS profile?** (Y/n)를 물으면 **Y** 입력 후 엔터

* **ausg-amplify** 선택



### Implementing Authentication

```bash
$ amplify add auth
```

* **Do you want to use the default authentication and security configuration?**
  * **Default configuration** 선택
* **How do you want users to be able to sign in when using your Cognito User Pool?**
  * **Username** 선택
* **What attributes are required for signing up?**
  * **Email** 선택



```bash
$ amplify push
```

* **Are you sure you want to continue?**
  * **Yes** 입력



### Adding Authentication to the React App

* **src/index.js** 수정

  ```react
  import Amplify from 'aws-amplify'
  import config from './aws-exports'
  Amplify.configure(config)
  ```

  

* **src/App.js** 수정

  ```react
  //파일의 맨 위에 추가
  import { withAuthenticator } from 'aws-amplify-react'
  
  //파일의 맨 밑 줄 대체
  export default withAuthenticator(App)
  ```



### 앱 실행

```bash
// Build Command
$ npm run-script build
// Start Command
$ npm run-script start
```



![](./img/19.png)

* Create account —> 작성한 **이메일** 로 온 인증코드 입력 —> 로그인



## Cognito 살펴보기

![](./img/20.png)

* AWS 콘솔 - 서비스 - Cognito 검색

![](./img/21.png)

* 사용자 풀 관리 클릭

![](./img/22.png)

* 우리가 만들어 둔 사용자 풀! 클릭합니다



### 속성

![](./img/23.png)

* 왼쪽 메뉴에서 일반 설정 - 속성을 클릭합니다

![](./img/24.png)

* `amplify add auth`시 설정했던 사항들 확인 가능! (**변경 불가**)



### 정책

![](./img/25.png)

* 왼쪽 메뉴에서 일반 설정 - 정책을 클릭합니다.



![](./img/26.png)

* 암호 강도, 사용자 가입 허용 여부 등을 설정할 수 있습니다.

* 여러분들은 `사용자가 가입할 수 있도록 허용` 상태일 텐데요, 

  ![](./img/28.png)

  저는 `관리자만 사용자를 생성할 수 있도록 허용` 으로 바꿔보겠습니다!

  콘솔에서 ` 변경 내용 저장` 을 누른 후, 터미널에서 `npm run-script start` 또는 `npm start` 를 실행해보겠습니다.

* ![](./img/27.png)

  위와 같이, `CREATE ACCOUNT` 버튼을 클릭하면 `A client attempted to write unauthorized attribute` 오류가 뜨는 것을 볼 수 있네요!



### Creating The Serverless Backend Services

```bash
// 프로젝트의 루트 디렉토리에서
$ amplify add api
```

* **? Please select from one of the below mentioned services**
  * `REST` 선택
* **? Provide a friendly name for your resource to be used as a label for this category in the project**
  * `todoAPI` 입력
* **? Provide a path (e.g., /items) **
  * `Enter`
* **? Choose a Lambda source**
  * `Create a new Lambda function` (Enter)

* **? Provide a friendly name for your resource to be used as a label for this category in the project**
  * `todoLambda` 입력
* **? Provide the AWS Lambda function name**
  * `todo` 입력
* **? Choose the function template that you want to use**
  * `CRUD function for Amazon DynamoDB table (Integration … )` 선택
* **? Choose a DynamoDB data source option**
  * `Create a new DynamoDB table` 선택

![](./img/29.png)



#### NoSQL DynamoDB database wizard 

로 진입했습니다! wizard를 사용해서 NoSQL 데이터베이스 테이블 세팅을 해 봅시다!



* **? Please provide a friendly name for your resource that will be
  used to label this category in the project**
  * `todoTable` 입력
* **? Please provide table name**
  * `todo` 입력

![](./img/30.png)



테이블에 column을 추가 해 봅시다.



* **? What would you like to name this column**
  * `id`
* **? Please choose the data type**
  * `string`
* **? Would you like to add another column?**
  * `No`

![](./img/31.png)



