# 기술 스택 요약

## **Java & Spring Boot** 🟥🟥🟥🟥🟥

- **QueryDSL 동적 쿼리**
  → BooleanBuilder로 8개 필터 조건 조합 (카테고리, 가격, 지역 등)
  → **경매 목록**: LEFT JOIN + GROUP BY로 입찰 수 집계하여 N+1 문제 해결
  → **공지사항 상세**: fetchJoin()으로 작성자 정보 한 번에 조회
- **WebSocket(STOMP) 실시간 통신**
  → 특정 사용자에게 메시지 전송
  → Redis로 서버 간 동기화, @Async 비동기 처리
- **Spring Batch & Scheduler**: 경매 종료 자동화, @Transactional 롤백 보장
- **REST API 아키텍처**: JWT 인증, AOP 로깅, 전역 예외 처리, Swagger 문서화

---

## **Python & Django** 🟥🟥🟥🟥⬜

- **Django Channels 기반 실시간 WebSocket 서비스 구축**
  → Raspberry Pi ↔ Django ↔ Frontend 양방향 통신, async_to_sync DB 최적화
- **Python 크롤링 & 데이터 수집 자동화**
  → Selenium + BeautifulSoup 네이버 부동산 크롤링 (4만+ 건 처리)
  → Spring Scheduler + ProcessBuilder로 Python 스크립트 주기 실행
- **EasyOCR 성능 최적화**
  → YOLO 검출 + ROI 확장 + 다운스케일링으로 처리 시간 10배 향상 (8.3s → 0.8s)
- **Picamera2 듀얼 스트림**
  → 고해상도(4608x2592) + 저해상도(2304x1296) 동시 캡처, libcamera 직접 제어
- **FastAPI MQTT 서버**
  → ESP32/Raspberry Pi 중계 허브, 4개 토픽 분리로 트래픽 75% 절감

---

## **Embedded & AIoT & Robotics** 🟥🟥🟥🟥⬜

- **15DOF 서보 기반 휴머노이드 로봇 전신 관절 제어**
  → 6축 로봇팔 17가지 제스처, 목/허리 3축 제어, Easing 애니메이션
  → FastLED 16x16 WS2812B LED 표정 30fps 렌더링
- **Autodesk Fusion 360 모델링 & 3D 프린팅**
  → 휴머노이드 외형 제작 (머리/몸통/팔/다리), IoT 장비 하우징 제작
- **ESP32 + E-INK Display 시스템 개발**
  → 백엔드 서버 연동 작업 상태 실시간 표시, WiFi + HTTP 폴링
- **ESP32 기반 환경 센서 통합 (eCO₂, TVOC, 온습도 등)**
  → IoT 서버와 자동화 기능 연동 (스케줄·알림·대시보드 표시)
- **MQTT 아키텍처 설계**
  → 토픽 분리로 트래픽 75% 절감, QoS 레벨별 우선순위 관리
- **PID 제어 알고리즘**
  → OpenCV 얼굴 검출 + PID 서보 제어, 오버슈트 70% 감소

---

## **AI & Reinforcement Learning** 🟥🟥🟥⬜⬜

- **Isaac Sim/Isaac Lab 기반 휴머노이드 강화학습**
  → USD Stage 물리 시뮬레이션, DofbotPickPlaceEnvV2/V3 커리큘럼 학습 (5-stage)
  → Action space 6 (5 arm joints + gripper), Observation space 23 (joint pos/vel, ee_pos, obj_pos, goal_pos)
- **YOLOv11s-OBB + EasyOCR 하이브리드 인식 시스템**
  → 회전 바운딩 박스 검출, ROI 확장 + 333px 다운스케일링으로 처리 시간 10배 향상 (8.3s → 0.8s)
  → QWERTY 키보드 레이아웃 Levenshtein 보정으로 EasyOCR 정확도 10배 향상
- **Multilingual CLIP + Faiss 벡터 검색 엔진**
  → 50개 언어 지원, 코사인 유사도 기반 실시간 검색 (PyInstaller EXE 배포, GitHub Release 6회)
- **OpenAI Whisper STT & 자동 YouTube 요약 디스코드 봇**
  → Whisper tiny 모델로 음성인식, YouTube 자막 크롤링, ChatGPT API로 5분 요약 생성
  → yt-dlp로 오디오 다운로드, YouTube Data API로 댓글 수집 (최대 40개)
