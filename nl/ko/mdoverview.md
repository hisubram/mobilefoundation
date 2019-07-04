---

copyright:
  years: 2016, 2019
lastupdated: "2019-06-06"

keywords: mobile foundation, features, overview

subcollection:  mobilefoundation
---

#	개요
{: #overview_mobilefoundation}

{{site.data.keyword.mobilefoundation_short}}은 모바일, 웹 및 PWA(Progressive Web Apps)에 대한 통합된 백엔드 기능 세트를 제공합니다. 개발자는 자신이 선택한 프론트 엔드 도구 또는 프레임워크를 사용하고 {{site.data.keyword.mobilefoundation_short}} 서비스에서 제공되는 다양한 백엔드 세트를 활용하도록 선택할 수 있습니다. {{site.data.keyword.mobilefoundation_short}} SDK는 Cordova, iOS, Android, Xamarin, Windows 10, React Native 및 모바일 웹에 사용할 수 있습니다.

## {{site.data.keyword.mobilefoundation_short}}에서 제공되는 주요 기능

### 사용자 참여
{: #user_engagement}

사용자 참여를 유도하는 모바일 앱 빌드는 모든 비즈니스의 앱 전략 성공을 위한 핵심 요소입니다. 푸시 알림을 사용하여 사용자 참여를 앱에 빌드할 수 있습니다. {{site.data.keyword.mobilefoundation_short}}은 iOS, Android 및 Windows 10 디바이스에 알림을 전송하도록 지원합니다. 모든 사용자 또는 사용자 서브세트에 알림을 전송하거나 관심 분야를 기반으로 사용자에게 알림을 전송할 수 있습니다. 관련 플랫폼에 대한 대화식 자동 알림이 지원됩니다. 개인화는 사용자 참여의 핵심 측면입니다. **라이브 업데이트** 기능을 사용하여 사용자를 세그먼트화하고 특정 사용자 세그먼트에 대한 앱을 사용자 조정할 수 있습니다.

###  앱 인사이트
{: #app_insights}

사용자가 계속 적절하게 적극적으로 참여하게 하려면 사용자의 애플리케이션 수행 방식을 파악해야 합니다.   Mobile Foundation의 Mobile Analytics 기능에서는 기본 제공된 시각화(차트 및 테이블)를 통해 이 기능을 제공합니다.  애플리케이션을 최소한으로 인스트루먼트하여 Mobile Analytics 콘솔에서 다음과 같은 실행 가능한 인사이트를 쉽게 시각화할 수 있습니다.
- **사용자 합류 패턴**: 새로 합류한 사용자가 있습니까? 기존 사용자 중 애플리케이션에 돌아온 사용자가 있습니까?
- **사용 패턴**: 애플리케이션이 가장 많이 사용되는 시간과 가장 적게 사용되는 시간은 언제입니까? 사용 시간과 관련하여 비즈니스 관련성을 충족시킵니까?
- **백엔드 성능**: 백엔드 시스템 중 가장 많이 사용되는 기능은 무엇이며 해당 응답 시간과 안정성은 어떻습니까? 백엔드를 재조정해야 합니까?
- **애플리케이션 안정성**: 시간 경과에 따른 애플리케이션 안정성은 어떻습니까? 충돌이 있었다면 원인은 무엇입니까(충돌 로그)? 애플리케이션 디자인/구현을 수정해야 합니까?
- **인앱(in-app) 사용자 경험**: 사용자가 앱을 사용할 때 실제로 경험하는 상호작용은 무엇이며, 사용자는 이 경험에 대해 어떻게 생각하고 있습니까? 사용자 연구를 재고해야 합니까?
- **사용자 정의 추적**: 사용자 정의 차트는 애플리케이션별 추적 및 플로우의 일부로 로깅된 사용자 정의 데이터를 중심으로 정의되어 구성되며 비즈니스 의사결정을 지원할 수 있는 고유 인사이트를 확장하고 정의할 유연성을 제공합니다.

###  보안 프레임워크
{: #security_framework}

광범위한 모바일 채널 특정 보안입니다. 디바이스의 데이터, API 및 사용자 브랜드를 해커와 취약성으로부터 보호합니다. 오프라인 데이터 암호화, 메시지 중간자 공격(Man-in-the-middle attack) 방지, 유실되거나 도난된 디바이스에 대한 액세스 거부, 권한 있는 오퍼레이션에 대한 추가 보안 설정, 기존 ID 관리 솔루션과의 통합, 리버스 엔지니어링 앱 코드로부터의 보호 등을 수행합니다.

###  비즈니스 로직
{: #business_logic}

백엔드 시스템에 연결하기 위한 서버 측 코드를 개발하고 실행합니다. Liberty, NodeJS 또는 Swift 런타임을 사용하는 마이크로서비스로 비즈니스 로직을 빌드합니다. 해당 Swagger 정의를 가져와서 마이크로서비스에 액세스합니다.

###  앱 관리
{:  #app_management}

애플리케이션 버전을 관리하고 만료된 애플리케이션 버전을 차단합니다. 앱 스토어 승인을 건너뛰고 직접 웹 리소스를 변경합니다.

###  코그너티브 API
{:  #cognitive_apis}

모바일 앱에서 Watson API(*Tone Analyzer, Language Translator, Discovery 또는 Conversation*)를 사용하여 스마트 앱을 빌드합니다. 이 통합은 모바일 채널 보안을 활용하고 API에 대한 인사이트를 얻는 데 도움이 됩니다.
