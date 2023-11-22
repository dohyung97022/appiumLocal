# ubuntuInstall
### 1. SSH 설치
* sudo apt update
* sudo apt install openssh-server
* sudo ufw allow ssh
* sudo systemctl enable ssh
  * 서비스 등록
* sudo systemctl daemon-reload
  * 서비스 재로드
* sudo systemctl start ssh
  * 서비스 시작

### 1. VNC 서버를 설치
* sudo apt update
* sudo apt install x11vnc
* sudo ufw allow 5900
* sudo vim /etc/gdm3/custom.conf
  * WaylandEnable=false 를 주석 해제
* sudo vim /etc/systemd/system/x11vnc.service
  * 서비스 생성
  * ```
    [Unit]
    Description=x11vnc
    After=display-manager.service

    [Service]
    Type=simple
    Environment=DISPLAY=:0
    User=ubuntu
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
