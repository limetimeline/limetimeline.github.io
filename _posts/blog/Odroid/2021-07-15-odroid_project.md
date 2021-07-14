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