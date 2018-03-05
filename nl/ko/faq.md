---

copyright:
  years: 2018
lastupdated:  "2018-02-09"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}


# 자주 묻는 질문(FAQ)

이 FAQ에서는 {{site.data.keyword.mobilefoundation_long}} 서비스에 대한 일반적인 질문에 대한 답을 제공합니다.
{: shortdesc}

## {{site.data.keyword.mobilefoundation_short}} 서비스에 대한 업데이트를 사용 가능한 시기를 어떻게 알 수 있습니까?
{: #maintupdates_mf}

{{site.data.keyword.mobilefoundation_short}}에서는 {{site.data.keyword.mfserver_short_notm}}를 프로비저닝합니다. {{site.data.keyword.mobilefoundation_short}} 서버에 대한 업데이트가 사용자에게 알림으로 전달됩니다. 알림이 서비스 인스턴스 대시보드에 표시됩니다. 사용자가 지정한 유지보수 시간대에 {{site.data.keyword.mobilefoundation_short}}에 대한 업데이트를 적용하도록 선택할 수 있습니다. 사용자가 편한 시간에 {{site.data.keyword.mobilefoundation_short}} 서버를 업데이트하도록 선택할 수 있습니다.

{{site.data.keyword.mobilefoundation_short}} 서비스 업데이트는 다음 컴포넌트 중 하나가 업데이트되는 경우 사용 가능하게 됩니다.

* {{site.data.keyword.mfserver_short_notm}}.
* 기본 Liberty 버전.
* 기본 Java Developer Kit 버전.

## {{site.data.keyword.mobilefoundation_short}} 서비스에 대한 업데이트를 어떻게 적용합니까?
{: #apply_update_mf}

**재작성**을 클릭하여 {{site.data.keyword.mobilefoundation_short}}에 대한 업데이트를 적용할 수 있습니다.
업데이트 적용 시 {{site.data.keyword.mfp_oc_short_notm}}에 표시되는 대로 서버 업데이트 버전을 표시하도록 서버 버전이 수정됩니다.

> **참고:**
>  * 사용자는 고유 수정사항 및 업데이트를 {{site.data.keyword.mobilefoundation_short}} 서비스 인스턴스에 적용할 수 없습니다.
>  * **재작성**을 클릭하는 경우 플랜에서 발생하는 동작의 차이점을 파악하려면 [Developer 플랜에서 서버 재작성](c_using_mfs_p1.html#recreate_mobilefoundation_p1), [DeveloperPro 플랜에서 서버 재작성](c_using_mfs_p3.html#recreate_mobilefoundation_p3), [Professional Per Capacity 플랜에서 서버 재작성](c_using_mfs_p4.html#recreate_mobilefoundation_p4) 및 [Professional 1 Application 플랜에서 서버 재작성](c_using_mfs_p2.html#recreate_mobilefoundation_p2)을 참조하십시오.
>

## 내 {{site.data.keyword.mobilefoundation_short}} 서버 인스턴스에 대해 어떻게 사용자 정의 도메인을 구성합니까?
{: #configcustomdomain}

{{site.data.keyword.mobilefoundation_short}}에서는 {{site.data.keyword.Bluemix_notm}} **지역**을 기반으로 하는 도메인 이름이 있는 URL을 사용하여 액세스 가능한 {{site.data.keyword.mfserver_short_notm}}를 프로비저닝합니다. 사용자 자신의 사용자 정의 도메인을 구성할 수도 있습니다.

URL 또는 라우트는 {{site.data.keyword.Bluemix_notm}} `Region`을 기반으로 하는 기본 도메인 이름을 사용하여 작성됩니다.

  |도메인 |  지역  |    
  |:----- | :----- |    
  |`mybluemix.net` | 미국 남부 |    
  |`eu-gb.mybluemix.net` | 영국  |
  |`au-syd.mybluemix.net` | 시드니  |   
  |`eu-de.mybluemix.net` | 프랑크푸르트 |   
  {: caption="표 1. {{site.data.keyword.Bluemix_notm}}에서 지역을 기반으로 하는 애플리케이션 도메인 이름" caption-side="top"}

자체 도메인을 사용할 수 있으려면 다음 단계를 수행하여 사용자 정의 도메인을 구성해야 합니다.

1.	지원되는 플랜 중 하나를 선택하고 {{site.data.keyword.mobilefoundation_short}} 서비스 인스턴스를 작성하여 {{site.data.keyword.mfserver_short_notm}} 인스턴스를 작성하십시오.

+ 사용하려는 사용자 정의 도메인을 사용자의 {{site.data.keyword.Bluemix_notm}} `Organization`에 추가하십시오. **조직 관리 > 도메인 > 도메인 추가**로 이동하여 자체 도메인을 추가하십시오.

+ 사용자 정의 도메인을 사용하도록 <!--container group--> 서버의 라우트를 설정하십시오.

+ 도메인의 DNS 제공자로 이동하여 CNAME 항목을 추가하십시오. 그러면 사용자 도메인에서 <!--container group--> 서버가 실행 중인 기본 {{site.data.keyword.Bluemix_notm}} 라우트로 트래픽이 라우팅됩니다.

+ 사용자 정의 도메인에 대해 `https`를 구성하려면 {{site.data.keyword.Bluemix_notm}}의 도메인에 대한 SSL 인증서를 업로드하십시오. 이를 위해 **조직 관리 > 도메인**으로 이동하여 SSL 인증서를 구성하려는 사용자 정의 도메인을 선택하고 **인증서 업로드**를 클릭하여 도메인에 대한 SSL 인증서를 업로드하십시오. 자세한 정보는 [이 포스트 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/bluemix/2014/09/28/ssl-certificates-bluemix-custom-domains/){: new_window}를 참조하십시오.
