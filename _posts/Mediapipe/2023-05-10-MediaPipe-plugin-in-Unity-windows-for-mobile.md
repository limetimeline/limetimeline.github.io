---
title:  "Mediapipe plugin in Unity(Windows) - Build Android"
excerpt: "미디어파이프 플러그인을 이용한 유니티에서 안드로이드 빌드."
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
이전 페이지의 [Mediapipe-plugin-in-Unity](https://limetimeline.github.io/mediapipe/Mediapipe-plugin-in-Unity/)를 참고하여 설치한다.   

# 빌드
Hand Tracking 위주로 진행하였다.   

## Bootstrap Asset Loader Type - Streaming Asset
원하는 솔루션의 Scene을 선택하여 Bootstrap 스크립트의 Asset Loader Type을 `Streaming Asset`으로 변경해준다.   
![Untitled](/assets/images/mediapipe/2023-05-10-Mediapipe-plugin-in-Unity-windows-for-mobile/1.png)

# 빌드 오류
빌드가 되지 않는 경우 참고.   
Release Note : [MediaPipeUnityPlugin-Releases](https://github.com/homuler/MediaPipeUnityPlugin/releases)   

## 방법 1.
[com.github.homuler.mediapipe-0.11.0](https://github.com/homuler/MediaPipeUnityPlugin/releases/download/v0.11.0/com.github.homuler.mediapipe-0.11.0.tgz)을 다운로드 받고 유니티에서 `Windows - Package Manager - Add package from tarball...`을 선택하여 설치해준다.   
![Untitled](/assets/images/mediapipe/2023-05-10-Mediapipe-plugin-in-Unity-windows-for-mobile/2.png)   

## 방법 2.
`방법 1`을 따라해도 되지 않는 경우, `com.github.homuler.mediapipe-0.11.0.tgz`를 압축풀고 `MediaPipeUnityPlugin-0.11.0\Packages\com.github.homuler.mediapipe\Runtime\Plugins`(Plugins 폴더)를 복사하여 유니티의 똑같은 경로에 붙여넣기 한다.

# 배운점

## Assets\StreamingAssets
Assets폴더에 StreamingAssets이 있어야 한다. 없으면 `com.github.homuler.mediapipe-0.11.0.tgz`를 풀어서 복붙해야 한다.   

# 참고
[homuler MediaPipeUnityPlugin](https://github.com/homuler/MediaPipeUnityPlugin)   
[homuler MediaPipeUnityPlugin Install Guide](https://github.com/homuler/MediaPipeUnityPlugin/wiki/Installation-Guide)   
[내가 수정한 코드](https://github.com/limetimeline/NewMediaPipeUnityPlugin)
