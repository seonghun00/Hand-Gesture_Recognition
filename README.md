<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=auto&height=80&section=header" width="100%" />
</p>

# ✋ On-Device Real-Time Custom Hand Gesture Recognition
> **구글 MediaPipe를 활용하여 나만의 손 제스처를 학습시키고 기기에서 실시간으로 구동하는 시스템입니다.**

<p align="left">
  <img src="https://img.shields.io/badge/License-MIT-yellow?logo=opensourceinitiative&logoColor=white">
  <img src="https://img.shields.io/badge/TensorFlow_Lite-FF6F00?logo=tensorflow&logoColor=white">
  <img src="https://img.shields.io/badge/MediaPipe-0082FC?logo=google&logoColor=white">
</p>

본 프로젝트는 기존 HGR(Hand Gesture Recognition) 시스템의 고정된 손동작 인식 한계를 넘어, 사용자가 원하는 커스텀 제스처를 직접 빌드하고 스마트폰이나 PC 환경에서 실시간으로 실행하는 것이 목적입니다.

---

## 🛠️ 기술 스택 (Tech Stack)

* **Core Framework**: Google MediaPipe (Hands & Tasks API)
* **Environment**: Google Colaboratory (Python / GPU 가속)
* **Deployment Format**: TensorFlow Lite Runtime (`.task` 파일)
* **Reference System**: Android Studio / Web Playgrounds

---

## 📺 결과 영상 (Demo)

### 1. 실시간 손가락 랜드마크 추적 (Core Landmark Tracking)
> 기본 랜드마크 모델을 사용하여 손가락의 21개 관절 포인트를 실시간으로 추적하는 모델입니다.

<p align="left">
  <img width="600" src="./assets/result_images/landmark_tracking.gif" alt="Hand Landmark Tracking" />
</p>

### 2. 최종 온디바이스 제스처 인식 애플리케이션 (Final Custom H.G.R App)
> 커스텀 데이터를 학습시킨 모델을 스마트폰에 이식하여, 가위바위보(Sissor, Rock, Paper) 제스처를 실시간으로 인식하는 결과영상입니다. 화면 하단에 인식된 제스처명이 출력됩니다.

<p align="left">
  <img width="350" src="./assets/result_images/final_result.gif" alt="Final Gesture Recognition App" />
</p>

---

## 🔗 Quick Links (핵심 주소 바로가기)

* 💻 **구글 공식 웹 플레이그라운드** ➔ [MediaPipe 실시간 웹 데모 사이트](https://google-ai-edge.github.io/mediapipe-samples-web/)
* 🚀 **구글 공식 코랩 노트북** ➔ [MediaPipe Gesture Recognizer Colab](https://colab.research.google.com/github/googlesamples/mediapipe/blob/main/examples/gesture_recognizer/python/gesture_recognizer.ipynb)
* 📄 **핵심 참조 연구 논문 (ICCVW 2023)** ➔ [On-device Real-time Custom Hand Gesture Recognition](https://ieeexplore.ieee.org/document/10350551)

---

## 🚀 How to Use

나만의 손 제스처 모델을 구축하고 구동하는 전체 프로세스는 다음의 **3단계**로 진행됩니다.

### 📂 1단계: 커스텀 데이터셋 준비 (Data Collection)
* **목적**: 인공지능이 새로운 손 모양을 구별할 수 있도록 기준 사진을 수집합니다.
* **방법**:
  * 인식시키고자 하는 나만의 손 모양(예: 가위, 바위, 보 등)을 정합니다.
  * 각 제스처당 **최소 50장 이상** 다양한 각도, 거리, 배경에서 사진을 촬영합니다.
  * 사진 데이터는 프로젝트 내 `assets/images/` 디렉토리에 분류하여 관리하면 편합니다.
  * ⚠️ *주의: 데이터셋의 양과 다양성이 확보되지 않으면 오인식률이 높아집니다.*

### 💻 2단계: Google Colab을 통한 모델 학습 (Model Training)
* **목적**: 준비한 사진들을 인공지능에게 가르치고 가벼운 실행 파일(`.task`)로 변환합니다.
* **방법**:
  1. [구글 공식 코랩 노트북](https://colab.research.google.com/github/googlesamples/mediapipe/blob/main/examples/gesture_recognizer/python/gesture_recognizer.ipynb) 환경에 접속합니다[cite: 1].
  2. 상단 메뉴 **[런타임] ➔ [런타임 유형 변경]**에서 하드웨어 가속기를 **`T4 GPU`**로 세팅합니다.
  3. 노트북에 적힌 `mediapipe-model-maker` 패키지 설치부터 데이터 로드 코드 블록의 `[ ▶ ]` 버튼을 순서대로 실행합니다.
  4. 학습이 끝나면 기기에 이식할 핵심 가속 모델인 **`gesture_recognizer.task`** 파일을 다운로드합니다.

### 📱 3단계: 웹 및 앱 기기 이식 및 실시간 구동 (Inference)
* **목적**: 완성된 모델을 비전 파이프라인에 연결하여 카메라 화면으로 제스처를 실시간 추적합니다.
* **방법**:
  1. 다운로드한 `.task` 모델 파일을 실행 프로그램의 리소스 폴더에 배치합니다.
  2. [MediaPipe 실시간 웹 데모 사이트](https://google-ai-edge.github.io/mediapipe-samples-web/)에 접속하여 왼쪽 메뉴에서 **`Gesture Recognizer`**를 선택합니다.
  3. **Running Mode**를 `Live stream (Webcam)`으로 바꾸고, **Delegate** 설정을 `GPU`로 선택합니다.
  4. 카메라 권한을 승인한 뒤 화면에 손을 올리면, `MediaPipe Hands` 알고리즘이 손가락 관절 21개 포인트를 실시간 추적하며 어떤 제스처인지 결과값을 즉시 출력합니다.

---

## ⚠️ 확인된 한계점 및 보완 요망 (Limitations)

* **정적 프레임 기반 학습**: 움직이는 동적 모션(Gesture) 분석이 아닌, 순간적으로 멈춰 있는 정적 프레임 속 손가락 기하학 구조를 판별하므로 연속 동작 인식에는 한계가 있습니다.
* **손 겹침 현상 (Occlusion)**: 카메라 시야 각도에 따라 손가락이 서로 겹치거나 가려지면 관절 좌표(`Landmarks`) 추적 궤적이 깨지는 현상이 발생할 수 있습니다.
* **조도 및 배경 환경 영향**: 어두운 환경이나 손 색상과 유사한 복잡한 배경에서는 인식 능력이 다소 저하될 수 있으므로 선명한 입력 데이터 확보가 권장됩니다.

---
© 2026 Custom H.G.R Team. All Rights Reserved.
