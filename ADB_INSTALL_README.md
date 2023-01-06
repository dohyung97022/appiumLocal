# adbInstall

### 1. adb 설치
* sudo apt install android-tools-adb


### 2. adb 를 실행
* adb devices

### 3. 핸드폰 자체를 HUAWEI 모듈 대신 proxy 로 사용할 경우
* adb shell cmd connectivity airplane-mode enable
  * 비행기 모드 on
* adb shell cmd connectivity airplane-mode disable
  * 비행기 모드 off
* adb shell svc usb setFunctions rndis
  * usb 테더링 on
* adb shell svc usb setFunctions
  * usb 테더링 off
* adb shell svc data enable
  * lte on
* adb shell svc data disable
  * lte off
