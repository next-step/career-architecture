# career-architecture
> mermaid로 작성된 과제는 마크다운 파일(ARCHITECTURE.md)로 올려주시면 됩니다. (md 파일 내에 기존 구조를 넣어주세요)<br>
> 별도 아키택쳐나 모델링 도구를 사용한 경우에는 마크다운 파일(ARCHITECTURE.md)과 png, gif, jpg, pdf 파일 형식으로 architecture-{gitID}.png 파일명으로 upload 해주세요

# 요구사항

- [x] 담당 하는 업무에서 비효율적인 프로세스나 기술적 개선을 하고 싶은 부분의 현재 구조를 문서화 한다.
- [x] 비효율적인 부분에 대한 분석내용을 정리한다.
- [x] 비효율적인 부분에 대한 프로세스 또는 시스템 구조를 그려본다.


## 🚀미션
- 이름 : 박다정
### 개선포인트 분석
- 새로 생성하는 프로젝트들이 늘어나고 있다.
- 팀에서 [linter](https://ko.wikipedia.org/wiki/%EB%A6%B0%ED%8A%B8_(%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4))를 사용하고 있는데 프로젝트들이 생성될 때마다 같은 파일을 복사 + 붙여넣고 있다
- 확장이 필요하다. 프론트, 백엔드 고유의 linter 설정이 존재하고 어떤 경우는 project 개별적으로 적용할 수 있다
 
### 프로세스
```mermaid
flowchart TB
    프로젝트 생성 -> 기존 프로젝트에 있는 설정 복사 붙여넣기 -> linter 사용
```
