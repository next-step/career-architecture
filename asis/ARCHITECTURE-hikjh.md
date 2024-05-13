# career-architecture
> mermaid로 작성된 과제는 마크다운 파일(ARCHITECTURE.md)로 올려주시면 됩니다. (md 파일 내에 기존 구조를 넣어주세요)<br>
> 별도 아키택쳐나 모델링 도구를 사용한 경우에는 마크다운 파일(ARCHITECTURE.md)과 png, gif, jpg, pdf 파일 형식으로 architecture-{gitID}.png 파일명으로 upload 해주세요
# 요구사항
- [ ] 담당 하는 업무에서 비효율적인 프로세스나 기술적 개선을 하고 싶은 부분의 현재 구조를 문서화 한다.
    - [ ] 비효율적인 부분에 대한 분석내용을 정리한다.
    - [ ] 비효율적인 부분에 대한 프로세스 또는 시스템 구조를 그려본다.


## 🚀미션
- 이름 : 김재현
### 개선포인트 분석
- DTO, VO, Entity 혼용되어 하나의 클래스에 너무 많은 책임이 부여되어 있음
- PDF 변환 작업시 setter의 무분별한 사용으로 인해 어떤 부분에서 데이터가 변하는지 파악하기 어려움
- 우편 접수 실패시 처리 필요
- 불필요한 RabbitMQ 사용

### 프로세스
```mermaid
flowchart LR
classDef orange fill:#f08c00,stroke-width:0px,color:#fff
classDef green fill:#40c057,stroke-width:0px,color:#fff
classDef yellow fill:#ffd43b,stroke-width:0px,color:#fff
classDef blue fill:#1971c2,stroke-width:0px,color:#fff

    %% orange: 서비스 제공자, 서비스 대상자
    %% green: 우리가 제공하는 서비스
    %% blue: 우리가 관리하는 저장소
    %% yellow: 외부 서비스
    
    Member:::orange
    UnionRegister:::orange
    OMS:::green
    Scheduler:::green
    DB:::blue
    S3:::blue
    MQ:::blue
    ThirdParty1:::yellow
    ThirdParty2:::yellow
    
    Member ---> |1.우편 서식 미리보기| OMS
    OMS --> |2.PDF 변환| OMS
    OMS --> |3.PDF 업로드| S3
    OMS --> |4.File 저장| DB

    Member ---> |5.우편 발송| OMS
    OMS ---> |6.File 조회| DB
    OMS ---> |7.PDF 다운로드| S3
    OMS ---> |8.데이터 세팅 및 비용 계산| OMS
    OMS ---> |9.우편 발송 및 상세 내역 저장| DB
    OMS --> |10.우편 제작 발송 요청| ThirdParty1
    ThirdParty1 --> |11.우편 제작 발송 응답| OMS

    Scheduler ---> |12.우편 접수 상태 조회| DB
    Scheduler --> |13.우편 접수 상태 및 등기번호 업데이트| DB
    
    Scheduler --> |14.우편 등기 번호 조회| DB
    Scheduler --> |15.우편 등기 번호 Publish| MQ
    MQ --> |16.우편 등기 번호 Consumer| Scheduler
    Scheduler --> |17.등기 번호로 우편 배달 상태 조회| ThirdParty2
    ThirdParty2 --> |18.등기 번호로 우편 배달 상태 조회| Scheduler
    Scheduler --> |19.우편 배달 상태 업데이트| DB
    Scheduler --> |20.문자 알림 이벤트| OMS
    OMS --> |21.문자 알림| UnionRegister
```
