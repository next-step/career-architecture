# career-architecture
- 이름: 박상권

## 🚀미션
- 결제 알람(Notification) 기능 개선

### 기대효과 분석
- 결제 기능과 결제 알람 기능에 대한 프로젝트 분리로 알람이 재전송 되는 상황에서 쓰레드 Blocking 방지
- 알람 실패 시 수동으로 알람 재전송하는 업무 개선

### 기술적용 아키텍쳐
flowchart TB
    A[Start] -- 주문 요청 -> B[주문 승인]
    B --> C[알람 데몬 서버에서 알람 전송]
    C -- Success --> D[주문완료]
    C -- Fail --> E[알람 재전송]
    E -- Max retry fail --> F[알람 배치 서버에서 알람 일괄 재전송]
