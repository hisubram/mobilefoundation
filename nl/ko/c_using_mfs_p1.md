---

copyright:
  years: 2016, 2018
lastupdated:  "2018-03-07"

---

#	Developer 플랜 사용
{: #using_mobilefoundation_p1}

Developer 플랜을 사용하여 {{site.data.keyword.mobilefoundation_short}} 서비스 인스턴스를 작성한 후에 {{site.data.keyword.Bluemix_notm}}의 개요 페이지에 액세스하십시오. 이 페이지에서 서비스를 시작하는 데 도움이 되는 튜토리얼과 동영상이 제공됩니다.

## MobileFirst 서버에 대한 작업
{: #start_mobilefoundation_p1}
* MobileFirst 서버에 즉시 액세스하여 작업할 수 있습니다.

  이 선택은 다음 설정으로 {{site.data.keyword.mfserver_long_notm}}를 프로비저닝합니다.
  *	1GB의 메모리. 이 크기는 개발, 간단한 테스트 활동 및 소규모 프로덕션 워크로드에 충분합니다.

  * CLI를 사용하여 MobileFirst 서버에 액세스하려면 IBM Cloud 콘솔의 왼쪽 탐색 분할창에서 **서비스 신임 정보**를 클릭할 때 사용 가능한 신임 정보가 필요합니다.

<!--  The process of provisioning starts. This process takes about 10 minutes, and a message window indicates the progress of this operation. When complete a dashboard is displayed where you can see:
    *	The status of your server that is running (state, size).

    *	The server route created for you. Use this route in your mobile application to connect to the {{site.data.keyword.mfserver_short_notm}}.

    *	Your personal `username` and `password` to access the {{site.data.keyword.mfp_oc_short_notm}}. The `password` is hidden. Click **Show Password** icon to visualize it.

*	Click **Launch Console** to launch the {{site.data.keyword.mfp_oc_short_notm}}.-->

이제 모바일 앱 및 모바일 디바이스를 관리하고 서버를 모바일 백엔드로 사용하며 푸시 알림을 전송하는 등의 작업을 수행할 수 있습니다.

## Mobile Analytics 서비스
{: #adding_analytics_server_dev}

Mobile Analytics 서버는 Mobile Foundation: Developer 플랜 서비스 인스턴스에 포함되고 사전 구성됩니다.

<!-- You can now monitor your mobile application on {{site.data.keyword.mobilefirst}} server by adding a Mobile Analytics service to the {{site.data.keyword.mobilefoundation_short}} service instance. Developer plan creates the Mobile Analytics service in a container group with a single node having 1 GB memory.

* Click **Add Analytics** to add the Mobile Analytics service to the {{site.data.keyword.mobilefoundation_short}} service instance.

  The process of provisioning starts. This process takes about 10 minutes, and a message window indicates the progress of this operation.  -->

* {{site.data.keyword.mfp_oc_short_notm}}에서 Mobile Analytics 서비스 콘솔을 실행하십시오.

* {{site.data.keyword.mfserver_short_notm}}와 Mobile Analytics 서비스 간에 싱글 사인온이 사용됩니다. Mobile Analytics 서비스는 {{site.data.keyword.mfserver_short_notm}}와 동일한 LTPA 키 및 사용자 신임 정보를 사용하여 구성됩니다. {{site.data.keyword.mfp_oc_short_notm}}에 로그인하는 데 사용한 `username` 및 `password`를 사용하여 Mobile Analytics 콘솔에 로그인할 수 있습니다.

Mobile Analytics에 대한 자세한 정보는 [MobileFirst Foundation Operational Analytics ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/analytics/){: new_window}를 참조하십시오.

> **참고:** {{site.data.keyword.mobilefoundation_short}} 서비스 인스턴스를 삭제하면 Mobile Analytics 서비스 인스턴스가 제거됩니다.

<!--##  Deleting Mobile Analytics service
{: #deleting_analytics_server_dev}

You can now delete the Mobile Analytics service that was added to the {{site.data.keyword.mobilefoundation_short}} service instance, from the {{site.data.keyword.mobilefoundation_short}} service dashboard.

* Click **Delete Analytics** to delete the  Mobile Analytics service that was added to the {{site.data.keyword.mobilefoundation_short}} service instance.

 Clicking **Delete Analytics** deletes the analytics server instance. The process of deleting analytics instance takes about 10 minutes. You can refresh the screen to view the updated status. Deletion of analytics instance reenables the **Add Analytics** button. If you choose to add the Mobile Analytics service again, you can click this button.


## Re-creating the MobileFirst server
{: #recreate_mobilefoundation_p1}

*	Click **Recreate** to re-create the server.

* This action stops your existing server and deletes the data. All the data in your mobile server is lost. A new server instance is created with an updated version, if available. This action takes a few minutes to complete.

##	Setting up advanced configuration
{: #using_mfs_advanced_p1}

Use the **Start Server with Advanced Configuration** from the `Overview` page to create the server with advanced or custom settings. You can also update the server settings to customize your server configuration by clicking the **Configuration** tab. {{site.data.keyword.mobilefoundation_short}} gives you access to some advanced settings.

*	From the **Topology** tab, you can select the server size and the number of instances you need. The default 1 GB server is enough for development and moderate testing.

  - Select the correct size for your server based on your need.

* **Nodes** displays the number of nodes that are created. This field is not editable in {{site.data.keyword.mobilefoundation_short}}: Developer. The number of nodes is defaulted to **1** in the Developer plan.-->

세부사항은 [{{site.data.keyword.mobilefoundation_long}} 문서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window}를 참조하십시오.
