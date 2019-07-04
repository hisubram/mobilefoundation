---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: obfuscating resources, security

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}

# 리소스 난독화
{: #obfuscate_resources}

난독화는 더 이상 해커에게 유용하지 않지만 계속 완벽하게 작동하도록 실행 파일을 수정하는 프로세스입니다. 자동화된 코드 난독화를 사용하면 프로그램의 리버스 엔지니어링이 어렵게 됩니다.

난독화를 통해 애플리케이션의 리버스 엔지니어링이 더욱 어려워지며 거래 본인확인정보(지적 재산) 도난, 무단 액세스, 라이센싱 우회 또는 기타 제어 및 취약성으로부터 보호합니다.

코드 난독화는 여러 다른 기술과 애플리케이션 보안 기술로 구성됩니다.

* 이름 바꾸기 난독화 - 이름 바꾸기를 수행하면 메소드와 변수의 이름이 변경되고 사용자가 역컴파일(decompile)된 소스를 이해하기가 어려워지지만 애플리케이션 실행은 변경하지 않습니다. .NET, iOS, Java 및 Android obfuscator를 사용하여 난독화하는 데 가장 선호되는 방법은 다음과 같습니다.
* 문자열 암호화
* 제어 플로우 난독화
* 지시사항 패턴 변환
* 더미 코드 삽입
* 변조 방지 등

난독화를 사용하면 공격자가 코드를 검토하고 애플리케이션을 분석하기가 훨씬 어렵습니다. 난독화를 위해 다양한 서드파티 도구를 사용할 수 있습니다.

* Android 애플리케이션 난독화의 경우 이 [블로그](https://mobilefirstplatform.ibmcloud.com/blog/2016/09/19/mfp-80-obfuscating-android-code-with-proguard/)를 참조하십시오.

  MobileFirst Android 애플리케이션의 Android ProGuard 난독화에 사용할 구성 파일(proguard-project.txt)을 작성해야 합니다.
  {: note}

* Cordova 애플리케이션의 기본 난독화는 [Cordova 애플리케이션 암호화](#encryptingcordovapackage)를 참조하십시오.

## Cordova 패키지의 웹 리소스 암호화
{: #encryptingcordovapackage}

`.apk` 또는 `.ipa` 패키지에 있는 동안 다른 사용자가 웹 리소스를 보고 수정하는 위험을 최소화하려면 다음 MobileFirst CLI 명령
```bash
mfpdev app webencrypt
```
{: codeblock}
또는 `mfpwebencrypt` 플래그를 사용하여 정보를 암호화할 수 있습니다. 이 프로시저에서는 해독 불가능한 암호화를 제공하는 것은 아니지만 기본 수준의 난독화는 제공합니다.

### 전제조건

* Cordova 개발 도구를 설치해야 합니다. 이 예에서는 Apache Cordova CLI를 사용합니다. 다른 Cordova 개발 도구를 사용하는 경우 단계 중 일부가 다릅니다. 지시사항은 Cordova 도구 문서를 참조하십시오.
* MobileFirst CLI를 설치해야 합니다.
* MobileFirst Cordova 플러그인을 설치해야 합니다.

이 프로시저는 앱 개발을 마친 후 앱을 배치할 준비가 되었을 때 완료하는 것이 가장 좋습니다. 웹 리소스 암호화 프로시저를 완료한 후 다음 명령을 실행하면 암호화된 컨텐츠가 복호화됩니다.

* `cordova prepare`
* `cordova build`
* `cordova run`
* `cordova emulate`
* `mfpdev app webupdate
`
* `mfpdev app preview`

웹 리소스를 암호화한 후 나열된 명령 중 하나를 실행하는 경우, 이 프로시저를 다시 완료하여 웹 리소스를 암호화해야 합니다.

1. 터미널 창을 열고 암호화할 Cordova 앱의 루트 디렉토리로 이동하십시오.
2. 다음 명령 중 하나를 입력하여 앱을 준비하십시오.
    ```bash
      cordova prepare
    ```
    {: codeblock}
    ```bash
      mfpdev app webupdate
    ```
    {: codeblock}
3. 다음 프로시저 중 하나를 완료하여 컨텐츠를 암호화하십시오.
    * 다음 명령을 입력하십시오.
      ```bash
      mfpdev app webencrypt
      ```
      {: codeblock}

      다음을 입력하여 `mfpdev app webencrypt` 명령에 대한 정보를 볼 수 있습니다. 
      ```bash
      mfpdev help app webencrypt
      ```
      {: codeblock}
      {: tip}

    * 패키지를 빌드할 때 `mfpwebencrypt` 플래그를 `cordova compile` 또는 `cordova build` 명령에 추가하여 Cordova 패키지의 웹 리소스를 암호화할 수도 있습니다.
        ```bash
         cordova compile -- --mfpwebencrypt
         ```
         {: codeblock}
         또는
         ```bash
         cordova build -- --mfpwebencrypt
         ```
         {: codeblock}
         **www** 폴더의 운영 체제 정보가 암호화된 컨텐츠를 포함하는 **resources.zip** 파일로 대체됩니다.
         Android 운영 체제용 앱이고 **resources.zip** 파일 크기가 1MB를 초과하는 경우 **resources.zip** 파일은 이름이 **resources.zip.nnn**이고 크기가 768KB로 줄어든 여러 개의 .zip 파일로 분할됩니다. nnn 변수는 001에서 999 사이의 숫자입니다.
4. 플랫폼별 도구와 함께 제공되는 에뮬레이터를 사용하여 암호화된 자원이 있는 애플리케이션을 테스트합니다. 예를 들어, Android에는 Android Studio의 에뮬레이터를 사용하고 iOS에는 Xcode를 사용합니다.

암호화되어 있는 애플리케이션을 테스트하는 경우 다음 Cordova 명령을 사용하지 마십시오.
```bash
cordova run
```
{: codeblock}
```bash
cordova emulate
```
{: codeblock}
이러한 명령은 WWW 폴더 내의 암호화된 컨텐츠를 새로 고치고 이를 복호화된 컨텐츠로 다시 저장합니다. 이 명령을 사용하는 경우, 앱을 공개하기 전에 암호화하는 프로시저를 다시 완료해야 합니다.
{: note}
