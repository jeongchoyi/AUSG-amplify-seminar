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
  

* **? Do you want to use an AWS profile? ** (Y/n)를 물으면 **Y** 입력 후 엔터

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

