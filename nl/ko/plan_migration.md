---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: mobile foundation plans, migration of plans

subcollection:  mobilefoundation
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:  .screen}
{:codeblock:  .codeblock}
{:tip: .tip}
{:note: .note}

# Mobile Foundation 서비스 플랜 마이그레이션

더 이상 사용되지 않는 플랜을 사용하여 작성된 Mobile Foundation 인스턴스를 새 플랜으로 업데이트해야 합니다. 인스턴스 사용을 기반으로 하는 플랜 업데이트가 필요할 수도 있습니다.
{: shortdesc}

## 샘플 시나리오: Professional Per Device 플랜에서 Professional 1 Application 대상으로 마이그레이션

1. IBM Cloud 대시보드에서 마이그레이션할 IBM Mobile Foundation 인스턴스를 선택하십시오.
2. 탐색에서 **플랜**을 선택하십시오.
   ![기존 Mobile Foundation 플랜](images/existing-plan.png)
3. 나열된 가격 플랜에서 Professional 1 Application을 선택하십시오.
   ![새 Mobile Foundation 플랜](images/new-plan.png)
4. **저장** 단추를 클릭하고 플랜 마이그레이션을 확인하십시오.
     이제 Professional 1 Application으로의 마이그레이션이 완료되고 기존 데이터가 모두 유지됩니다. 청구가 변경되고 작동 중단이 없습니다.
5. 플랜 마이그레이션 후 올바른 구성이 적용되려면 서비스 대시보드에서 Mobile Foundation 인스턴스를 재작성해야 합니다. 이 업데이트를 수행하려면 잠시 작동이 중단되어야 합니다. 작동 중단을 계획해야 합니다. 탐색에서 **관리**를 선택하고 **재작성**을 클릭하십시오.

더 이상 사용되지 않는 플랜 중 하나를 사용 중인 경우 새 플랜으로 마이그레이션해야 합니다.
{: note}

## 지원되는 플랜 마이그레이션

* *Developer*(더 이상 사용되지 않음) 플랜은 새 *Developer* 플랜으로만 업데이트할 수 있습니다.
* *Developer Pro*(더 이상 사용되지 않음) 플랜은 *Professional Per Device* 또는 *Professional 1 Application* 플랜으로만 업데이트할 수 있습니다.
* *Professional Per Capacity*(더 이상 사용되지 않음) 플랜은 *Professional Per Device* 또는 *Professional 1 Application* 플랜으로만 업데이트할 수 있습니다.
* *Professional Per Device* 플랜은 *Professional 1 Application* 플랜으로만 업데이트할 수 있습니다.
* *Professional 1 Application* 플랜은 *Professional Per Device* 플랜으로만 업데이트할 수 있습니다.
* 새 *Developer* 플랜에 대한 플랜 업데이트는 지원되지 않습니다.
