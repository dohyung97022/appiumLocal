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
* 부팅시 자동 로그인
  * sudo vim /usr/share/lightdm/lightdm.conf.d/60-lightdm-gtk-greeter.conf
  * ```
    [SeatDefaults]
    greeter-session=lightdm-gtk-greeter
    autologin-user=odroid
    ```
* service 추가 방법
  * sudo vim /etc/systemd/system/x11vnc.service
    * 서비스 생성
  * ```
    [Unit]
    Description=x11vnc
    After=display-manager.service

    [Service]
    Type=simple
    Environment=DISPLAY=:0
    User=odroid
    ExecStart=/usr/bin/x11vnc -loop -forever -bg -rfbport 5900 -display :0 -noxrecord -noxfixes -noxdamage -shared -norc -auth guess
    Restart=always
    RestartSec=1

    [Install]
    WantedBy=graphical.target
    ```
  * sudo systemctl enable x11vnc.service
    * 서비스 등록
  * sudo systemctl daemon-reload
    * 서비스 재로드
  * sudo systemctl start x11vnc.service
    * 서비스 시작
* crontab 방법
  * sudo crontab -e (일반 권한 crontab 은 부팅 불가)
  * @reboot sudo nohup x11vnc -reopen -forever -display :0 -auth guess 2>&1 | logger -t vncsh
  * @reboot
    * 부팅시 시작
  * -display :0 
    * 0번 포트에 VNC 서버 연결
  * -reopen -forever
    * 1번 접속이 끊겨도 서버 지속, 재접속 허용
  * -auth guess
    * Xauthority 파일 위치를 서버가 추정합니다.
    * -auth /home/$user/.Xauthority 같이 지정 가능
  * 2>&1 | logger -t vncsh
    * stderr 를 stdout 와 병합
    * vim /var/log/syslog 안에 vncsh 이라는 명칭의 로그를 남깁니다.
    * 작동이 안될 경우 cat /var/log/syslog | grep vncsh 를 통해 확인

### 4. fan 작동 온도 조절 방법
* /sys/class/thermal/thermal_zone0/trip_point_4_temp
  * 단위는 mili-celsius 입니다.
  * ex ) 35000 는 35도
