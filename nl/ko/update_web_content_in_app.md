---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: update web content, update apps

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# 직접 업데이트
{: #direct_update}

사용자가 App Store 또는 Play Store 업데이트를 수행할 필요 없이 **직접 업데이트**를 사용하여 Cordova 또는 Ionic 애플리케이션의 웹 리소스(JavaScript, HTML, CSS 또는 이미지 파일)를 OTA(Over-The-Air)로 업데이트할 수 있습니다. 직접 업데이트 기능을 사용하면 비즈니스에서 일반 사용자가 최신 버전의 앱을 사용하는지 확인할 수 있습니다. 애플리케이션을 업데이트하려면 Mobile Foundation CLI를 사용하거나 생성된 아카이브 파일을 배치하여 애플리케이션의 업데이트된 웹 리소스를 패키지하고 Mobile Foundation 인스턴스에 업로드해야 합니다. 그러면 직접 업데이트가 자동으로 활성화됩니다. 이후 보호된 리소스에 대한 모든 요청에서 적용됩니다.

![직접 업데이트 작업 방식 다이어그램](images/internal_function.jpg)

직접 업데이트는 Cordova iOS 및 Cordova Android 플랫폼에서 지원됩니다.
{: note}

실시간 프로덕션 또는 사전 프로덕션 테스트 단계의 경우에는 애플리케이션을 앱 스토어에 공개하기 전에 보안 직접 업데이트를 구현하도록 권장됩니다. 보안 직접 업데이트를 사용하려면 실제 CA 서명 서버 인증서에서 추출된 RSA 키 쌍이 필요합니다.

1. 애플리케이션이 공개된 후 키 저장소 구성을 수정하지 않도록 주의하십시오. 새 공개 키로 애플리케이션을 재구성하고 애플리케이션을 다시 공개하기 전에는 다운로드된 업데이트를 인증할 수 없습니다. 앞의 두 단계를 수행하지 않으면 클라이언트에서 직접 업데이트가 실패할 수 있습니다.
2. 직접 업데이트는 애플리케이션의 웹 리소스만 업데이트합니다. 네이티브 리소스를 업데이트하려면 새 애플리케이션 버전을 각 앱 스토어에 제출해야 합니다.
3. 직접 업데이트 기능을 사용하고 웹 리소스 체크섬 기능이 사용으로 설정되면 각 직접 업데이트에 대해 새 체크섬 기준이 설정됩니다.
4. Mobile Foundation 서버가 업그레이드된 경우 직접 업데이트를 계속 올바르게 제공합니다. 그러나 최근에 빌드된 직접 업데이트 아카이브(.zip 파일)가 업로드되는 경우 이전 클라이언트에 대한 업데이트가 정지될 수 있습니다. 아카이브에 `cordova-plugin-mfp` 플러그인의 버전이 포함되어 있기 때문입니다. 서버가 해당 아카이브를 모바일 클라이언트에 제공하기 전에 클라이언트 버전을 플러그인 버전과 비교합니다. 두 버전이 충분히 비슷하면(즉, 가장 중요한 3개의 숫자가 동일한 경우) 직접 업데이트가 정상적으로 수행됩니다. 그렇지 않으면 Mobile Foundation가 업데이트를 자동으로 건너뜁니다. 버전 불일치에 대한 한 가지 솔루션은 원래 Cordova 프로젝트의 버전과 동일한 버전인 `cordova-plugin-mfp`를 다운로드하고 직접 업데이트 아카이브를 재생성하는 것입니다.
{: tip}

직접 업데이트 후 애플리케이션에서 사전 패키지된 웹 리소스를 더 이상 사용하지 않습니다. 대신 애플리케이션의 샌드박스에서 다운로드된 웹 리소스를 사용합니다. 디바이스에 있는 애플리케이션의 캐시가 지워지면 원래 패키지된 웹 리소스가 다시 사용됩니다.

웹 리소스의 직접 업데이트는 특정 버전의 앱에만 적용됩니다. 예를 들어, 애플리케이션 버전 2.0에 대해 생성된 업데이트가 동일한 앱의 버전 2.1에 전달되지 않습니다.
{: note}

## 업데이트된 웹 리소스 작성 및 배치
{: #creating_deploying_updates}

웹 리소스를 변경한 후 Mobile Foundation CLI를 사용하여 패키지한 후 Mobile Foundation 인스턴스에 업로드할 수 있습니다.

1.  직접 업데이트는 애플리케이션의 웹 리소스만 업데이트합니다. 네이티브 리소스를 업데이트하려면 새 애플리케이션 버전을 각 앱 스토어에 제출해야 합니다.
2. 앱의 네이티브 파트가 Mobile Foundation 서비스에 업로드된 파트와 크게 다른 경우 Mobile Foundation 서비스가 클라이언트에 대한 직접 업데이트를 정지합니다. 이는 *cordova-plugin-mfp*가 새 버전의 플러그인으로 업데이트될 때 발생할 수 있습니다. 서버가 아카이브를 모바일 클라이언트에 제공하기 전에 클라이언트 버전을 플러그인 버전과 비교합니다. 두 버전이 충분히 비슷하면(즉, 버전 ID의 가장 중요한 3개의 숫자가 동일한 경우) 직접 업데이트가 정상적으로 수행됩니다. 그렇지 않으면 Mobile Foundation가 업데이트를 자동으로 건너뜁니다. 버전 불일치에 대한 한 가지 솔루션은 원래 Cordova 프로젝트의 버전과 동일한 버전인 *cordova-plugin-mfp*를 다운로드하고 직접 업데이트 아카이브를 재생성하는 것입니다.
{: tip}

1. 명령행 창을 열고 Cordova 프로젝트의 루트로 이동하십시오.
2. 다음 명령을 실행하십시오.
  ```bash
  mfpdev app webupdate
  ```
`mfpdev app webupdate` 명령은 업데이트된 웹 리소스를 `.zip` 파일로 패키지하여 개발자 워크스테이션에서 실행 중인 기본 Mobile Foundation 서버에 업로드합니다. 패키지된 웹 리소스는 `[cordova-project-root-folder]/mobilefirst/` 폴더에 있습니다.

앱의 웹 컨텐츠를 업데이트하는 대체 단계는 [여기](/docs/services/mobilefoundation?topic=mobilefoundation-alternate_steps_to_update_app_web_content_in_app#alternate_steps_to_update_app_web_content_in_app)를 참조하십시오.

## 직접 언데이트 고급 구성
{: #direct_update_advanced_config}

직접 업데이트 구성에 대한 고급 주제는 [여기](/docs/services/mobilefoundation?topic=mobilefoundation-advanced_direct_update_configuration#advanced_direct_update_configuration)를 참조하십시오.
