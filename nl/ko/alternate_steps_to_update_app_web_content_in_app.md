---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-14"

keywords: update web content in apps, update apps

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# 앱의 웹 컨텐츠를 업데이트하는 대체 단계
{: #alternate_steps_to_update_app_web_content_in_app}

앱의 웹 컨텐츠를 업데이트하기 위한 일부 대체 방법이 아래 나열되어 있습니다.

* `.zip` 파일을 빌드하여 다른 Mobile Foundation 서버에 업로드: `mfpdev app webupdate [server-name] [runtime-name]`.
  예:
  ```bash
  mfpdev app webupdate myQAServer MyBankApps
  ```

* 이전에 생성된 `.zip` 파일 업로드: `mfpdev app webupdate [server-name] [runtime-name] --file [path-to-packaged-web-resources]`.
  예:
  ```bash
  mfpdev app webupdate myQAServer MyBankApps --file mobilefirst/ios/com.mfp.myBankApp-1.0.1.zip
  ```

* 패키지된 웹 리소스를 수동으로 Mobile Foundation 서버에 업로드:
  1. 업로드하지 않고 .zip 파일을 빌드하십시오.
      ```bash
      mfpdev app webupdate --build
      ```
      {: pre}
  2. Mobile Foundation Operations 콘솔을 로드하고 애플리케이션 항목을 클릭하십시오.
  3. **웹 리소스 파일 업로드**를 클릭하여 패키지된 웹 리소스를 업로드하십시오.    
      ![콘솔에서 직접 업데이트 .zip 파일 업로드](images/upload-direct-update-package.png)

자세히 알아보려면 `mfpdev help app webupdate` 명령을 실행하십시오.
{: tip}
