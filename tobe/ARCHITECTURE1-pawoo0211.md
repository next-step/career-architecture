# career-architecture
- 이름: 박상권

## 🚀미션
- 결제 취소 기능 마이그레이션

### 기대효과 분석
- JSP와 JDBC Template을 이용한 코드에서 스프링 부트와 MyBatis, JPA 적용
- 메서드 하나에 존재하는 수백 라인의 코드 단축으로 인한 가시성 향상
- 취소 기능에 대한 유지 보수성 증가
- 새로운 결제 수단이 추가될 때마다 공통화 된 취소 기능을 사용하여 빠른 개발 진행

### 기술적용 아키텍쳐
flowchart TB
    A,B[Spring boot]
    C,D[JPA]
    E,F,G[Spring boot]
    H[My Batis]

    A[Start] -- 결제 취소 요청 -> B[가맹점 요청 파라미터 검사]
    B --> C[가맹점 조회]
    C --> D[승인 거래 조회]
    C --> E[취소 금액 계산]
    E --> F[결제 취소 진행]
    F --> G[결제사 측으로 결제 취소 API 요청]
    H --> H[취소 API 응답에 대한 DB 후 처리 진행]
