## 프로젝트 만들기

![](./img/1-1.png)

xcode에서 **Single View App** 을 만들어줍니다



![](./img/2-2.png)

위와 같이 설정하고 Next를 눌러줍니다.



## Amplify 설치

### Install the CLI

```bash
$ npm install -g @aws-amplify/cli
$ amplify congifure
```



### amplify configure 후

- AWS 콘솔 창이 열리면, AWS 콘솔 로그인
- 터미널로 돌아와서 
  - `Enter`
  - 리전 `ap-northeast-2` (서울)로 지정
  - IAM 유저 생성
    - `user name` : **ausg-amplify** 입력

* AWS 콘솔에서 다음과 같이 설정

  * ![img](./img/1.png)

* ![img](./img/2.png)

  

* **다음** - **다음** - **사용자 만들기** 클릭

  

  * 빨간 네모 안의 내용들을 `.csv 다운로드` 하거나, 창을 그대로 놔두고 터미널로 이동합니다!
    ![](./img/3.png)

  

- 터미널에서
  - `Enter`
  - ![](./img/4.png)
    - `accessKeyId`에는  `액세스 키 ID`, `secretAccessKey`에는 `비밀 액세스 키`를 넣어주세요.
    - 새 유저가 만들어졌습니다!



### IAM 권한 설정

- ![](./img/3.png)

  - 이 창을 그대로 열어두셨다면, `닫기` 버튼을 눌러주세요.

  - 이 창을 닫으셨다면,

    **콘솔** - **서비스** - **IAM 검색** - **사용자** 

- ![](./img/5.png)

- ![](./img/6.png)



#### CloudFormation 정책 추가하기

1. ![](./img/7.png)
2. ![](./img/8.png)
3. ![](./img/9.png)
4. ![](./img/10.png)
5. **권한 추가**



- 1~5번을 아래와 같이 5번 더 반복합니다!

  - Cognito Identity

  - ![](./img/11.png)

  - Cognito User Pools

    ![](./img/12.png)

  - IAM

    ![](./img/13.png)

  - Lambda

    ![](./img/14.png)

  - S3

    ![](./img/15.png)

    

- **정책 검토** 버튼을 클릭합니다

  - ![](./img/16.png)
    - 정책의 **이름을 입력** 하고, `요약` 의 내용이 스크린샷과 동일한지 확인합니다.

- **정책 생성** 버튼을 클릭합니다



### install Amplify

- `ctrl + C`

- react app의 root directory에서

- ```bash
  $ npm install --save aws-amplify
  $ npm install --save aws-amplify-react
  ```



### Set Up the AWS Backend

- 프로젝트 폴더 경로로 이동해서 amplify init 하기

  ```bash
  $ amplify init
  ```

- 아래와 같이 작성해줍니다 
  ( `environment : dev`, `editor : Visual Studio Code` 를 제외하고는 다 엔터! )

  ![](./img/18.png)

- **? Do you want to use an AWS profile?** (Y/n)를 물으면 **Y** 입력 후 엔터

- **ausg-amplify** 선택



### Implementing Authentication

```bash
$ amplify add auth
```

- **Do you want to use the default authentication and security configuration?**
  - **Default configuration** 선택
- **How do you want users to be able to sign in when using your Cognito User Pool?**
  - **Username** 선택
- **What attributes are required for signing up?**
  - **Email** 선택



```bash
$ amplify push
```

- **Are you sure you want to continue?**
  - **Yes** 입력



### Cocoapods

```bash
$ pod init
```



* Podfile 수정

  ```
  # Uncomment the next line to define a global platform for your project
  # platform :ios, '9.0'
  
  target 'iosAmplify' do
    # Comment the next line if you're not using Swift and don't want to use dynamic frameworks
    use_frameworks!
  
    # Pods for iosAmplify
    pod 'AWSUserPoolsSignIn'
    pod 'AWSAuthUI'
    pod 'AWSMobileClient'
  
  end
  ```



```
pod install
```

![](./img/99.png)



### Add awsconfiguration.json to project

iosAmplify.xcodeproj 파일을 열어줍니다

![](./img/100.png)

![](./img/101.png)

![](./img/102.png)



### modify AppDelegate.swift

AppDelegate.swift 파일을 열어줍니다.

```swift
import AWSMobileClient
```

```swift
//함수 추가
func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool{
        
        return AWSMobileClient.sharedInstance().interceptApplication(application, open: url, sourceApplication: sourceApplication, annotation: annotation)
        
    }
```

```swift
//함수 수정
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        return AWSMobileClient.sharedInstance().interceptApplication(application, didFinishLaunchingWithOptions: launchOptions)
        
    }
```



ViewController.swift 파일을 열어줍니다.

```swift
import UIKit

//import문 추가
import AWSAuthCore
import AWSAuthUI

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        
        //추가
        showSignIn()
    }
    
    //추가
    func showSignIn() {
        if !AWSSignInManager.sharedInstance().isLoggedIn {
            AWSAuthUIViewController.presentViewController(with: self.navigationController!, configuration: nil, completionHandler: { (provider: AWSSignInProvider, error: Error?) in
                if error != nil {
                    print("Error occured: \(String(describing: error))")
                }else{
                    print("Logged in with provider: \(provider.identityProviderName) with Token: \(provider.token())")
                }
            })
        }
    }


}

```



스토리보드에 가서 Editor - Embed In - Navigation Controller를 추가해줍니다

![](./img/103.png)



프로젝트를 빌드해주세요!



### 실습

![](./img/104.png)

초기 화면입니다.



![](./img/105.png)



회원가입을 하지 않고 로그인을 시도해보면,



![](./img/106.png)



유저가 없다는 오류가 뜹니다.



![](./img/107.png)

**Create new account** 버튼을 눌러 회원가입을 해줍시다!



![](./img/109.png)

네 항목을 채워줍니다.

핸드폰 번호는 **+821012341234** 형식으로 작성해주세요!



![](./img/110.png)

Confirmation code가 담긴 이메일을 확인해주세요.



![](./img/111.png)

회원가입이 성공적으로 완료되었습니다!



![](./img/112.png)

로그인 후 우리의 view controller가 보이는 모습입니다 :>