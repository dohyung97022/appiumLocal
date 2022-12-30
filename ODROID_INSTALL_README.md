# odroidInstall

### 1. odroid 안에 SSH
* odroid 인터넷 연결
* odroid 내부 ip 확인
* ssh odroid@192.168.35.139
* default password: odroid

### 2. VNC 서버를 설치
* sudo apt update
* sudo apt install x11vnc (tightvncserver 불가)

### 3. 부팅시 VNC 서버를 자동 실행
* sudo crontab -e (일반 권한 crontab 은 부팅 불가)
* 다음을 추가
  * @reboot sudo nohup x11vnc -display :0 -auth guess 2>&1 | logger -t vncsh
  * @reboot
    * 부팅시 시작
  * -display :0 
    * 0번 포트에 VNC 서버 연결
  * -auth guess 
    * Xauthority 파일 위치를 서버가 추정합니다.
    * -auth /home/$user/.Xauthority 같이 지정 가능
  * 2>&1 | logger -t vncsh
    * stderr 를 stdout 와 병합
    * vim /var/log/syslog 안에 vncsh 이라는 명칭의 로그를 남깁니다.
    * 작동이 안될 경우 cat /var/log/syslog | grep vncsh 를 통해 확인
