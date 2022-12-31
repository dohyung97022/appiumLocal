# 3proxyInstall

### 1. sysctl 설정 수정
* sudo vim /etc/sysctl.conf
* 아래의 #net.ipv4.ip_forward=1 -> net.ipv4.ip_forward=1 로 수정
  * sysctl 은 시스템 설정을 칭합니다.
  * sysctl -a 로 변경 가능한 시스템 설정들을 확인할 수 있습니다.
  * 프록시를 생성하기 위해서 내부 ip 들의 통신 이동 경로를 조정할 필요성이 있어 0 -> 1 로 변경합니다.
* sudo sysctl -p
  * sysctl 신규 설정 갱신

### 2. fail2ban 설치
* sudo apt -y install fail2ban software-properties-common build-essential libevent-dev libssl-dev
  * fail2ban 은 ip 가 공개 주소로 열릴 경우 의심되는 ip의 공격을 막아줍니다.

### 3. 3proxy 설치
* sudo apt install git
* git clone https://github.com/z3apa3a/3proxy
* cd 3proxy
* vim src/proxy.h
* #define ANONYMOUS 1 추가
  * 정확한 설명을 3proxy 내에서 찾기 어렵지만 https://github.com/3proxy/3proxy/blob/5fa261e91e9f9877419202f596620d4af1fa5026/man/proxy.8 안에서 -a 명령어를 기본 설정으로 주는 것으로 추정됩니다.
* sudo ln -s Makefile.Linux Makefile
  * makefile 링크 생성
* sudo make
* sudo make install
  * 설치
* sudo systemctl is-enabled 3proxy.service
  * 3proxy 가 현제 실행되는지 확인
* echo '1 gw1' | sudo tee -a /etc/iproute2/rt_tables
  * rt_tables 는 숫자와 라우팅 테이블 매핑용 파일입니다.
  * https://serverfault.com/questions/963206/confused-about-iproute2-rt-tables
  * http://linux-ip.net/html/routing-tables.html
* vim 3proxy.cfg
  * #! /usr/local/bin/3proxy
  * daemon
  * nserver 8.8.8.8
  * nscache 65536   (https://github.com/3proxy/3proxy/blob/6279e860861ba78bc0c3fd217a485c8e60380934/cfg/3proxy.cfg.sample)
  * timeouts 1 5 30 60 180 15 60
  * users root:CL:pass
  * log /var/log/3proxy.log
  * rotate 30
  * setgid 13
  * setuid 13
  * auth none
  * allow root
  * proxy -p3128 -e192.168.1.100
  * flush
* vim startproxy.sh
  * 







mkdir proxy   
cd proxy   
echo 'changing sysctl settings'   
echo '# Custom 3PROXY settings' | sudo tee -a /etc/sysctl.conf   
echo 'net.ipv4.ip_forward=1' | sudo tee -a /etc/sysctl.conf   
sudo sysctl -p   
sudo apt -y install fail2ban software-properties-common build-essential libevent-dev libssl-dev   
sudo apt -y install git   
sudo git clone https://github.com/z3apa3a/3proxy   
cd 3proxy    
sudo sed -i "28i #define ANONYMOUS 1" src/proxy.h   
echo '# custom routes' | sudo tee -a /etc/iproute2/rt_tables   
echo '1 gw1' | sudo tee -a /etc/iproute2/rt_tables   
