# DataDog 기본 모니터링 Import & JSON 목록

## 1. DataDog 모니터링 Alert Import

0. https://app.datadoghq.com 접속 후,  원하는 **Organization** 내부로 접속합니다.
![00](https://github.com/AhchimLee/memo/raw/main/import_mnt_00.png)

1. **Monitors > Manage Monitors** 접근 후 **New Monitor**를 클릭합니다.
![01](https://github.com/AhchimLee/memo/raw/main/import_mnt_01.png)

2. Monitor type 중 **Import Monitor from JSON**을 클릭합니다.
![02](https://github.com/AhchimLee/memo/raw/main/import_mnt_02.png)

3. 하기 `모니터링 정책 목록 (JSON)` 내용 중 하나를 선택하여 복사 붙여넣기합니다.
![03](https://github.com/AhchimLee/memo/raw/main/import_mnt_03.png)

4. Save 후 모니터링 Alert 내용을 수정합니다. (메트릭 값, 알람 수신자 등)
![04-1](https://github.com/AhchimLee/memo/raw/main/import_mnt_04_1.png)
![04-2](https://github.com/AhchimLee/memo/raw/main/import_mnt_04_2.png)

5. Save 클릭 후 생성된 모니터링은 목록에서 확인 가능합니다. 
![05](https://github.com/AhchimLee/memo/raw/main/import_mnt_05.png)

## 2. 모니터링 정책 목록 (JSON)

## CPU Used % >=

EC2 연동 방식
```json
{
	"id": 33062997,
	"name": "[Company] CPU Used >= {{threshold}}% at {{name.name}}",
	"type": "metric alert",
	"query": "avg(last_5m):avg:aws.ec2.cpuutilization.maximum{*} by {name,instance_id,availability-zone,cloud_provider,instance-type,env,service,host} > 90",
	"message": "*Alarm   :  CPU Used >= {{threshold}}%, Current {{value}}%\n*Name   :  {{name.name}} ({{instance_id.name}}) / {{host.ip}}\n*AZ         :  {{cloud_provider.name}} > {{availability-zone.name}}\n*Service :  {{service.name}}  >  {{env.name}}\n*Time(UTC):  {{last_triggered_at}}\n===============================   \n@ahchim.lee@bespinglobal.com @webhook-AlertNow",
	"tags": [],
	"options": {
		"notify_audit": false,
		"locked": false,
		"timeout_h": 0,
		"silenced": {},
		"include_tags": false,
		"no_data_timeframe": null,
		"require_full_window": false,
		"new_host_delay": 300,
		"notify_no_data": false,
		"renotify_interval": 0,
		"evaluation_delay": 900,
		"escalation_message": "",
		"thresholds": {
			"critical": 90
		}
	},
	"priority": null
}
```

Agent 방식
```json
{
	"id": 33237155,
	"name": "[Company] CPU Used >= {{threshold}}% at {{name.name}}",
	"type": "query alert",
	"query": "avg(last_5m):100 - avg:system.cpu.idle{*} by {name,cloud_provider,service,env,host} > 90",
	"message": "*Alarm   :  CPU Used >= {{threshold}}%, Current {{value}}%\n*Name   :  {{name.name}}  / {{host.ip}} \n*Region :  {{cloud_provider.name}}\n*Service :  {{service.name}}  >  {{env.name}}\n*Time(UTC):  {{last_triggered_at}}\n=============================== \n@ahchim.lee@bespinglobal.com @webhook-AlertNow",
	"tags": [],
	"options": {
		"notify_audit": false,
		"locked": false,
		"timeout_h": 0,
		"silenced": {},
		"include_tags": false,
		"no_data_timeframe": null,
		"require_full_window": false,
		"new_host_delay": 300,
		"notify_no_data": false,
		"renotify_interval": 0,
		"escalation_message": "",
		"thresholds": {
			"critical": 90
		}
	},
	"priority": null
}
```

## Disk Used % >=
```json
{
	"id": 33063048,
	"name": "[Company] Disk Used >= {{threshold}}% at {{name.name}}",
	"type": "query alert",
	"query": "avg(last_5m):avg:system.disk.in_use{!device_name:loop*,!device_name:*tmpfs,!device_name:*udev} by {name,availability-zone,cloud_provider,service,env,host,device} * 100 > 90",
	"message": "*Alarm   :  Disk Used >= {{threshold}}%, Current {{value}}%\n*Name   :  {{name.name}} ({{device.name}}) / {{host.name}} \n*Region :  {{cloud_provider.name}} > {{availability-zone.name}}\n*Service :  {{service.name}}  >  {{env.name}}\n*Time(UTC):  {{last_triggered_at}}\n=============================== \n@ahchim.lee@bespinglobal.com @webhook-AlertNow",
	"tags": [],
	"options": {
		"notify_audit": false,
		"locked": false,
		"timeout_h": 0,
		"silenced": {},
		"include_tags": false,
		"no_data_timeframe": null,
		"require_full_window": false,
		"new_host_delay": 300,
		"notify_no_data": false,
		"renotify_interval": 0,
		"escalation_message": "",
		"thresholds": {
			"critical": 90
		}
	},
	"priority": null
}
```

## Mem Used % >=
```json
{
	"id": 33063072,
	"name": "[Company] Memory Used >= {{threshold}}% at {{name.name}}",
	"type": "query alert",
	"query": "avg(last_1m):( avg:system.mem.total{*} by {name,availability-zone,cloud_provider,service,env,host} - avg:system.mem.usable{*} by {name,availability-zone,cloud_provider,service,env,host} ) / avg:system.mem.total{*} by {name,availability-zone,cloud_provider,service,env,host} * 100 > 90",
	"message": "*Alarm   :  Memory Used >= {{threshold}}%, Current {{value}}%\n*Name   :  {{name.name}} / {{host.name}} \n*Region :  {{cloud_provider.name}} > {{availability-zone.name}}\n*Service :  {{service.name}}  >  {{env.name}}\n*Time(UTC):  {{last_triggered_at}}\n=============================== \n@ahchim.lee@bespinglobal.com @webhook-AlertNow",
	"tags": [],
	"options": {
		"notify_audit": false,
		"locked": false,
		"timeout_h": 0,
		"silenced": {},
		"include_tags": false,
		"no_data_timeframe": null,
		"require_full_window": true,
		"new_host_delay": 300,
		"notify_no_data": false,
		"renotify_interval": 0,
		"escalation_message": "",
		"thresholds": {
			"critical": 90,
			"warning": 85
		}
	},
	"priority": null
}
```


## [Host] host_name is Down
AWS 연동 방식
```json
{
	"id": 33063164,
	"name": "[Company] EC2 {{name.name}} is Down",
	"type": "metric alert",
	"query": "avg(last_5m):avg:aws.ec2.host_ok{*} by {name,availability-zone,instance-type,cloud_provider,env,service,host} <= 0",
	"message": "*Alarm   :  {{name.name}} is Down ({{value}})\n*Name   :  {{name.name}} ({{instance-type.name}}) / {{host.name}}\n*Region :  {{cloud_provider.name}} > {{availability-zone.name}}\n*Service :  {{service.name}}  >  {{env.name}}\n*Time(UTC):  {{last_triggered_at}}\n=============================== \n@ahchim.lee@bespinglobal.com @webhook-AlertNow",
	"tags": [],
	"options": {
		"notify_audit": false,
		"locked": false,
		"timeout_h": 0,
		"new_host_delay": 300,
		"require_full_window": true,
		"notify_no_data": false,
		"renotify_interval": "0",
		"escalation_message": "",
		"no_data_timeframe": null,
		"include_tags": false,
		"thresholds": {
			"critical": 0
		}
	},
	"priority": null
}
```

Agent 방식
```json
{
	"id": 33533015,
	"name": "[Company] {{name.name}} Server Not Responding",
	"type": "query alert",
	"query": "avg(last_1m):default( exclude_null(avg:datadog.agent.running{*} by {name,cloud_provider,service,env,host}) , 0 ) <= 0",
	"message": "*Alarm   :  {{name.name}} Server Not Responding\n*Name   :  {{name.name}}  / {{host.ip}} \n*Region :  {{cloud_provider.name}}\n*Service :  {{service.name}}  >  {{env.name}}\n*Time(UTC):  {{last_triggered_at}}\n=============================== \n@ahchim.lee@bespinglobal.com @slack-SREDataDogTEMP-sre5-smb3 @slack-2ndZETDevOps-infra-announcements @webhook-AlertNow",
	"tags": [],
	"options": {
		"notify_audit": false,
		"locked": false,
		"timeout_h": 0,
		"new_host_delay": 300,
		"require_full_window": true,
		"notify_no_data": false,
		"renotify_interval": "0",
		"escalation_message": "",
		"no_data_timeframe": null,
		"include_tags": false,
		"thresholds": {
			"critical": 0
		}
	},
	"priority": null
}
```

## Uptime [time] below

- Aurora Only

```json
{
	"id": 33118252,
	"name": "[Company] Uptime {{threshold}} below at {{name.name}}",
	"type": "query alert",
	"query": "avg(last_5m):avg:system.uptime{*} by {name,availability-zone,cloud_provider,service,env,host} < 100",
	"message": "*Alarm   :  Uptime {{threshold}} below at {{name.name}}\n*Name   :  {{name.name}} ({{device.name}}) / {{host.name}} \n*Region :  {{cloud_provider.name}} > {{availability-zone.name}}\n*Service :  {{service.name}}  >  {{env.name}}\n*Time(UTC):  {{last_triggered_at}}\n=============================== \n@ahchim.lee@bespinglobal.com @webhook-AlertNow",
	"tags": [],
	"options": {
		"notify_audit": false,
		"locked": false,
		"timeout_h": 0,
		"new_host_delay": 300,
		"require_full_window": false,
		"notify_no_data": false,
		"renotify_interval": "0",
		"escalation_message": "",
		"no_data_timeframe": null,
		"include_tags": false,
		"thresholds": {
			"critical": 100
		}
	},
	"priority": null
}
```


## [ALB] Unhealthy Host Count >= 1

```json
{
	"id": 33108854,
	"name": "[Company] ALB Unhealthy Host Count >= {{threshold}} at {{name.name}}",
	"type": "metric alert",
	"query": "avg(last_1m):max:aws.applicationelb.un_healthy_host_count{*} by {name,availability-zone,host,targetgroup} >= 1",
	"message": "*Alarm   :  ALB Unhealthy Host Count >= {{threshold}}\n*Name   :  {{name.name}} / TG: {{targetgroup.name}} \n*Region :  {{cloud_provider.name}} > {{availability-zone.name}}\n*DNS      :  {{host.name}}\n*Time(UTC):  {{last_triggered_at}}\n===============================   \n@ahchim.lee@bespinglobal.com @webhook-AlertNow",
	"tags": [],
	"options": {
		"notify_audit": false,
		"locked": false,
		"timeout_h": 0,
		"silenced": {},
		"include_tags": false,
		"no_data_timeframe": null,
		"require_full_window": true,
		"new_host_delay": 300,
		"notify_no_data": false,
		"renotify_interval": 0,
		"escalation_message": "",
		"thresholds": {
			"critical": 1
		}
	},
	"priority": null
}
```

## [NLB] Healthy Host Count = 0

```json
{
	"id": 33109005,
	"name": "[Company] NLB Healthy Host Count = {{threshold}} at {{name.name}}",
	"type": "metric alert",
	"query": "min(last_1m):min:aws.networkelb.healthy_host_count{*} by {name,availability-zone,host,targetgroup} <= 0",
	"message": "*Alarm   :  NLB Healthy Host Count = {{threshold}}\n*Name   :  {{name.name}} / TG: {{targetgroup.name}} \n*Region :  aws > {{availability-zone.name}}\n*DNS      :  {{host.name}}\n*Time(UTC):  {{last_triggered_at}}\n===============================    \n@ahchim.lee@bespinglobal.com @webhook-AlertNow",
	"tags": [],
	"options": {
		"notify_audit": false,
		"locked": false,
		"timeout_h": 0,
		"new_host_delay": 300,
		"require_full_window": true,
		"notify_no_data": false,
		"renotify_interval": "0",
		"escalation_message": "",
		"no_data_timeframe": null,
		"include_tags": false,
		"thresholds": {
			"critical": 0
		}
	},
	"priority": null
}
```

## RDS Connection >=

```json
{
	"id": 33114695,
	"name": "[Company] RDS Connection >= {{threshold}} at {{name.name}}",
	"type": "metric alert",
	"query": "avg(last_1m):avg:aws.rds.database_connections{*} by {name,engine,host} >= 500",
	"message": "*Alarm                 :  RDS Connection >= {{threshold}}\n*Current              : {{value}}\n*ClusterName    :  {{name.name}} ({{engine.name}}) \n*DBName           : {{host.dbname}} ({{host.name}})\n*Time(UTC)        :  {{last_triggered_at}}\n===============================\n@ahchim.lee@bespinglobal.com @webhook-AlertNow",
	"tags": [],
	"options": {
		"notify_audit": false,
		"locked": false,
		"timeout_h": 0,
		"new_host_delay": 300,
		"require_full_window": true,
		"notify_no_data": false,
		"renotify_interval": "0",
		"escalation_message": "{{host.name}} RDS Cunnection is still High",
		"no_data_timeframe": null,
		"include_tags": false,
		"thresholds": {
			"critical": 500,
			"warning": 450
		}
	},
	"priority": null
}
```

## RDS CPU Used % >=

```json
{
	"id": 33109357,
	"name": "[Company] RDS CPU Used >= {{threshold}}% at {{name.name}}",
	"type": "metric alert",
	"query": "avg(last_1m):avg:aws.rds.cpuutilization{*} by {name,engine,host} >= 90",
	"message": "*Alarm                 :  RDS CPU Used >= {{threshold}}%\n*Current              : {{value}}%\n*ClusterName    :  {{name.name}} ({{engine.name}}) \n*DBName           : {{host.dbname}} ({{host.name}})\n*Time(UTC)        :  {{last_triggered_at}}\n===============================\n@ahchim.lee@bespinglobal.com @webhook-AlertNow",
	"tags": [],
	"options": {
		"notify_audit": false,
		"locked": false,
		"timeout_h": 0,
		"new_host_delay": 300,
		"require_full_window": true,
		"notify_no_data": false,
		"renotify_interval": "0",
		"escalation_message": "{{host.name}} RDS CPU Utilization is still High",
		"no_data_timeframe": null,
		"include_tags": false,
		"thresholds": {
			"critical": 90,
			"warning": 85
		}
	},
	"priority": null
}
```


## RDS Freeable Memory <=

```json
{
	"id": 33110261,
	"name": "[Company] RDS Freeable Memory <= {{eval \"threshold/1074000000\"}}GB at {{name.name}}",
	"type": "metric alert",
	"query": "avg(last_1m):avg:aws.rds.freeable_memory{!engine:aurora-mysql} by {name,engine,host} <= 1074000000",
	"message": "*Alarm                 :  RDS Freeable Memory <= {{eval \"threshold/1074000000\"}}GB\n*Current              : {{value}}%\n*ClusterName    :  {{name.name}} ({{engine.name}}) \n*DBName           : {{host.dbname}} ({{host.name}})\n*Time(UTC)        :  {{last_triggered_at}}\n===============================\n@ahchim.lee@bespinglobal.com @webhook-AlertNow",
	"tags": [],
	"options": {
		"notify_audit": false,
		"locked": false,
		"timeout_h": 0,
		"new_host_delay": 300,
		"require_full_window": true,
		"notify_no_data": false,
		"renotify_interval": "0",
		"escalation_message": "{{host.name}} RDS Freeable Memory is still low",
		"no_data_timeframe": null,
		"include_tags": false,
		"thresholds": {
			"critical": 1074000000
		}
	},
	"priority": null
}
```


## RDS Storage Space Used % >=

```json
{
	"id": 33110168,
	"name": "[Company] RDS Storage Space Used >= {{threshold}}% at {{name.name}}",
	"type": "query alert",
	"query": "avg(last_1m):( avg:aws.rds.total_storage_space{!engine:aurora-mysql} by {name,engine,host} - avg:aws.rds.free_storage_space{!engine:aurora-mysql} by {name,engine,host} ) / avg:aws.rds.total_storage_space{!engine:aurora-mysql} by {name,engine,host} * 100 >= 90",
	"message": "*Alarm              :  RDS Storage Space Used >= {{threshold}}%\n*Current           : {{value}}%\n*ClusterName :  {{name.name}} ({{engine.name}}) \n*DBName        : {{host.dbname}} ({{host.name}})\n*Time(UTC)     :  {{last_triggered_at}}\n===============================\n@ahchim.lee@bespinglobal.com @webhook-AlertNow",
	"tags": [],
	"options": {
		"notify_audit": false,
		"locked": false,
		"timeout_h": 0,
		"silenced": {},
		"include_tags": true,
		"no_data_timeframe": null,
		"require_full_window": true,
		"new_host_delay": 300,
		"notify_no_data": false,
		"renotify_interval": 0,
		"evaluation_delay": 900,
		"escalation_message": "RDS Storage Space Usage is Still High",
		"thresholds": {
			"critical": 90,
			"warning": 85
		}
	},
	"priority": null
}
```

## RDS Uptime [time] below
- Aurora Only
```json
{
	"id": 33114634,
	"name": "[Company] RDS {{name.name}} Uptime {{threshold}} below",
	"type": "metric alert",
	"query": "avg(last_1m):avg:aws.rds.engine_uptime{*} by {dbname,engine,name} <= 0",
	"message": "*Alarm   :  {{name.name}} is Down ({{value}})\n*RDS Down : {{name.name}} ({{dbname.name}})\n*Time(UTC) : {{last_triggered_at}}\n\n @ahchim.lee@bespinglobal.com @webhook-AlertNow",
	"tags": [],
	"options": {
		"notify_audit": false,
		"locked": false,
		"timeout_h": 0,
		"new_host_delay": 300,
		"require_full_window": true,
		"notify_no_data": false,
		"renotify_interval": "0",
		"escalation_message": "",
		"no_data_timeframe": null,
		"include_tags": true,
		"thresholds": {
			"critical": 0
		}
	},
	"priority": null
}
```

## RDS Failover Event
```json
{
	"name": "[Company] {{event.title}}",
	"type": "event alert",
	"query": "events('tags:event_type:failover priority:all sources:rds').rollup('count').last('5m') >= 1",
	"message": "*Title              :  {{event.title}}\n*Description :  {{event.text}}\n*EventId        :  {{event.id}}\n*Time(UTC)   :  {{last_triggered_at}}\n-------------------------------------------------------------------------------------- \n@ahchim.lee@bespinglobal.com @webhook-AlertNow",
	"tags": [],
	"options": {
		"notify_audit": false,
		"locked": false,
		"timeout_h": 0,
		"silenced": {},
		"include_tags": true,
		"thresholds": {
			"critical": 1
		},
		"new_host_delay": 300,
		"notify_no_data": false,
		"renotify_interval": 1440,
		"no_data_timeframe": 2
	},
	"priority": null
}
```


## VPN Tunnel Down

```json
{
	"id": 33112951,
	"name": "[Company] VPN Tunnel {{name.name}} is Down",
	"type": "metric alert",
	"query": "avg(last_1m):avg:aws.vpn.tunnel_state{*} by {name,vpnid} <= 0",
	"message": "*Name : {{name.name}} ({{vpnid.name}})\n*Time(UTC) : {{last_triggered_at}}\n@ahchim.lee@bespinglobal.com @webhook-AlertNow",
	"tags": [],
	"options": {
		"notify_audit": false,
		"locked": false,
		"timeout_h": 0,
		"new_host_delay": 300,
		"require_full_window": true,
		"notify_no_data": false,
		"renotify_interval": "0",
		"escalation_message": "",
		"no_data_timeframe": null,
		"include_tags": true,
		"thresholds": {
			"critical": 0
		}
	},
	"priority": null
}
```


## AWS Health Scheduled Event  
DataDog Integration 중 **Amazon Health** 설치 후 사용
```json
{
	"id": 32998109,
	"name": "[Company] AWS Health Scheduled Event: {{event.title}}",
	"type": "event alert",
	"query": "events('priority:all scheduled sources:health').rollup('count').last('1d') >= 1",
	"message": "*Alarm           :  AWS Health Scheduled Event Notification - {{event.host.name}}\n*EventId        :  {{event.id}}\n*Title              :  {{event.title}}\n*Description :  {{event.text}}\n*Time(UTC)   :  {{last_triggered_at}}\n--------------------------------------------------------------------------------------\n@ahchim.lee@bespinglobal.com @webhook-AlertNow",
	"tags": [],
	"options": {
		"notify_audit": false,
		"locked": false,
		"timeout_h": 0,
		"silenced": {},
		"include_tags": true,
		"thresholds": {
			"critical": 1
		},
		"new_host_delay": 300,
		"notify_no_data": false,
		"renotify_interval": 1440
	},
	"priority": null
}
```

## AutoScaling Event
```json
{
	"id": 33859928,
	"name": "[Company] {{event.title}} Autoscaling Event",
	"type": "event alert",
	"query": "events('priority:all sources:autoscaling').rollup('count').last('5m') >= 2",
	"message": "**[Description]**  {{event.text}}\n--------------------------------------------------------------------------------------\n@ahchim.lee@bespinglobal.com @slack-newrelictestahchim-newrelic-test-ahchim",
	"tags": [],
	"options": {
		"notify_audit": false,
		"locked": false,
		"timeout_h": 0,
		"silenced": {},
		"include_tags": true,
		"thresholds": {
			"critical": 2,
			"warning": 1
		},
		"new_host_delay": 300,
		"notify_no_data": false,
		"renotify_interval": 0,
		"no_data_timeframe": 2
	},
	"priority": null
}
```


## [process_name] Process Count = 0
**/etc/datadog-agent/conf.d/process.d/conf.yaml** 파일 설정 후 **datadog-agent** 재시작하여 사용  
ex) name: 프로세스명_포트(옵션)
```json
init_config:

instances:
  - name: tomcat_8080
    search_string: ["java", "tomcat"]
```
메트릭 생성 시 상단 name: 부분 process_name.name으로 확인 가능

참고: https://docs.datadoghq.com/integrations/process/
```json
{
	"id": 33139409,
	"name": "[Company] {{process_name.name}} process count = {{eval \"int(threshold)\"}} in {{name.name}}",
	"type": "metric alert",
	"query": "avg(last_1m):avg:system.processes.number{*} by {name,process_name,host,cloud_provider,service,instance-type,availability-zone,env} <= 0",
	"message": "*Alarm   :  [{{process_name.name}}] process count = {{eval \"int(threshold)\"}}\n*Name   :  {{name.name}} ({{instance-type.name}}) / {{host.ip}}\n*AZ         :  {{cloud_provider.name}} > {{availability-zone.name}}\n*Service :  {{service.name}}  >  {{env.name}}\n*Time(UTC):  {{last_triggered_at}}\n===============================\n@ahchim.lee@bespinglobal.com @webhook-AlertNow",
	"tags": [],
	"options": {
		"notify_audit": false,
		"locked": false,
		"timeout_h": 0,
		"new_host_delay": 300,
		"require_full_window": true,
		"notify_no_data": false,
		"renotify_interval": "0",
		"escalation_message": "",
		"no_data_timeframe": null,
		"include_tags": false,
		"thresholds": {
			"critical": 0
		}
	},
	"priority": null
}
```

### Process Count 알람 예시 (Slack)  
![](https://github.com/AhchimLee/memo/raw/main/process_test_01.png)

## URL Check = 0  

### 웬만하면 하기 드라이브 링크의 Synthetic URL 모니터링 추천 (서버 설정 불필요)
Synthetic URL Monitoring: [https://drive.google.com/file/d/1cmM_nMbKhI4ymM8wehG-Pc8jLJstjaI2/view](https://drive.google.com/file/d/1cmM_nMbKhI4ymM8wehG-Pc8jLJstjaI2/view)


Bastion 서버 등 URL 체크 가능한 인스턴스에서  
**/etc/datadog-agent/conf.d/http_check.d/conf.yaml** 파일 설정 후 **datadog-agent** 재시작하여 사용

```json
ex) 
  - name: 표시명 (메트릭 상 instance.name으로 집계)
    url: 체크 원하는 URL 기입 (메트릭 상 url.name으로 집계)
```
```json
init_config:

instances:
  - name: Google
    url: https://www.google.com/
  - name: ZET_Main
    url: https://www.zetmobility.com/
  - name: failurl              # 실패 테스트 URL
    url: http://zettotototo.com
```

참고: https://docs.datadoghq.com/integrations/http_check/#setup  
```json
{
	"id": 33169024,
	"name": "[Company] URL {{url.name}} Check = {{eval \"int(threshold)\"}}",
	"type": "metric alert",
	"query": "avg(last_5m):avg:network.http.can_connect{*} by {url,instance,host,cloud_provider,service,availability-zone,env} <= 0",
	"message": "*Alarm   :  URL {{url.name}} Check = {{eval \"int(threshold)\"}}\n*Name   :  {{instance.name}} ({{url.name}}), Check in {{host.ip}}\n*AZ         :  {{cloud_provider.name}} > {{availability-zone.name}}\n*Service :  {{service.name}}  >  {{env.name}}\n*Time(UTC):  {{last_triggered_at}}\n===============================\n@ahchim.lee@bespinglobal.com @webhook-AlertNow",
	"tags": [],
	"options": {
		"notify_audit": false,
		"locked": false,
		"timeout_h": 0,
		"new_host_delay": 300,
		"require_full_window": false,
		"notify_no_data": false,
		"renotify_interval": "0",
		"escalation_message": "",
		"no_data_timeframe": null,
		"include_tags": false,
		"thresholds": {
			"critical": 0
		}
	},
	"priority": null
}
```

### URL Check 알람 예시 (Slack)  
![](https://github.com/AhchimLee/memo/raw/main/url_test_01.png)



## 포트번호 Port Down at 서버명
Bastion 서버 등 Port 체크 가능한 인스턴스에서  
**/etc/datadog-agent/conf.d/tcp_check.d/conf.yaml** 파일 설정 후 **datadog-agent** 재시작하여 사용  

```json
ex) 
  - name: 대상 인스턴스 이름 (메트릭 상 instance.name으로 집계)
    host: 체크 원하는 host ip 혹은 URL 기입 (메트릭 상 target_host.name으로 집계)
    port: 포트번호 입력
    collect_response_time: true  로 입력 (기본: false)
```
```json
init_config:

instances:
  - name: BESPIN-TEST-WAS-A-01
    host: 10.180.11.145
    port: 8080
    collect_response_time: true
```

참고: https://docs.datadoghq.com/integrations/http_check/#setup  
```json
{
	"id": 33171341,
	"name": "[Company] {{port.name}} Port Down at {{instance.name}}",
	"type": "metric alert",
	"query": "avg(last_5m):avg:network.tcp.can_connect{*} by {target_host,port,instance,cloud_provider,availability-zone,service,env,host,name} <= 0",
	"message": "*Alarm   :  {{port.name}} Port Down at {{instance.name}} \n*URL       :  {{target_host.name}} (Check at {{name.name}} ({{host.ip}})\n*AZ         :  {{cloud_provider.name}} > {{availability-zone.name}}\n*Service :  {{service.name}}  >  {{env.name}}\n*Time(UTC):  {{last_triggered_at}}\n===============================\n@ahchim.lee@bespinglobal.com @webhook-AlertNow",
	"tags": [],
	"options": {
		"notify_audit": false,
		"locked": false,
		"timeout_h": 0,
		"new_host_delay": 300,
		"require_full_window": false,
		"notify_no_data": false,
		"renotify_interval": "0",
		"escalation_message": "",
		"no_data_timeframe": null,
		"include_tags": false,
		"thresholds": {
			"critical": 0
		}
	},
	"priority": null
}
```

### Port Check 알람 예시 (Slack)  
![](https://github.com/AhchimLee/memo/raw/main/port_test_01.png)
![](https://github.com/AhchimLee/memo/raw/main/port_test_02.png)
