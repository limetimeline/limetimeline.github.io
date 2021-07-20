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

# 1. OS Install and Network Setting

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

## Network Setting
- Package Update and Upgrade

```shell
# apt-get update; apt-get upgrade -y; apt-get autoremove -y; apt-get autoclean
# passwd // root 패스워드 지정
# apt-get install vim   // vim 에디터 설치
```

- Network Setting <br>
=> netplan으로 네트워크를 설정하고 lightdm과 networkmanager는 CLI에서 동작하지 않으므로 해제.

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
            addresses [168.126.63.1,8.8.8.8] # DNS 서버 주소
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

# 2. wheel group, su, pam.d
SSH로 원격 접속 시 root로 접근하게 두면 보안에 약하다.
<br>그렇기 때문에 SSH 전용 계정을 하나 만들고 그 계정으로 `su`명령어를 통해 root로 접근할 수 있도록 만들어줄 것이다. (물론 `sudo`를 사용하는 것이 더 안전하다.)

```shell
   # mkdir /home/dorothy   // dorothy라는 계정을 만들건데 dorothy의 홈 디렉터리
   # useradd -d /home/dorothy dorothy   // dorothy 계정 만들기. -d는 홈디렉터리 지정
   # chown /home/dorothy dorothy   // /home/dorothy 디렉터리를 dorothy가 소유하도록 설정
   # groupadd wheel   // wheel 이라는 그룹 생성
   # vi /etc/pam.d/su     // su 명령을 wheel 그룹에 속한 사용자들만 사용하도록 허용
      Auth   required    pam_wheel.so           
   # usermod -G wheel dorothy   // dorothy가  wheel 그룹에 속하도록 설정
   # passwd dorothy    // dorothy 비번 변경
```

# 3. SSH Setting
SSH 포트를 바꿔보자!

```shell
   # vi /etc/ssh/sshd_config
      Port 1234   # 포트 번호를 기존 22번에서 1234번으로 변경
      PermitRootLogin no   # 루트로 로그인 금지
   # service sshd restart
```

이제 SSH 원격 접속 시 포트를 `1234`로 변경해야 접속이 가능하고 이전에 SSH 전용 계정인 dorothy로 로그인하여 su명령어로 root로 전환해야 한다.

# 4. iptables Setting
최상의 보안 상태를 유지하기 위하여 사용하는 포트 외에 다른 모든 포트는 막아둘 것이다.
<br> **※ SSH, HTTP(S), FTP 포트 (추후 추가 예정)**
<br>
> **주의: SSH 포트를 막게되면 통신이 안되는 주의!**

```shell
   # apt-get install iptables-persistent   // iptables-persistent 설치
   # mkdir /firewall
   # vi /firewall/servermode    // iptables 스크립트로 관리하기.
         #!/bin/bash     # 쉘스크립트 선언
         # iptables setting auto script
         # iptables setting reset
         iptables -F
         # SSH TCP PORT 22->7070
         iptables -A INPUT -p tcp -m tcp --dport 7070 -j ACCEPT
         # Base Policy Setting
         iptables -P INPUT DROP
         iptables -P FORWARD DROP
         iptables -p OUTPUT ACCEPT
         # localhost connection accept
         iptables -A INPUT -i lo -j ACCEPT
         # established and related Connection accept
         iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
         # HTTP 80 PORT, HTTPS 443 PORT, FTP 21 PORT ACCEPT
         iptables -A INPUT -p tcp --dport 80 -j ACCEPT
         iptables -A INPUT -p tcp --dport 443 -j ACCEPT
         iptables -A INPUT -p tcp --dport 21 -j ACCEPT
         # Settings save
         netfilter-persistent save
         # Settings list print
         iptables -L -v
   # ./firewall/servermode   // 스크립트 실행 ( 위 내용들 모두 실행된다.)
```

# 5. SWAP Memory Setting & HDD Mount
HC2에는 RAM이 2GB밖에 되지 않는다. 지금 당장 처리할 트래픽이 별로 없지만 혹시 모르니.. swap영역을 만들어주자!
> **SWAP은 RAM이 부족할 때 지정한 HDD나 SSD에 영역을 지정하여 RAM의 역할을 대신 수행하게 하는 것.**

그 후 HDD를 마운트하도록 하겠다.

## SWAP Memory Setting
보통 RAM의 두 배 정도 지정한다.

```shell
   # fallocate -l 4G /swapfile  // 용량이 4G인 빈파일 생성
   # chmod 600 /swapfile   // 권한 주기. rw-------
   # mkswap /swapfile   // 스왑메모리로 변경
   # swapon /swapfile   // 스왑메모리 활성화
   # vi /etc/fstab   // 재부팅 후에도 지정
         /swapfile swap swap default 0 0
   # free -h   // 램 메모리와 스왑 메모리의 용량을 표시
                   total        used        free         shared   buff/cache   available
         Mem:        1.9G         98M        1.5G        6.2M        394M        1.8G
         스왑:          3.7G          0B          3.7G
```

## HDD Mount

```shell
   # fdisk -l  // 인식된 디스크 정보 출력
   # fdisk /dev/sda  // 파티션 생성 작업
         n  // 파티션 생성
         p  // 파티션 List 출력
         w  // 저장
   # mkfs.ext4 /dev/sda1   // /dev/sda1을 ext4 형식으로 포맷
   # mkdir /data   // 마운트 포인트 생성
   # mount /dev/sda1 /data   // /data 디렉터리를 마운트
   # ls -l /dev/disk/by-uuid   // 마운트된 장치들의 UUID 값. (blkid)
         lrwxrwxrwx 1 root root 10 11월 29 07:00 1a0d8a4c-6fe5-3bda-ab6e-22381194e917 -> ../../sda1
   # vi /etc/fstab   // 재부팅 후에도 마운트 적용
         UUID=1a0d8a4c-6fe5-3bda-ab6e-22381194e917 /data ext4 defaults 0 0
```

# 6. HDD 스핀 절전 설정
필요할 때만 돌고 그 외에는 절전하게 하여 HDD의 수명을 관리.

```shell
   # vi /etc/hdparm.conf
         /dev/sda{
            spindown_time = 240  # 240*5 = 1200초 = 20분
         }
```

# 7. RTC Wakeup & NTP Setting
HC2는 `WOL`을 지원하지 않기 때문에 `RTC Wakeup` 기능을 사용.
<br> `RTC Backup Battery`의 시스템 시간을 저장하는 특징을 이용하여 특정 시간대에 꺼두고 켜기 위함.

-  NTP서버와 RTC Backup Battery 연동
   - 한국 시간 설정 및 타임서버 동기화

```shell
   # dpkg-reconfigure tzdata  // 서버 지역 시간 설정. Asia - Seoul
   # apt-get install ntpdate  // ntp 패키지 설치
   # mkdir /sh
   # vi /sh/ntpsynchronization.sh   // 동기화 스크립트.
         #!/bin/bash
         #ntp synchronization
         ntpdate time.bora.net

         #ntp date(system date)->CMOS date
         hwclock -w
   # chmod +x /sh/ntpsynchronization.sh   // 실행권한 부여
   # vi /etc/crontab
         10 * * * * root /sh/ntpsynchronization.sh   # 10 분 주기로 계속 동기화 스크립트 실행
```

- RTC Wakeup 설정
   6시간 후에 켜지도록 작성된 쉘 스크립트를 1시에 실행되게 하여 아침 7시에 켜지도록 설정.

```shell
   #vi /sh/rtcwakeup.sh   // 21600초 -> 6시간.
         #!/bin/bash
         # RTC backup battery for Auto wake up
         rtcwake -m no -s 21600
         # shutdown
         shutdown -h 0
   # vi /etc/crontab   // 매일 1시에 스크립트 실행 ( 7시에 다시 켜짐 ㅎ)
         # rtc backup battery auto wakeup 01 ~ 07
         00 1 * * * root /sh/rtcwakeup.sh
```

# 8. SFTP
OpenSSH에서 제공하는 SFTP. 기존 FTP보다 보안에 강하다. TCP를 사용하여 조금 느리다.
> SSH의 포트와 같은 포트를 사용한다. (ex) 1234

```shell
   # groupadd sftpusers
   # useradd -G sftpusers -s /sbin/nologin dataman   // 로그인이 안되는 단순히 sftp만을 위해 존재하는 계정
   # passwd dataman
   # vi /etc/ssh/sshd_config
         #Subsystem   sftp   /usr/lib/openssh/sftp-server
        subsystem sftp internal-sftp
         Match group sftpusers    # sftpusers 그룹에 속한 계정만 접근 가능
         ChrootDirectiory /data    # 루트 디렉터리를 /data로 설정. (상위 디렉토리로 접근 X)
         X11Forwarding no
         Allowtcpforwarding no
         Forcecommand internal-sftp
   # chown dataman.sftpusers /data
   # chmod -R 755 /data
   # service sshd restart
   # mkdir /data/upload
   # chmod 775 /data/upload
   # chown dataman.sftpusers /data/upload
   # chmod 711 /
   # chmod 711 /bin
   # chmod 711 /boot
   # chmod 711 /dev
   # chmod 711 /etc
   # chmod 711 /home
   # chmod 711 /mnt
   # chmod 711 /opt
   # chmod 711 /proc
   # chmod 711 /usr/local
   # chmod 711 /var
```

# 9. SSH motd(Message of The day) Setting
- 로그인 완료 시 나오는 리눅스 버전 없애기.
![motd_ver](/assets/images/odroid/motd_ver.png)
뭔가 보안상 없애고 싶다...

```shell
   # cd /etc/update-motd.d  // ssh 로그인 완료할 때 나오는 리눅스 버전 관련 디렉터리
   # tar cvfp backup.tar *     // 압축해서 백업하기(퍼미션도 그대로 유지)
   # mkdir backup
   # mv backup.tar backup
   # chmod –x /etc/update-motd.d/*            // 실행권한 제거
   # chmod +x /etc/update-motd.d/backup    // 백업 디렉터리에 실행권한 주기
   # service sshd restart      // 이제 리눅스 버전 안나옴
```

- 로그인 시 나타나는 반겨주는 메시지.
![motd_modify](/assets/images/odroid/motd_modify.png)

```shell
   # vi /etc/motd
          -------------------------------------------------------------
         | Welcome to Limetime's with Lock&Lock Server! |
         -------------------------------------------------------------
   # vi /etc/ssh/sshd_config
         PrintMotd no   #주석 해제. 기본값이 yes로 되어 있음 (두 번 출력됨)
         #PrintLastLog yes   # 'Last login: Sun Nov 29 14: 18: 22 2020 from' 과 같은 마지막 접속 로그 출력
   # service sshd restart
```











# Error
> **일어나는 에러들을 정리해보았다.**

## -sh: 2: [: x: unexpected operator
![unexpected operator](/assets/images/odroid/unexpected_operator.png)
- 보통은 `/bin/sh가 bash`를 가리키는데 우분투는 `dash`를 가리킴으로써 발생하는 에러
- `/bin/sh와 bash`를 링크시켜주면 해결!

```shell
   # ls -ahl /bin/sh
         lrwxrwxrwx 1 root root 4  4월 12  2018 /bin/sh -> dash    // dash로 링크되어 있음
   # unlink /bin/sh  // 링크 해제
   # ls -ahl /bin/sh
         ls: '/bin/sh'에 접근할 수 없습니다: 그런 파일이나 디렉터리가 없습니다         // 링크 해제 확인
   # ln -s /bin/bash /bin/sh  // bash로 링크
   # ls -ahl /bin/sh
         lrwxrwxrwx 1 root root 9 11월 29 13:52 /bin/sh -> /bin/bash           // bash로 링크됨
```