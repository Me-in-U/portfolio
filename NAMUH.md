<div align="center">

# 🤖 NAMUH (Tori)

**MQTT 기반 실시간 제어 감정 표현 휴머노이드 로봇**

<img src="https://raw.githubusercontent.com/Me-in-U/NAMUH/main/readme-assets/namuh_banner.png" width="100%" style="border-radius: 15px; box-shadow: 0 4px 6px rgba(0,0,0,0.1);">

</div>

---

## ✔️ 프로젝트 개요

- **진행 기간**: 2024.03.05 ~ 2024.06.20 (15 Weeks)
- **팀 구성**: 4명 (Embedded 2, Backend 1, Frontend 1)
- **나의 주요 역할**: Embedded Lead, Backend Developer

> NAMUH (Tori)는 음성 명령과 얼굴 추적을 통해 사용자와 자연스럽게 상호작용하는 감정 표현 휴머노이드 로봇입니다.
>
> ESP32 기반 서보 제어, Raspberry Pi 기반 비전 AI, MQTT 실시간 통신을 결합하여 안정적인 분산 제어 시스템을 구현했습니다.

---

## ✔️ 구현 사항

- **6축 로봇팔 제어**: 역기구학(IK) 기반 17가지 감정 표현 제스처 구현 (`make_heart`, `scissors`, `rock` 등)
- **11+ 감정 표현 모션**: FastLED 16x16 LED 매트릭스로 `Happy`, `Sad`, `Tracking` 등 11가지 표정 구현
- **MQTT 실시간 제어**: Pub/Sub 아키텍처 기반 QoS 1 레벨로 메시지 전달 보장 및 지연 시간 60% 감소
- **OpenCV 얼굴 추적**: Haar Cascade + PID 제어(`Kp=0.25, Ki=0.1, Kd=0.05`)로 실시간 얼굴 추적 (30fps)

---

## ✔️ 담당 역할

> **Embedded Lead** > **(기여도 70%)**

- ESP32 기반 서보/LED 제어 시스템 설계 및 구현
- 6-DOF 로봇팔 17가지 제스처 동작 개발
- PID 제어 알고리즘 구현 및 파라미터 튜닝
- FastLED 기반 16x16 LED 매트릭스 감정 표현 시스템
- 멀티스레딩 동시성 제어 (`threading.Lock` 기반)

> **Backend 개발** > **(기여도 50%)**

- MQTT 브로커 설계 및 통신 프로토콜 정의 (`buriburi/robot/*` 토픽 구조)
- Pub/Sub 아키텍처 기반 4개 노드(ESP32 x2, Raspberry Pi x2) 통신 구현
- QoS 레벨 설정 및 메시지 신뢰성 보장
- Raspberry Pi 메인 제어 시스템 개발 (얼굴 추적 + 음성 인식 통합)

---

## ✔️ 기술 스택

<div align="center">

**Embedded Systems**

![ESP32](https://img.shields.io/badge/ESP32-E7352C?style=for-the-badge&logo=espressif&logoColor=white)
![Raspberry Pi](https://img.shields.io/badge/Raspberry_Pi_5-C51A4A?style=for-the-badge&logo=raspberry-pi&logoColor=white)
![Arduino](https://img.shields.io/badge/Arduino-00979D?style=for-the-badge&logo=arduino&logoColor=white)
![C++](https://img.shields.io/badge/C++-00599C?style=for-the-badge&logo=c%2B%2B&logoColor=white)

**Control & AI**

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white)
![PID Control](https://img.shields.io/badge/PID_Control-FF6B6B?style=for-the-badge)
![Picovoice](https://img.shields.io/badge/Picovoice-00C7B7?style=for-the-badge)

**Communication**

![MQTT](https://img.shields.io/badge/MQTT-660066?style=for-the-badge&logo=mqtt&logoColor=white)
![Mosquitto](https://img.shields.io/badge/Mosquitto-3C5280?style=for-the-badge&logo=eclipsemosquitto&logoColor=white)

**Hardware**

![DOFBOT](https://img.shields.io/badge/DOFBOT_6DOF-FF9900?style=for-the-badge)
![FastLED](https://img.shields.io/badge/FastLED-00979D?style=for-the-badge)
![PCA9685](https://img.shields.io/badge/PCA9685_PWM-4285F4?style=for-the-badge)

</div>

---

## ✔️ 기술 선정 이유

- **MQTT (Mosquitto)**:

  - ESP32/Raspberry Pi 간 경량화된 메시지 전송 필요
  - Pub/Sub 구조로 4개 노드 간 효율적인 분산 통신 구현
  - QoS 레벨 설정으로 중요 명령은 전달 보장, 센서 데이터는 저지연 전송 가능

- **PID 제어**:

  - 비례 제어만으로는 오버슈트(20~30%)와 진동 발생
  - PID 제어 도입으로 안정적인 얼굴 추적 및 정상 상태 오차 ±2° 이내 달성
  - 실시간 피드백 루프 구현 필요

- **OpenCV Haar Cascade**:

  - Raspberry Pi의 제한된 리소스에서도 30fps 실시간 처리 가능
  - 사전 학습된 모델로 빠른 개발 가능
  - ROI 설정 및 다운스케일링으로 성능 최적화 용이

- **FastLED**:
  - WS2812B LED 스트립 고속 제어 가능 (256개 LED 동시 제어)
  - 다양한 색상 및 애니메이션 효과 구현 가능
  - ESP32와 호환성 우수

---

## ✔️ 프로젝트 성과

### **1. PID 제어 기반 안정적 얼굴 추적 달성**

**😭 [문제 상황]:**

- 초기 비례 제어(P-Control)만 사용 시 다음 문제 발생:
  - **오버슈트 심각**: 목표 각도 도달 후 20~30% 초과 회전
  - **진동(Oscillation) 발생**: 목표 위치 주변에서 지속적인 흔들림 (±10°)
  - **정상 상태 오차**: ±10° 이상의 오차 누적으로 얼굴 추적 실패

**🧐 [해결 과정]:**

1. **PositionalPID 알고리즘 구현**:

   ```python
   # BotControl/commands/face_tracking/PID.py
   class PositionalPID:
       def __init__(self, Kp=0.25, Ki=0.1, Kd=0.05):
           self.Kp = Kp  # 비례 게인
           self.Ki = Ki  # 적분 게인
           self.Kd = Kd  # 미분 게인
   ```

2. **파라미터 튜닝 과정**:

   - 초기값: `Kp=1.0, Ki=0, Kd=0` → 오버슈트 30%
   - 1차 튜닝: `Kp=0.5, Ki=0.05, Kd=0.02` → 오버슈트 15%
   - **최종값: `Kp=0.25, Ki=0.1, Kd=0.05`** → 오버슈트 5% 이하

3. **제어 로직 통합**:
   - **P(비례) 제어**: 현재 오차에 비례하여 제어 → 빠른 반응
   - **I(적분) 제어**: 누적 오차 보상 → 정상 상태 오차 제거
   - **D(미분) 제어**: 오차 변화율 기반 감쇠 → 오버슈트 방지

**🥳 [성과]:**

| 지표               | 비례 제어 (Before) | PID 제어 (After) | 개선율       |
| ------------------ | ------------------ | ---------------- | ------------ |
| **오버슈트**       | 20~30%             | 5% 이하          | **70% 감소** |
| **정상 상태 오차** | ±10°               | ±2°              | **80% 감소** |
| **안정화 시간**    | 1.5초              | 0.5초            | **67% 단축** |
| **진동 발생**      | 심각               | 미미             | **90% 감소** |

---

### **2. 멀티스레딩 동시성 제어로 시스템 안정성 확보**

**😭 [문제 상황]:**

- 얼굴 추적 스레드와 MQTT 제어 스레드가 동시에 서보를 조작하여 발생한 문제:
  - **Race Condition**: 2개 스레드가 동시에 서보 각도 변경 시도
  - **서보 충돌**: 상반된 명령으로 인한 비정상 동작 (떨림, 소음)
  - **명령 손실**: MQTT 제어 명령이 얼굴 추적에 의해 무시됨

**🧐 [해결 과정]:**

1. **threading.Lock 기반 동기화 메커니즘**:

   ```python
   # BotControl/commands/face_tracking/controller.py
   servo_lock = threading.Lock()
   _arm_io_lock = threading.Lock()

   # 얼굴 추적 스레드
   def face_tracking_thread():
       while True:
           faces = detect_faces(frame)
           if faces:
               with servo_lock:  # Critical Section
                   update_servo_angles(faces[0])

   # MQTT 제어 스레드
   def mqtt_command_thread():
       while True:
           command = mqtt_queue.get()
           with servo_lock:  # Critical Section
               execute_gesture(command)
   ```

2. **명령 우선순위 시스템 구현**:

   ```python
   # MQTT 제어가 항상 우선
   if mqtt_queue.qsize() > 0:
       with servo_lock:
           execute_mqtt_command()
   else:
       # MQTT 명령 없을 때만 얼굴 추적
       if face_detected:
           with servo_lock:
               track_face()
   ```

3. **queue.Queue 기반 명령 버퍼링**:
   - MQTT 명령을 큐에 저장하여 순차 처리
   - 명령 손실 방지 및 처리 순서 보장

**🥳 [성과]:**

- **Race Condition**: 100% 해결 (Lock 기반 동기화)
- **명령 충돌**: 완전 제거
- **MQTT 제어 우선순위**: 100% 보장
- **시스템 안정성**: 24시간 연속 운영 테스트 통과

---

### **3. MQTT 프로토콜 도입으로 통신 효율 대폭 향상**

**😭 [문제 상황]:**

- 초기 HTTP Polling 방식의 문제점:
  - **높은 지연 시간**: 평균 200ms (Polling 주기 100ms + 처리 시간)
  - **불필요한 트래픽**: 빈 응답 반복 수신 (전체 트래픽의 80% 이상)
  - **메시지 손실**: WiFi 불안정 시 5~10% 손실률

**🧐 [해결 과정]:**

1. **MQTT Pub/Sub 아키텍처 전환**: 이벤트 기반 통신으로 불필요한 Polling 제거
2. **QoS 레벨 차등 적용**: 중요 명령(QoS 1), 센서 데이터(QoS 0)로 분리
3. **자동 재연결 메커니즘**: WiFi 불안정 시 5초 간격 재연결

**🥳 [성과]:**

- **명령 응답 지연**: 200ms → 80ms (**60% 감소**)
- **네트워크 트래픽**: 40 req/s → 5~10 msg/s (**75% 절감**)
- **메시지 손실률**: 5~10% → 0% (**완전 해결**)
- **4개 노드 동시 연결** 안정적 동작

---

## ✔️ 프로젝트 리뷰

### **🔍 개선 가능성**

1. **ROS (Robot Operating System) 미적용**

   - PID 제어와 MQTT 통신으로 안정적인 시스템을 구축했으나, **ROS를 도입했다면** 더 체계적인 로봇 제어 시스템을 구축할 수 있었을 것입니다
   - ROS의 **tf(Transform) 라이브러리**를 활용했다면 3D 공간 좌표 변환이 더 정확했을 것이며, **rosbag**을 통한 센서 데이터 기록/재생으로 디버깅 효율을 크게 높일 수 있었을 것입니다
   - ROS의 **Navigation Stack**을 활용하면 실내 자율 주행 기능도 추가할 수 있었을 것

2. **강화학습 미적용**

   - 현재는 사전 정의된 17가지 제스처만 수행 가능하지만, **강화학습(Reinforcement Learning)**을 적용했다면 다음이 가능했을 것입니다:
     - 사용자 반응에 따른 **자율적인 감정 표현 학습**
     - 상황별 **최적의 제스처 선택** (예: 슬픈 표정 감지 시 위로 동작)
     - **지속적인 성능 개선** (사용자 피드백 기반)

3. **Vision AI 고도화 부족**
   - Haar Cascade는 빠르지만 정확도가 낮아, **YOLO 또는 MediaPipe**를 활용했다면:
     - 얼굴 표정 인식으로 사용자 감정 파악 가능
     - 손 제스처 인식으로 비접촉 제어 가능
     - 여러 사람 동시 추적 및 우선순위 선택 가능

### **✨ 배운 점**

1. **제어 공학 이론의 실전 적용**

   - PID 제어 이론을 실제 임베디드 시스템에 적용하며 **제어 공학의 중요성**을 체감했습니다. `Kp`, `Ki`, `Kd` 파라미터 튜닝 과정에서 시행착오를 겪으며 **실시간 피드백 루프의 동작 원리**를 깊이 이해할 수 있었습니다.

2. **멀티스레딩 및 동시성 제어**

   - 멀티스레딩 환경에서 **Race Condition**을 직접 경험하고 해결하며 **동시성 제어 역량**을 크게 향상시켰습니다. `threading.Lock`, `queue.Queue` 등의 동기화 메커니즘을 실전에서 활용하며 안정적인 시스템 설계 방법을 습득했습니다.

3. **분산 시스템 아키텍처 설계**

   - MQTT 프로토콜 설계를 통해 **분산 시스템 아키텍처의 효율적인 설계 방법**을 습득했습니다. Pub/Sub 패턴, QoS 레벨 선택, 토픽 구조 설계 등 IoT 시스템 설계의 핵심 개념을 실전에서 적용할 수 있었습니다.

4. **임베디드 시스템 성능 최적화**

   - Raspberry Pi와 ESP32의 **제한된 리소스 환경**에서 최적의 성능을 내기 위한 다양한 최적화 기법을 학습했습니다:
     - ROI 설정 및 다운스케일링으로 프레임 처리 속도 36% 향상
     - FastLED 라이브러리 활용으로 256개 LED 동시 제어
     - MQTT QoS 레벨 차등 적용으로 네트워크 트래픽 75% 절감

5. **Git 협업 워크플로우 확립**
   - 4인 팀 프로젝트를 통해 Git을 활용한 본격적인 협업 경험을 쌓았습니다:
     - **브랜치 전략 수립**: feature/develop/main 3단계 브랜치 전략 적용
     - **Pull Request(PR) 프로세스**: 코드 리뷰 및 충돌 해결 방법 학습
     - **커밋 컨벤션**: Conventional Commits 방식으로 명확한 커밋 메시지 작성

---

<div align="center">

**💬 NAMUH 프로젝트를 통해 임베디드 시스템, 제어 공학, 분산 시스템 아키텍처를 실전에서 적용하며 성장할 수 있었습니다!**

[![GitHub](https://img.shields.io/badge/GitHub-NAMUH-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Me-in-U/NAMUH)

</div>
