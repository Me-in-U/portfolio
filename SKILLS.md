# Skills

## Java & Spring Boot (5칸)

### P2P 경매 REST API 설계 (Tako)

- **QueryDSL 동적 쿼리**: 8개 필터(카테고리, 가격, 등급 등) 조합 경매 검색 구현
- **페이지네이션 최적화**: LEFT JOIN + GROUP BY로 입찰수 집계 (서브쿼리 제거)
- **프로젝션 매핑**: `AuctionListProjection` DTO로 불필요한 필드 조회 방지

### Spring Security + JWT 인증 (Tako)

- **Access/Refresh Token**: JWT 기반 Stateless 인증/인가 구현
- **CustomUserDetails**: UUID 기반 사용자 정보 추출 및 권한 관리
- **Password Encoder**: BCrypt 해시로 비밀번호 암호화 저장

### JPA + QueryDSL 쿼리 최적화 (Tako)

- **N+1 문제 해결**: `@EntityGraph`, Fetch Join으로 연관 엔티티 일괄 로딩
- **동적 where절**: `BooleanBuilder`로 null-safe 필터 조건 구성
- **정렬 타이브레이커**: `OrderSpecifier`로 2차 정렬 기준(id DESC) 적용

### WebSocket 실시간 입찰 (Tako)

- **STOMP 프로토콜**: Spring WebSocket + SockJS로 실시간 입찰 브로드캐스팅
- **Redis Lua 스크립트**: 입찰 경합 원자성 보장 (동시성 제어)
- **비동기 DB 반영**: `@Async`로 입찰 검증 후 비동기 저장

### Spring Batch 정산 자동화 (Tako)

- **낙찰 후 정산**: 경매 종료 → NFT 전송 → 에스크로 해제 자동화
- **트랜잭션 관리**: `@Transactional`로 정산 실패 시 롤백 보장
- **스케줄링**: Cron 표현식으로 정기 Batch 작업 실행

---

## Python & Django (5칸)

### Django Channels WebSocket (Alpacar)

- **실시간 6개 노드 통신**: Raspberry Pi → Django → Frontend 양방향 통신 구현
- **비동기 Consumer**: `async_to_sync`, `database_sync_to_async`로 DB 조회 최적화
- **그룹 브로드캐스팅**: `channel_layer.group_send`로 주차 상태 변경 실시간 전파
- **ASGI 라우팅**: `ProtocolTypeRouter`로 HTTP/WebSocket 트래픽 분리

### EasyOCR 성능 최적화 (Alpacar)

- **ROI 전처리**: YOLO 검출 → 좌5%/우10%/상하10% 확장 → 333px 다운스케일링
- **처리 시간 단축**: 8.3s → 0.8s (10배 향상) - 전처리 파이프라인 최적화
- **한글 인식**: EasyOCR Korean 모델 + Grayscale + MedianBlur 전처리
- **정규식 검증**: 번호판 패턴(지역/한글/숫자) 매칭으로 유효성 검사

### Picamera2 듀얼 스트림 (Alpacar)

- **동시 캡처**: 고해상도(4608x2592) + 저해상도(2304x1296) 듀얼 스트림 구성
- **libcamera 제어**: AfMode Manual, LensPosition, FrameRate 직접 설정
- **줌/플립 변환**: 중앙 크롭 2배 줌 + 좌우 반전으로 실시간 프레임 변환

### FastAPI MQTT 서버 (NAMUH)

- **IoT 통신 허브**: ESP32, Raspberry Pi 간 MQTT 메시지 중계 서버 구현
- **토픽 분리**: `buriburi/robot/{rx,tx,joint,event}` 4개 토픽으로 트래픽 75% 절감
- **JSON Pub/Sub**: QoS 레벨별 메시지 우선순위 관리 (명령 QoS1, 이벤트 QoS0)

### PID 제어 알고리즘 (NAMUH)

- **얼굴 추적 서보 제어**: OpenCV Haar Cascade 검출 → PID 계산 → 서보 각도 제어
- **파라미터 튜닝**: Kp=0.25, Ki=0.1, Kd=0.05로 오버슈트 70% 감소
- **Dead Zone 필터링**: 중앙(260~380, 180~300) 영역 무시로 미세 떨림 제거

---

## Embedded Robotics (4칸)

### ESP32 C++ 로봇 제어 (NAMUH)

- **서보 모터 제어**: ESP32Servo로 6축 로봇팔 17가지 제스처 구현
- **FastLED 감정 표현**: 16x16 WS2812B 매트릭스로 11가지 LED 표정 30fps 렌더링
- **MQTT Client**: PubSubClient로 WiFi 기반 명령 수신 및 JSON 파싱
- **상태 머신**: 17가지 액션별 step 관리 (Hello, Heart, Hug, Tracking 등)

### Raspberry Pi 통합 제어 (Alpacar, NAMUH)

- **GPIO 제어**: LED, 서보, VL53L0X 거리센서 통합 제어로 자동 차단기 구현
- **멀티스레딩**: Picamera2 듀얼 스트림 + YOLO 병렬 처리로 지연 0%
- **Python Serial**: PySerial로 로봇팔 UART 통신 (Arm_Lib 래퍼 사용)

### MQTT Broker 토픽 설계 (NAMUH)

- **4개 분리 토픽**: `rx`(명령 수신), `tx`(이벤트 전송), `joint`(관절 제어), `event`(상태 알림)
- **트래픽 절감**: 토픽 분리로 불필요한 구독 제거 → 트래픽 75% 감소
- **QoS 레벨 관리**: 명령(QoS 1 보장), 이벤트(QoS 0 베스트에포트)

### PID + 스무딩 필터 (NAMUH)

- **Positional PID**: 목표 좌표와 현재 서보 각도 차이 계산 → PID 출력
- **스무딩 필터**: `alpha=0.25` Low-pass 필터로 급격한 서보 움직임 완화
- **범위 제한**: `_clamp` 함수로 서보 각도 범위(s1: 30~160°, s2: 40~170°) 강제

---

## AI (3.5칸)

### 임베딩 기술 활용 자연어 기반 사진 검색

- **Vision Transformer**: CLIP 모델로 이미지 임베딩 벡터 생성
- **벡터 DB**: Faiss 또는 Qdrant로 유사도 검색 구현
- **자연어 쿼리**: 텍스트 임베딩과 이미지 임베딩 간 코사인 유사도 계산

### 번호판 탐지 및 OCR (Alpacar)

- **YOLO11s-OBB**: Oriented Bounding Box로 회전된 번호판 정확 검출
- **confidence=0.5**: 검출 임계값으로 False Positive 제거
- **ROI 추출**: 검출 박스 확장 (좌5%/우10%/상하10%) 후 OCR 영역 전달

### 전처리 파이프라인 (Alpacar, NAMUH)

- **Grayscale + Blur**: `cv2.cvtColor` + `cv2.medianBlur(3)` 노이즈 제거
- **다운스케일링**: 4608x2592 → 333px 너비 (비율 유지) → OCR 처리 시간 36% 향상
- **Haar Cascade**: 얼굴 검출 30fps 유지 (640x480, scaleFactor=1.3, minNeighbors=5)
