# huaweiInstall

### 1. 서버 내에 Huawei 사의 E353/E3131 usim LTE 모뎀 연결
* lsusb 결과가 12d1:14db Huawei Technologies Co., Ltd. E353/E3131 로 나와야 합니다.
* 만일 12d1:14db 가 안나오고 12d1:1f01 일 경우
  * sudo apt update
  * sudo apt install usb_modeswitch
  * sudo usb_modeswitch -v 12d1 -p 1f01 -M '55534243123456780000000000000a11062000000000000100000000000000'
    * 벤더사 12d1 (Huawei) 의 타입 1f01 (mass storage mode) 을 14db (hilink mode) 타입으로 변경하는 메시지를 전할합니다.

### 2. 네트워크 연결 확인
* ifconfig 의 결과 내에 eth{숫자} 의 네트워크가 추가되어야 합니다.
* ifconfig 내의 해당 eth 의 inet 을 확인합니다.
* inet 192.168.1.100 일 경우 192.168.1.1 이 LTE 모뎀의 제어 서버입니다.

### 3. LTE 인터넷 연결 요청
* curl -X POST http://192.168.1.1/api/dialup/dial -d "<?xml version="1.0" encoding="UTF-8"?><request><Action>1</Action></request>"
  * LTE 제어 서버에 usim LTE 연결을 요청합니다.
  * Huawei 와 다른 모뎀을 사용한다면 해당 회사의 제어 서버 주소에서 보내는 요청을 확인하면 됩니다.
* 만일 요청이 안될 경우 http://192.168.1.1 를 확인하여 usim, 모뎀에 문제가 있는지 확인합니다.

### 4. 네트워크 우선순위 설정
* 서버가 연결된 랜선 네트워크와 LTE 모뎀이 연결된 네트워크 중 우선순위를 설정해야 합니다.
* sudo apt install ifmetric
* route -n (네트워크 연결 정보 조회)
  * 만일 Iface 의 eth(LTE 모뎀) 네트워크의 Metric 이 wlan(서버 랜선) 의 Metric 보다 낮을 경우 LTE 네트워크가 우선됩니다.
* sudo ifmetric wlan0 50
  * wlan0 의 Metric 수치를 낮춰 우선순위를 높힙니다.

### 5. 네트워크 연결 확인
* curl https://www.google.com --interface eth0 (LTE 모뎀을 통해 조회)
* curl https://www.google.com --interface wlan0 (내부망을 통해 조회)
