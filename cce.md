
### <div id='2.8'/>2.8. Docker daemon audit 설정
- 2.8. Docker daemon audit 설정부터 2.13. /etc/default/docker audit 설정 항목까지는 동일한 파일 audit.rules를 수정하므로 ## Add at the bottom 부분의 설정을 하위에 계속 추가 적용하면 된다. 

- 항목 설명
  + Docker 데몬은 root 권한으로 실행 되기 때문에 그 활동과 용도를 감사하여야한다.

- 조치대상

| <center>대상 환경</center> | <center>분류</center> | <center>조치 대상</center> |
| :--- | :--- | :---: |
| Cluster | Master | O |
|| Worker | O |
| Bosh | MariaDB | X |
|| HAProxy | X |
|| Private-image-repository | X |

- 진단방법
  + 명령어를 통해 /usr/bin/docker 감사 설정 확인
```
# auditctl -l | grep /usr/bin/docker
```
 + 명령어를 통해 /etc/audit/audit.rules 파일의 내용 확인
```
# cat /etc/audit/audit.rules | grep /usr/bin/docker
```

- 조치방법
  + audit 설정 적용
```
다음과 같은 절차로 설정 적용
# apt install auditd -y
# vi /etc/audit/rules.d/audit.rules

## Add at the bottom
-w /usr/bin/docker -k docker
-w /var/lib/docker -k docker
-w /etc/docker -k docker
-w /lib/systemd/system/docker.service -k docker
-w /lib/systemd/system/docker.socket -k docker
-w /etc/default/docker -k docker

# systemctl restart auditd.service
```


---

### <div id='2.13'/>2.13. /etc/default/docker audit 설정



- 조치방법
  + audit 설정 적용
```
다음과 같은 절차로 설정 적용
# apt install auditd -y
# vi /etc/audit/rules.d/audit.rules

## Add at the bottom


# systemctl restart auditd.service
```
---
