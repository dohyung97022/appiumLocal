# windows10Install

### 1. windows 안에 SSH
* windows 의 powershell 을 관리자 권한으로 실행
* Add-WindowsCapability -Online -Name OpenSSH.Server*
    * ssh 설치
* Start-Service sshd
    * ssh 실행
* Set-Service -Name sshd -StartupType 'Automatic'
    * 부팅시 ssh 실행

### 2. VNC 서버를 설치
* https://www.tightvnc.com/download.php
    * tightvnc 설치
