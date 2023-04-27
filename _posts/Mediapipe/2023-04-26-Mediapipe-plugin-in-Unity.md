---
title:  "Mediapipe plugin in Unity"
excerpt: "Mediapipe plugin in Unity"
categories:
  - Mediapipe
tags:
  - ComputerVision
  - MediaPipe
  - Unity
last_modified_at: 2023-04-26
toc: false
toc_sticky: false
---






# Install

1. MSYS2 설치   
2. Python 3.7.8 설치   
3. Visual Studio 2019 설치   
4. Bazel 4.1.0 설치   
5. OpenCV 3.4.10 설치   
6. NuGet 5.11.3 설치   
7. git clone [MediaPipeUnityPlugin](https://github.com/Arham-Aalam/MediaPipeUnityPlugin)   
8. virtualenv 설치 및 가상 환경 설정   
9. Bazel 환경 변수 설정   
10.빌드   

---  

### MSYS2([https://www.msys2.org/](https://www.msys2.org/)) 설치

[https://github.com/msys2/msys2-installer/releases/download/2023-03-18/msys2-x86_64-20230318.exe](https://github.com/msys2/msys2-installer/releases/download/2023-03-18/msys2-x86_64-20230318.exe)

- **설치 경로 : `C:\msys64`**

![Untitled](/assets/images/mediapipe/2023-04-26-Mediapipe-plugin-in-Unity/1.png)

- **환경 변수 설정 확인 (사용자)**
    - 자동으로 되어 있는지 확인.
        - `C:\msys64\usr\bin`
    
    ![Untitled](/assets/images/mediapipe/2023-04-26-Mediapipe-plugin-in-Unity/2.png)
    
- **CMD를 열어서 아래 명령어 입력 (`C:\`에서 진행)**
    
    ```bash
    cd C:\
    pacman -S git patch unzip
    ```
    

### Python 3.7.8([https://www.python.org/downloads/release/python-378/](https://www.python.org/downloads/release/python-378/)) 설치

[https://www.python.org/ftp/python/3.7.8/python-3.7.8-amd64.exe](https://www.python.org/ftp/python/3.7.8/python-3.7.8-amd64.exe)

- **설치 경로 : `C:\Users\durumary\AppData\Local\Programs\Python\Python37` (자동)**
- **환경 변수 설정 (사용자)**
    - 사용자 환경 변수에서 Path 등록
        - `C:\Users\durumary\AppData\Local\Programs\Python\Python37`
        - `C:\Users\durumary\AppData\Local\Programs\Python\Python37\Scripts`

![Untitled](/assets/images/mediapipe/2023-04-26-Mediapipe-plugin-in-Unity/3.png)

![파이썬 버전 확인](/assets/images/mediapipe/2023-04-26-Mediapipe-plugin-in-Unity/4.png)

파이썬 버전 확인

### **Visual Studio 2019 설치**

[https://visualstudio.microsoft.com/ko/vs/older-downloads/](https://visualstudio.microsoft.com/ko/vs/older-downloads/)

- **워크로드 설치**
    - `C++를 사용한 데스크톱 개발`
    - `유니버설 Windows 플랫폼 개발`
    - `Unity를 사용한 게임 개발`
    
    ![Untitled](/assets/images/mediapipe/2023-04-26-Mediapipe-plugin-in-Unity/5.png)
    

### Bazel 4.1.0([https://bazel.build/install/windows?hl=ko](https://bazel.build/install/windows?hl=ko)) 설치

- **Microsoft Visual C++ 재배포 가능 패키지**
    - 없으면 설치! 혹시나 모르니 실행하고 설치되어 있는지 확인.
        
        [https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist?view=msvc-170](https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist?view=msvc-170)
        
- **chocolatey 설치하기**
    - Powershell을 관리자 권한으로 실행 후 아래 명령어 입력
        
        ```powershell
        Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
        ```
        
- **Bazel 설치하기**
    - Powershell을 관리자 권한으로 실행 후 아래 명령어 입력
        
        ```powershell
        choco install bazel --version 4.1.0 --force
        ```
        
        - YES/ALL/NO/PRINT 중 물어보면 그냥 `ALL`
        
        ![Bazel 버전 확인](/assets/images/mediapipe/2023-04-26-Mediapipe-plugin-in-Unity/6.png)
        
        Bazel 버전 확인
        

### OpenCV 3.4.10([https://opencv.org/releases/page/3/](https://opencv.org/releases/page/3/)) 설치

[https://sourceforge.net/projects/opencvlibrary/files/3.4.10/opencv-3.4.10-vc14_vc15.exe/download](https://sourceforge.net/projects/opencvlibrary/files/3.4.10/opencv-3.4.10-vc14_vc15.exe/download)

- **다운로드 후 `C:/opencv` 로 Extract**
    
    ![Untitled](/assets/images/mediapipe/2023-04-26-Mediapipe-plugin-in-Unity/7.png)
    
    ![Untitled](/assets/images/mediapipe/2023-04-26-Mediapipe-plugin-in-Unity/8.png)
    

### NuGet 5.11.3([https://www.nuget.org/downloads](https://www.nuget.org/downloads)) 설치

[https://dist.nuget.org/win-x86-commandline/v5.11.3/nuget.exe](https://dist.nuget.org/win-x86-commandline/v5.11.3/nuget.exe)

- `C:\`에 `nuget` 폴더 만든 후 그 안에 `nuget.exe` 복사 붙여넣기.
    - `C:\nuget\nuget.exe`
        
        ![Untitled](/assets/images/mediapipe/2023-04-26-Mediapipe-plugin-in-Unity/9.png)
        
- **환경 변수 설정 (사용자)**
    - 사용자 환경 변수에 Path 등록
        - `C:\nuget`
            
            ![Untitled](/assets/images/mediapipe/2023-04-26-Mediapipe-plugin-in-Unity/10.png)
            

### git clone **[MediaPipeUnityPlugin](https://github.com/Arham-Aalam/MediaPipeUnityPlugin)**

- `**C:\`에 git clone 하기 (CMD)**
    
    ```bash
    cd C:\
    git clone https://github.com/Arham-Aalam/MediaPipeUnityPlugin.git
    ```
    
- **WORKSPACE 수정 (`C:\MediaPipeUnityPlugin\WORKSPACE`)**
    - 수정 전 (파일 이름이 `rules_cc-master` → `rules_cc-main`으로 바뀌기도 했음)
        
        ```bash
        http_archive(
            name = "rules_cc",
        		strip_prefix = "rules_cc-master",
            urls = ["https://github.com/bazelbuild/rules_cc/archive/master.zip"],
        }
        ```
        
    - 수정 후
        
        ```bash
        http_archive(
            name = "rules_cc",
            strip_prefix = "rules_cc-8bb0eb5c5ccd96b91753bb112096bb6993d16d13",
            urls = ["https://github.com/bazelbuild/rules_cc/archive/8bb0eb5.zip"],
        )
        ```
        
        ![Untitled](/assets/images/mediapipe/2023-04-26-Mediapipe-plugin-in-Unity/11.png)
        
- **builde.py 수정(**`C:\MediaPipeUnityPlugin\build.py`)
    - `argparse.BooleanOptionalAction` → `‘store_true’`로 수정
        
        ```bash
        build_command_parser.add_argument('--resources', action='store_true', default=True)
        
        uninstall_command_parser.add_argument('--desktop', action='store_true', default=True)
        uninstall_command_parser.add_argument('--android', action='store_true', default=True)
        uninstall_command_parser.add_argument('--ios', action='store_true', default=True)
        uninstall_command_parser.add_argument('--resources', action='store_true', default=True)
        uninstall_command_parser.add_argument('--protobuf', action='store_true', default=True)
        ```
        
        ![Untitled](/assets/images/mediapipe/2023-04-26-Mediapipe-plugin-in-Unity/12.png)
        

### **virtualenv 설치 및 가상 환경 설정**

- **virtualenv 설치 (CMD)**
    
    ```bash
    cd C:\MediaPipeUnityPlugin
    pip install virtualenv
    virtualenv env
    env\Scripts\activate
    
    pip install numpy
    ```
    

### Bazel 환경 변수 설정

- **지금부터 설정하는 환경 변수는 일회용이다. 즉, CMD를 끄면 휘발한다.**
    - CMD를 다시 열 때 마다 입력해줘야 한다. (반드시 버전과 경로를 확인하자!)
        
        ```bash
        set BAZEL_VS=C:\Program Files (x86)\Microsoft Visual Studio\2019\Community
        set BAZEL_VC=C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\VC
        set BAZEL_VC_FULL_VERSION=14.29.30133
        set WINSDK_FULL_VERSION=10.0.19041.0
        set PYTHON_BIN_PATH=C:\MediaPipeUnityPlugin\env\Scripts\python.exe
        ```
        
        - **BAZEL_VS (Visual Studio 2019 경로)**
            - `C:\Program Files (x86)\Microsoft Visual Studio\2019\Community`
                
                ![Untitled](/assets/images/mediapipe/2023-04-26-Mediapipe-plugin-in-Unity/13.png)
                
        - **BAZEL_VC (Visual Studio 2019/VC 경로)**
            - `C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\VC`
                
                ![Untitled](/assets/images/mediapipe/2023-04-26-Mediapipe-plugin-in-Unity/14.png)
                
        - **BAZEL_VC_FULL_VERSION (VC\Tools\MSVC 버전)**
            - `C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\VC\Tools\MSVC\**14.29.30133**`
                
                ![Untitled](/assets/images/mediapipe/2023-04-26-Mediapipe-plugin-in-Unity/15.png)
                
        - **WINSDK_FULL_VERSION**
            - **Visual Studio 2019 - Windows 10 SDK 버전 (10.0.19041.0)**
                
                ![Untitled](/assets/images/mediapipe/2023-04-26-Mediapipe-plugin-in-Unity/16.png)
                
        - **PYTHON_BIN_PATH (python.exe 경로)**
            - **가상 환경의 Scripts안에 들어 있다.**
            - `C:\MediaPipeUnityPlugin\env\Scripts\python.exe`
                
                ![Untitled](/assets/images/mediapipe/2023-04-26-Mediapipe-plugin-in-Unity/17.png)
                
            

### 빌드

- **반드시 Bazel 환경 변수를 입력한 후 진행해야 한다.**   
    
    ```bash
    python build.py build --desktop cpu --include_opencv_libs -v
    ```   
    


# Unity   


### 유니티

- **유니티 버전은 `2022.3.7f1`로 테스트 했다.**
    
    ![Untitled](/assets/images/mediapipe/2023-04-26-Mediapipe-plugin-in-Unity/18.png)
    
- **Face, Face Mesh, Holistic 등이 있지만 Hand만 테스트 했다.**
    
    ![MediaPipeUnityPlugin](/assets/images/mediapipe/2023-04-26-Mediapipe-plugin-in-Unity/1.gif)
    

---

# Reference   
https://github.com/Arham-Aalam/MediaPipeUnityPlugin   
[https://www.youtube.com/watch?v=eZbi08dNCOU&t=277s](https://www.youtube.com/watch?v=eZbi08dNCOU&t=277s)   

