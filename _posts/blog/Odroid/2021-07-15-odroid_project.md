---
title:  "Odroid Project"
excerpt: "Odroid HC2를 활용!!"
categories:
  - Odroid
  - Project
tags:
  - Odroid
  - Project
  - HC2
last_modified_at: 2021-07-15
toc: true
toc_sticky: true
---

# ODROID Project with Lock&Lock
![hc2_with_lock&lock](/assets/images/odroid/lock&lock.jpg)
<br>Lock&Lock 케이스 속의 작은 서버.

---

# Specifications
![HC2](/assets/images/odroid/HC2.jpg)
<br>서버의 기반 하드웨어는 하드커널사의 HC2이다.
<br>HC2는 `HomeCloud`의 약자이다.

- CPU/GPU : Samsung Exynos5422
- 2GB LPDDR3 RAM
- SATA slot x 1 → HDD or SSD
- Gigabit Ethernet x 1
- USB 2.0 slot x 1
- microSD slot x 1

## Customize
1. HDD : Toshiba HDD (?)
2. microSD : Samsung Pro Plus 64GB
   
   ![microSD](/assets/images/odroid/microSD.jpg)

3. RTC Backup Battery
   - 메인보드에서 WOL(Wake On Lan) 기능을 지원하지 않기 때문에 원격으로 끄고 켜기 쉽지 않다.
   - RTC 백업 배터리로 RTC Wake Up 기능을 사용.

    ![RTC](/assets/images/odroid/RTC_backup_battery.jpg)

4. Case
   - Lock & Lock Case
   - +) Mesh Filter
  
5. UART to USB
   - HC2는 CPIO(입출력)이 자유롭지 않다.
   - SSH로만 접근이 가능하여 네트워크 오류는 매우 치명적이다.
   - 그럴경우 UART 시리얼라인으로 복구한다.
   - `UART 전압 값을 주의!` `1.8 ~ 3.3V`
   - `반드시 하드커널에서 파는 UART to USB를 구입하시길..`
   
   ![UART](/assets/images/odroid/UART_Cable.jpg)

6. OS
    - Ubuntu Mate

# 1. OS Install and Network Setting.

## OS Install
[OS이미지 다운받기.](https://wiki.odroid.com/odroid-xu4/getting_started/os_installation_guide?redirect=1#tab__odroid-xu4)

위 사이트에서 OS Image를 다운받아 `etcher`을 이용해 SD카드에 구워주면 된다.
<br> ※ `Ubuntu Mate`로 시작하겠다.
<br> 구워진 SD카드를 HC2에 끼워서 전원을 넣어주면 자동으로 꺼졌다 켜졌다 하면서 시작된다. UART 모듈이 있다면 통신이 간단하겠지만 제일 중요한 IP주소 확인이 필요하다.
`공유기 필수!`
<br> 공유기 관리 페이지로 가서 DHCP로 뿌려준 IP 중 `ODROID`라는 이름을 가진 IP주소를 확인하자! 그 후 `Putty`를 이용해서 `SSH`에 접속해주자! `초기포트 : 22`
<br> ※ 우분투에서 자동으로 SSH를 열어준다.

```
   초기 SSH 로그인 ID/PW
   ID : root
   PW : odroid
```

## Network Setting.
1. Package Update and Upgrade

```shell
# apt-get update; apt-get upgrade -y; apt-get autoremove -y; apt-get autoclean
# passwd // root 패스워드 지정
# apt-get install vim   // vim 에디터 설치
```

2. Network Setting <br>
=> netplan으로 네트워크를 설정하고 lightdm과 networkmanager는 GUI에서 동작하지 않으므로 해제.

```shell
# vi /etc/netplan/01-netcfg.yaml
```

```shell
network:
   version: 2
   renderer: networkd
   ethernets:
      eth0:
         address:
            - 192.168.0.100/24   # 고정IP 지정. 192.168.0.100/24
         gateway4: 192.168.0.1   # 게이트웨이 주소 지정 공유기 IP
         nameservers:
            addresses [168.126.63.1,8.8.8.8] # DNS 서버 주소 생각나서.
```

```shell
# netplan apply   // Netplan을 적용. (/etc/netplan/01-netcfg.yam)
# systemctl disable lightdm.service // lightdm(GUI) 서비스 비활성
# service lightdm stop  // lightdm 서비스 정지.
# systemctl set-default multi-user.target // GUI → Multi-USER(CLI)모드로 런레벨 변경 (5 → 3)
# systemctl disable NetworkManager-wait-online.service
# systemctl stop NetworkManager-wait-online.service 
# systemctl disable NetworkManager.service   // Network-Manager(GUI) 비활성화
# systemctl stop Network Manager.service  // Network-Manager 서비스 정지
# apt-get update
```