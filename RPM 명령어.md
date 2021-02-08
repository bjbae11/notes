### RPM 명령어

____

```
rpm -ivh 패키지명
> 설치

rpm -qa
> 설치된 패키지 전체목록 보기

rpm -qa | grep 패키지명
> 확인

rpm -ev 패키지명
> 제거

rpm -Uvh 패키지명
> 업그레이드

rpm -qf 파일
> 파일이 속한 패키지 찾기

rpm -qi 설치된 패키지명
rpm -qip 파일명.rpm
> rpm 패키지 정보 보기

rpm -ql 설치된 패키지명
rpm -qlp 파일명.rpm
> rpm 내부 파일목록 보기

rpm -qc 설치된 패키지명
rpm -qcp 파일명.rpm
> rpm 내부 설정파일 확인

rpm -qd 설치된 패키지명
rpm -qdp 파일명.rpm
> rpm 내부 문서파일 확인

rpm -q --scripts 설치된 패키지명
rpm -qp --scripts 파일명.rpm
> rpm 내부 스크립트 확인
```



### advanced packaging tools(APT) 명령어

____

|          기능           |      apt 명령어 ★      |       apt-get 명령어        |       aptitude 명령어        |         비고         |
| :---------------------: | :--------------------: | :-------------------------: | :--------------------------: | :------------------: |
|       패키지 설치       | `apt install 패키지명` | `apt-get install 패키지명`  | `aptitude install 패키지명`  |                      |
|       패키지 삭제       | `apt remove 패키지명`  |  `apt-get remove 패키지명`  |  `aptitude remove 패키지명`  |   패키지 파일 제거   |
|    패키지 정보 확인     |  `apt show 패키지명`   |              ·              |   `aptitude show 패키지명`   |                      |
|       패키지 검색       |  `apt search 검색어`   |              ·              |   `aptitude search 검색어`   |                      |
|  전체 패키지 목록 조회  |       `apt list`       |              ·              |              ·               |                      |
| 설치된 패키지 목록 조회 | `apt list --installed` |              ·              |              ·               |      `dpkg -l`       |
|      패키지 삭제2       |           ·            |  `apt-get purge 패키지명`   |  `aptitude purge 패키지명`   | 패키지+설정파일 제거 |
|     패키지 다운로드     |           ·            | `apt-get download 패키지명` | `aptitude download 패키지명` | 패키지 .deb파일 받기 |

