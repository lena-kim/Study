
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

# systemctl restart auditd.service
```
---

### <div id='2.9'/>2.9. /usr/lib/docker audit 설정

- 항목 설명
  + /var/lib/docker 디렉터리는 컨테이너에 대한 모든 정보를 보유하고 있는 디렉터리이므로 감사 설정을 하여야 한다.

- 조치대상

| <center>대상 환경</center> | <center>분류</center> | <center>조치 대상</center> |
| :--- | :--- | :---: |
| Cluster | Master | O |
|| Worker | O |
| Bosh | MariaDB | X |
|| HAProxy | X |
|| Private-image-repository | X |

- 진단방법
  + 명령어를 통해 /var/lib/docker 감사 설정 확인
```
# auditctl -l | grep /var/lib/docker
```
 + 명령어를 통해 /etc/audit/audit.rules 파일의 내용 확인
```
# cat /etc/audit/audit.rules | grep /var/lib/docker
```

- 조치방법
  + audit 설정 적용
```
다음과 같은 절차로 설정 적용
# apt install auditd -y
# vi /etc/audit/rules.d/audit.rules

## Add at the bottom
-w /var/lib/docker -k docker

# systemctl restart auditd.service
```
---

### <div id='2.10'/>2.10. /etc/docker audit 설정

- 항목 설명
  + /etc/docker 디렉터리는 Docker 데몬과 Docker 클라이언트 간의 TLS 통신에 사용되는 다양한 인증서와 키를 보유하고 있으므로 감사 설정을 하여야 한다.

- 조치대상

| <center>대상 환경</center> | <center>분류</center> | <center>조치 대상</center> |
| :--- | :--- | :---: |
| Cluster | Master | O |
|| Worker | O |
| Bosh | MariaDB | X |
|| HAProxy | X |
|| Private-image-repository | X |

- 진단방법
  + 명령어를 통해 /etc/docker 감사 설정 확인
```
# auditctl -l | grep /etc/docker
```
 + 명령어를 통해 /etc/audit/audit.rules 파일의 내용 확인
```
# cat /etc/audit/audit.rules | grep /etc/docker
```

- 조치방법
  + audit 설정 적용
```
다음과 같은 절차로 설정 적용
# apt install auditd -y
# vi /etc/audit/rules.d/audit.rules

## Add at the bottom
-w /etc/docker -k docker

# systemctl restart auditd.service
```
---

### <div id='2.11'/>2.11. docker.service audit 설정

- 항목 설명
  + 데몬 매개변수가 관리자에 의해 변경된 경우 docker.service 파일이 존재한다. docker.service 파일은 Docker 데몬을 위한 다양한 파라미터를 보유하고 있으므로 감사 설정을 하여야 한다.

- 조치대상

| <center>대상 환경</center> | <center>분류</center> | <center>조치 대상</center> |
| :--- | :--- | :---: |
| Cluster | Master | O |
|| Worker | O |
| Bosh | MariaDB | X |
|| HAProxy | X |
|| Private-image-repository | X |

- 진단방법
  + docker.service 파일의 경로 확인
```
# systemctl show -p FragmentPath docker.service
```
  + 명령어를 통해 docker.service 감사 설정 확인
```
# auditctl -l | grep /lib/systemd/system/docker.service
```
 + 명령어를 통해 /etc/audit/audit.rules 파일의 내용 확인
```
# cat /etc/audit/audit.rules | grep docker.service
```

- 조치방법
  + audit 설정 적용
```
다음과 같은 절차로 설정 적용
# apt install auditd -y
# vi /etc/audit/rules.d/audit.rules

## Add at the bottom
-w /lib/systemd/system/docker.service -k docker

# systemctl restart auditd.service
```
---

### <div id='2.12'/>2.12. docker.socket audit 설정

- 항목 설명
  + docker.socket 파일은 Docker 데몬 소켓을 위한 다양한 파라미터를 보유하고 있으므로 감사 설정을 하여야 한다.

- 조치대상

| <center>대상 환경</center> | <center>분류</center> | <center>조치 대상</center> |
| :--- | :--- | :---: |
| Cluster | Master | O |
|| Worker | O |
| Bosh | MariaDB | X |
|| HAProxy | X |
|| Private-image-repository | X |  

- 진단방법
  + docker.socket 파일의 경로 확인
```
# systemctl show -p FragmentPath docker.socket
```
  + 명령어를 통해 docker.socket  감사 설정 확인
```
# auditctl -l | grep /lib/systemd/system/docker.socket
```
 + 명령어를 통해 /etc/audit/audit.rules 파일의 내용 확인
```
# cat /etc/audit/audit.rules | grep docker.socket
```

- 조치방법
  + audit 설정 적용
```
다음과 같은 절차로 설정 적용
# apt install auditd -y
# vi /etc/audit/rules.d/audit.rules

## Add at the bottom
-w /lib/systemd/system/docker.socket -k docker

# systemctl restart auditd.service
```
---

### <div id='2.13'/>2.13. /etc/default/docker audit 설정

- 항목 설명
  + /etc/default/docker 파일은 Docekr 데몬을 위한 다양한 파라미터를 보유하고 있으므로 감사 설정을 하여야 한다.

- 조치대상

| <center>대상 환경</center> | <center>분류</center> | <center>조치 대상</center> |
| :--- | :--- | :---: |
| Cluster | Master | O |
|| Worker | O |
| Bosh | MariaDB | X |
|| HAProxy | X |
|| Private-image-repository | X |

- 진단방법
  + 명령어를 통해 /etc/default/docker 감사 설정 확인
```
# auditctl -l | grep /etc/default/docker
```
 + 명령어를 통해 /etc/audit/audit.rules 파일의 내용 확인
```
# cat /etc/audit/audit.rules | /etc/default/docker
```

- 조치방법
  + audit 설정 적용
```
다음과 같은 절차로 설정 적용
# apt install auditd -y
# vi /etc/audit/rules.d/audit.rules

## Add at the bottom
-w /etc/default/docker -k docker

# systemctl restart auditd.service
```
---
