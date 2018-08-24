---

copyright:
  years: 2017,2018
lastupdated: "2017-10-16"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 백업
{: #backups}

서비스 대시보드의 _관리_ 페이지에 있는 _백업_ 탭에서 백업을 작성하고 다운로드할 수 있습니다. 일별, 주별, 월별 및 요청 시 백업이 사용 가능합니다. 이들은 다음 스케줄에 따라 보존됩니다.

백업 유형|보존 스케줄
----------|-----------
일별|일별 백업은 7일 간 보존됨
주별|주별 백업은 4주 간 보존됨
월별|월별 백업은 3개월 간 보존됨
요청 시|1회의 요청 시 백업이 보존됩니다. 보존된 백업은 항상 최신 요청 시 백업입니다.
{: caption="표 1. 백업 보존 스케줄" caption-side="top"}

백업 스케줄 및 보존 정책은 고정되어 있습니다. 보존 스케줄이 허용하는 것보다 더 많은 백업을 보존해야 하는 경우에는 자신의 비즈니스 요구사항에 따라 백업을 다운로드하고 아카이브를 보존해야 합니다.

## 기존 백업 보기

데이터베이스의 일간 백업이 자동으로 스케줄됩니다. 기존 백업을 보려면 다음을 수행하십시오.

1. 서비스 대시보드의 _관리_ 페이지로 이동하십시오.
2. 탭에서 **백업**을 클릭하여 _백업_ 페이지를 여십시오. 사용 가능한 백업 목록이 표시되며, 최근 백업이 목록의 맨 위에 있습니다.

  ![사용 가능한 백업](./images/janusgraph-backups-show.png "보류 중인 백업이 포함된 사용 가능한 백업 목록")

해당 행을 클릭하여 사용 가능한 백업에 대한 옵션을 펼치십시오.
  ![백업 옵션](./images/janusgraph-backups-options.png "백업에 대한 옵션") 

### API를 사용하여 기존 백업 보기

백업 목록은 `GET /2016-07/deployments/:id/backups` 엔드포인트에서 사용 가능합니다. 서비스 인스턴스 ID와 배치 ID가 모두 포함된 기반 엔드포인트가 서비스의 _개요_에 표시됩니다. 예를 들어, 다음과 같습니다. 
``` 
https://composebroker-dashboard-public.mybluemix.net/api/2016-07/instances/$INSTANCE_ID/deployments/$DEPLOYMENT_ID/backups
```  

## 수동 백업 작성

스케줄된 백업은 물론 수동으로 백업을 작성할 수도 있습니다. 수동 백업을 작성하려면 단계에 따라 기존 백업을 확인한 후 사용 가능한 백업 목록 위에 있는 **지금 백업**을 클릭하십시오. 백업이 시작되었음을 알리는 메시지가 표시되고 '보류 중인' 백업이 사용 가능한 백업 목록에 추가됩니다.

### API를 사용하여 백업 작성

`POST /2016-07/deployments/:id/backups`를 통해 백업 엔드포인트에 POST 요청을 전송하여 수동 백업을 시작하십시오. 실행 중인 백업에 대한 정보와 레시피 ID를 즉시 리턴합니다. 백업 엔드포인트를 확인하여 백업이 완료되었는지 알아보고 백업을 사용하기 전에 해당 backup_id를 찾아야 합니다. `GET /2016-07/deployments/:id/backups/`를 사용하십시오.

## 백업 복원

백업을 새 서비스 인스턴스에 복원하려면 단계에 따라 기존 백업을 확인한 후 해당 행을 클릭하여 다운로드할 백업에 대한 옵션을 펼치십시오. **복원** 단추를 클릭하십시오. 복원이 시작되었음을 알리는 메시지가 표시됩니다. 새 서비스 인스턴스가 자동으로 "janusgraph-restore-[timestamp]"로 이름 지정되고 프로비저닝이 시작될 때 대시보드에 표시됩니다.

### {{site.data.keyword.cloud_notm}} CLI를 통해 복원

{{site.data.keyword.cloud_notm}} CLI를 사용하여 실행 중인 JanusGraph 서비스의 백업을 새 JanusGraph 서비스에 복원하려면 다음 단계를 사용하십시오. 
1. 필요한 경우 [이를 다운로드하여 설치](https://console.{DomainName}/docs/cli/index.html#overview)하십시오. 
2. 서비스에 대한 _백업_ 페이지에서 복원하려는 백업을 찾아 백업 ID를 복사하십시오.  
  **또는**  
  `GET /2016-07/deployments/:id/backups`를 사용하여 Compose API를 통해 백업 및 해당 ID를 찾으십시오. 기반 엔드포인트와 서비스 인스턴스 ID가 모두 서비스의 _개요_에 표시됩니다. 예를 들어, 다음과 같습니다. 
  ``` 
  https://composebroker-dashboard-public.mybluemix.net/api/2016-07/instances/$INSTANCE_ID/deployments/$DEPLOYMENT_ID/backups
  ```  
  응답에는 해당 서비스 인스턴스에 대해 사용 가능한 모든 백업의 목록이 포함됩니다. 복원할 백업을 선택하고 해당 백업의 ID를 복사하십시오.

3. 적절한 계정과 신임 정보로 로그인하십시오. `ibmcloud login`(또는 모든 로그인 옵션을 보려면 `ibmcloud login -help`)

4. 조직 및 영역으로 전환하십시오. `ibmcloud target -o "$YOUR_ORG" -s "YOUR_SPACE"`

5. `service create` 명령을 사용하여 새 서비스를 프로비저닝하고 소스 서비스 및 JSON 오브젝트에 복원할 특정 백업을 제공하십시오. 예를 들어, 다음과 같습니다.
``` 
  ibmcloud service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": "$BACKUP_ID" }'
  ```
  _SERVICE_에는 `compose-for-janusgraph` 값을 사용하고 _PLAN_에는 환경에 따라 Standard 또는 Enterprise를 사용하십시오. _SERVICE\_INSTANCE\_NAME_은 새 서비스의 이름을 배치할 위치입니다. _source\_service\_instance\_id_는 백업 소스의 서비스 인스턴스 ID이며 `ibmcloud cf service DISPLAY_NAME --guid`를 실행하여 얻을 수 있습니다. 여기서 _DISPLAY\_NAME_은 백업이 생성된 JanusGraph 서비스의 이름입니다. 
  
  Enterprise 사용자인 경우 `"cluster_id": "$CLUSTER_ID"` 매개변수를 사용하여 JSON 오브젝트에 배치할 클러스터도 지정해야 합니다.
  
### 새 버전으로 업그레이드

일부 주 버전 업그레이드는 현재 실행 중인 배치에서 사용할 수 없습니다. 업그레이드된 버전을 실행 중인 새 서비스를 프로비저닝한 후 백업을 사용하여 데이터를 해당 서비스로 마이그레이션해야 합니다. 이 프로세스는 업그레이드하려는 버전을 지정한다는 점을 제외하고는 백업을 복원하는 것과 동일합니다.

``` 
bimcloud service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": ""$BACKUP_ID", "db_version":"$VERSION_NUMBER" }'
```

예를 들어, 이전 버전의 {{site.data.keyword.composeForJanusGraph}} 서비스를 JanusGraph 0.1.1이 실행되는 새 서비스로 복원하는 방법은 다음과 같습니다.
```
ibmcloud service create compose-for-janusgraph Standard migrated_janusgraph -c '{ "source_service_instance_id": "0269e284-dcac-4618-89a7-f79e3f1cea6a", "backup_id":"5a96d8a7e16c090018884566", "db_version":"0.1.1"  }'
