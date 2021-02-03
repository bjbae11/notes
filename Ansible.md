### Ansible

- 서버의 provisioning, SW deployment 자동화 툴
- Agentless: 비슷한 다른 자동화 도구와 달리 agent 기반 pull 방식이 아니라 타겟 대상에 agentless push 방식으로 동작(관리 서버에만 Ansible을 설치하면 됨)
- 한 번 설치한 것은 중복해서 다시 설치하지 않음



- 구성 요소

  - inventory
    - 대상, 즉 host ip들을 정리한 파일
    - 별명, 그룹 지정, SSH접근 방식 등을 기록
  - playbook
    - inventory에 작성된 서버들을 대상으로 수행할 특정 행위에 대해 정의한 파일(.yaml)

  => 공통적으로 사용되는 변수, 행위 등을 task 단위로 따로 분리, 저장해 다양한 배포 버전을 만들어 낼 수 있음

- 

