# career-architecture
> mermaid로 작성된 과제는 마크다운 파일(ARCHITECTURE.md)로 올려주시면 됩니다. (md 파일 내에 기존 구조를 넣어주세요)<br>
> 별도 아키택쳐나 모델링 도구를 사용한 경우에는 마크다운 파일(ARCHITECTURE.md)과 png, gif, jpg, pdf 파일 형식으로 architecture-{gitID}.png 파일명으로 upload 해주세요
# 요구사항
- [ ] 담당 하는 업무에서 비효율적인 프로세스나 기술적 개선을 하고 싶은 부분의 현재 구조를 문서화 한다.
    - [ ] 비효율적인 부분에 대한 분석내용을 정리한다.
    - [ ] 비효율적인 부분에 대한 프로세스 또는 시스템 구조를 그려본다.


## 🚀미션
- 이름 : 이병덕
### 개선포인트 분석
- CMS에서 조합원 명부 승인 시 엑셀 업로드를 하여 명부 등록한다. 이 때 주소 검토가 필수적으로 들어가는데 주소 검토 실패가 많다.
- 주소 검토 실패가 발생하여 CMS 등록된 명부 목록 중 1건씩 직접 주소 재검토를 위해 주소 검토 버튼을 해야한다.
- 주소 검토는 Third Party를 사용하는데 Third Party의 문제이다. 현재 1건씩 검토를 해도 30%확률로 실패한다.
- Third Party에서는 해결을 해주지 않는 상황으로 추후 다른 Third Party로 바꿀 예정이지만 현재는 바꾸기엔
- 만약 명부 5천 건 중 1천 건이 주소 검토를 실패하면 1건 씩 주소 검토 버튼을 클릭해야 한다.
- 현재는 유저가 많지 않아서 명부 승인할 일이 많지는 않은 상태임에도 매번 승인 시 마다 재검토 시간으로 1시간이 낭비된다.
- 앞으로 유저가 많아지면 하루에도 몇 번씩 명부 승인을 할텐데 수 시간으로 늘어날 것이다.

### 프로세스
```mermaid
flowchart TD
	%% Colors %%
	classDef green fill:#137433,stroke-width:0px,color:#fff
	classDef red fill:#FF3399,stroke-width:0px,color:#fff
	classDef violet fill:#6600CC,stroke-width:0px,color:#fff
	
	A(수기 처리):::red
	
	Start((Start))
    ThirdParty((ThirdParty)):::violet
    Success((Success)):::violet
    Fail((Fail)):::violet
    UnionRegi(명부 등록):::green
    UnionRegiList(명부 목록):::green
    UnionRegiFailList(주소 검토 실패 목록):::red
    UnionRegiSuccessList(주소 검토 성공 목록):::green
    CheckAddress(주소 검토 클릭):::red
    
    Start --> |명부 엑셀 업르도| UnionRegi
    UnionRegi --> |주소 검토| ThirdParty
    UnionRegi --> |주소 검토 여부 포함하여 생성| UnionRegiList
    UnionRegiList --> |목록에서 실패 목록 찾기| UnionRegiFailList
    UnionRegiList --> UnionRegiSuccessList
    UnionRegiFailList --> |1건 씩| CheckAddress 
    ThirdParty --> |30%| Fail
    ThirdParty --> |70%| Success

    linkStyle 3,5 stroke-width:4px, stroke:red
```
### 시스템 프로세스
```mermaid
flowchart TD
%% Colors %%
  classDef green fill:#137433,stroke-width:0px,color:#fff
  classDef red fill:#FF3399,stroke-width:0px,color:#fff
  classDef violet fill:#6600CC,stroke-width:0px,color:#fff
  classDef orange fill:#FFB226,stroke-width:0px,color:#fff

  A(수기 처리):::red

  Start((Start))
  ThirdParty((ThirdParty)):::violet
  Success((Success)):::violet
  Fail((Fail)):::violet
  UnionRegi(명부 등록):::green
  UnionRegi(명부 등록):::green
  UnionRegiList(명부 목록):::green
  UnionRegiFailList(주소 검토 실패 목록):::red
  UnionRegiSuccessList(주소 검토 성공 목록):::green
  Consumer(Consumer):::green
  CheckAddress(주소 검토 클릭):::red
  RabbitMQ((RabbitMQ)):::orange

  Start --> |명부 엑셀 업르도| UnionRegi
  UnionRegi --> |주소 검토 x 생성| UnionRegiList
  UnionRegi --> |주소 검토| RabbitMQ
  UnionRegiList --> |목록에서 실패 목록 찾기| UnionRegiFailList
  UnionRegiList --> UnionRegiSuccessList
  UnionRegiFailList --> |1건 씩| CheckAddress
  ThirdParty --> |30%| Fail
  ThirdParty --> |70%| Success
  Consumer --> RabbitMQ
  Consumer --> |주소 검토| ThirdParty
  Consumer --> |주소 검토 상태 변경| UnionRegiList
  CheckAddress --> |주소 검토| ThirdParty
  CheckAddress --> |주소 검토 상태 변경| UnionRegiList

  linkStyle 3,5 stroke-width:4px, stroke:red
```