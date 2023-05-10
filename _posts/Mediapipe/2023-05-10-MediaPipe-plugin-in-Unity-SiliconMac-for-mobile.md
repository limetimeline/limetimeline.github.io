---
title:  "Mediapipe plugin in Unity(Silicon Mac) - Build iOS"
excerpt: "미디어파이프 플러그인을 이용한 유니티에서 iOS 빌드."
categories:
  - Mediapipe
tags:
  - ComputerVision
  - Mediapipe
  - Unity
last_modified_at: 2023-05-10
toc: true
toc_sticky: true
---

# 환경구성
## Mediapipe plugin 다운로드 및 압축풀기
[MediaPipeUnityPlugin v0.11.0](https://github.com/homuler/MediaPipeUnityPlugin/releases/download/v0.11.0/MediaPipeUnityPlugin-all.zip)   
Release Note : [MediaPipeUnityPlugin-Releases](https://github.com/homuler/MediaPipeUnityPlugin/releases)   

## build.py 실행 환경 구성 및 실행
### Homebrew 설치
[Homebrew](https://brew.sh/)   

### python 및 numpy 설치
```bash
brew install python
export PATH=$PATH:"$(brew --prefix)/opt/python/libexec/bin"

# Python version must be >= 3.9.0
python --version
# Python 3.9.x

pip3 install --user six numpy
```

`anaconda 사용 중이라면`   
```bash
conda create -n handUnityPlugin python=3.9
conda activate handUnityPlugin

#--- (handUnityPlugin) 상태인지 확인!! ---#

# anaconda 위치 : /Users/계정명/anaconda3/envs/handUnityPlugin/bin
export PATH=$PATH:"/Users/계정명/anaconda3/envs/handUnityPlugin/bin"
echo $PATH # PATH에 잘 등록되었는지 확인
pip3 install --user six numpy
```

### Bazelisk 설치
```bash
brew install bazelisk
```

### Nuget 설치
```bash
/usr/sbin/softwareupdate --install-rosetta # A입력하라고 했던 것 같다.
arch -x86_64 /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
arch -x86_64 /usr/local/Homebrew/bin/brew install nuget
```

### Xcode 설치
앱스토어에서 Xcode 설치
```bash
sudo xcodebuild -license # if not agreed to the license yet, 설치할 때 동의 했으면 안해도 되는듯?
sudo xcode-select -s /Applications/Xcode.app # Mac이랑 ios 설치할래? 물어보는거 했으면 안해도 되는듯?
xcode-select --install
```


### WORKSPACE 수정 
`C:\MediaPipeUnityPlugin\WORKSPACE`   
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
        
### builde.py 수정
`C:\MediaPipeUnityPlugin\build.py`   
- `argparse.BooleanOptionalAction` → `‘store_true’`로 수정
        
```bash
    build_command_parser = subparsers.add_parser('build', help='Build and install native libraries')
    build_command_parser.add_argument('--desktop', choices=['cpu', 'gpu'])
    build_command_parser.add_argument('--android', choices=['armv7', 'arm64', 'fat'])
    build_command_parser.add_argument('--android_ndk_api_level', type=int, choices=range(16, 31))
    build_command_parser.add_argument('--ios', choices=['arm64'])
    build_command_parser.add_argument('--resources', action='store_true', default=True)
    build_command_parser.add_argument('--analyzers', action='store_true', default=False, help='Install Roslyn Analyzers')
    build_command_parser.add_argument('--compilation_mode', '-c', choices=['fastbuild', 'opt', 'dbg'], default='opt')
    build_command_parser.add_argument('--opencv', choices=['local', 'cmake', 'cmake_static', 'cmake_dynamic'], default='local', help='Decide to which OpenCV to link for Desktop native libraries')
    build_command_parser.add_argument('--solutions', nargs='+',
        choices=['face_detection', 'face_mesh', 'iris', 'hands', 'pose', 'holistic', 'selfie_segmentation', 'hair_segmentation', 'object_detection', 'box_tracking', 'instant_motion_tracking', 'objectron'])
    build_command_parser.add_argument('--linkopt', '-l', action='append', help='Linker options')
    build_command_parser.add_argument('--apple_bitcode', action='store_true', default=True, help='Embed bitcode to iOS Framework')
    build_command_parser.add_argument('--macos_universal', action='store_true', default=False, help='Build a universal library')
    build_command_parser.add_argument('--bazel_startup_opts', action='append', help='Bazel startup options')
    build_command_parser.add_argument('--bazel_build_opts', action='append', help='Bazel startup options')
    build_command_parser.add_argument('--verbose', '-v', action='count', default=0)

    clean_command_parser = subparsers.add_parser('clean', help='Clean cache files')
    clean_command_parser.add_argument('--bazel_startup_opts', action='append', help='Bazel startup options')
    clean_command_parser.add_argument('--verbose', '-v', action='count', default=0)

    uninstall_command_parser = subparsers.add_parser('uninstall', help='Remove installed files')
    uninstall_command_parser.add_argument('--desktop', action='store_true', default=True)
    uninstall_command_parser.add_argument('--android', action='store_true', default=True)
    uninstall_command_parser.add_argument('--ios', action='store_true', default=True)
    uninstall_command_parser.add_argument('--resources', action='store_true', default=True)
    uninstall_command_parser.add_argument('--protobuf', action='store_true', default=True)
    uninstall_command_parser.add_argument('--analyzers', action='store_true', default=True)
    uninstall_command_parser.add_argument('--verbose', '-v', action='count', default=0)
```

### build.py 실행
```bash
python build.py build --desktop cpu --opencv cmake --macos_universal -vv
```

# 빌드
Hand Tracking 위주로 진행하였다.   

## Bootstrap Asset Loader Type - Streaming Asset
원하는 솔루션의 Scene을 선택하여 Bootstrap 스크립트의 Asset Loader Type을 `Streaming Asset`으로 변경해준다.   
![Untitled](/assets/images/mediapipe/2023-05-10-Mediapipe-plugin-in-Unity-windows-for-mobile/1.png)

# 빌드 오류
빌드가 되지 않는 경우 참고. `특히 Not Found Mediapipe`
Release Note : [MediaPipeUnityPlugin-Releases](https://github.com/homuler/MediaPipeUnityPlugin/releases)   

## 방법 1. (안되더라)
[com.github.homuler.mediapipe-0.11.0](https://github.com/homuler/MediaPipeUnityPlugin/releases/download/v0.11.0/com.github.homuler.mediapipe-0.11.0.tgz)을 다운로드 받고 유니티에서 `Windows - Package Manager - Add package from tarball...`을 선택하여 설치해준다.   
![Untitled](/assets/images/mediapipe/2023-05-10-Mediapipe-plugin-in-Unity-windows-for-mobile/2.png)   

## 방법 2. (O)
`방법 1`을 따라해도 되지 않는 경우, `com.github.homuler.mediapipe-0.11.0.tgz`를 압축풀고 `MediaPipeUnityPlugin-0.11.0\Packages\com.github.homuler.mediapipe\Runtime\Plugins`(Plugins 폴더)를 복사하여 유니티의 똑같은 경로에 붙여넣기 한다.

# 배운점

## Assets\StreamingAssets
Assets폴더에 StreamingAssets이 있어야 한다. 없으면 `com.github.homuler.mediapipe-0.11.0.tgz`를 풀어서 복붙해야 한다.   

# 참고
[homuler MediaPipeUnityPlugin](https://github.com/homuler/MediaPipeUnityPlugin)   
[homuler MediaPipeUnityPlugin Install Guide](https://github.com/homuler/MediaPipeUnityPlugin/wiki/Installation-Guide)   
[내가 수정한 코드](https://github.com/limetimeline/NewMediaPipeUnityPlugin)