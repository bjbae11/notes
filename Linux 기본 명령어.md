## Linux 기본 명령어

```
pwd
// print working directory
// 현재 작업중인 디렉토리 정보 출력

cd
// change directory
// 경로 이동(절대경로, 상대경로 이동 가능)

ls
// list
// 디렉토리 목록 확인(-R 붙이면 하위 디렉토리 목록까지 보여줌)

cp
// copy
// 파일 또는 디렉토리 복사(디렉토리 복사 시 -r 붙임)
// ex) $ cp testfile1 testfile2
// ex) $ cp -r testdir testdir_cp

mv
// move
// 파일 또는 디렉토리 이동
// ex) $ mv testfile testdir/
// ex) $ mv testfile testfile_a

mkdir
// make directory
// 디렉토리 생성(-p 붙이면 하위 디렉토리까지 한 번에 생성 가능)

rm
// remove
// 파일 또는 디렉토리 삭제(디렉토리 삭제 시 -r 붙임)

touch
// 파일이나 디렉토리의 최근 업데이트 일자를 현재 시간으로 변경
// 최근 업데이트 일자는 ls -l 로 확인 가능
// 파일이나 디렉토리가 존재하지 않으면 빈 파일을 만든다

sed
// 특정 문자열을 다른 문자열로 바꿈
// $ sed -i 's/hi/hello/g' testfile.txt
// textfile.txt 라는 파일의 모든 'hi'라는 문자열을 'hello'로 바꿈

cat
// concatenate
// 1. 파일 내용 출력 (ex: cat file1)
// 2. 파일 여러 개 병합 (ex: cat file1 file2 > file1_2)
// 3. 한 파일의 내용을 다른 파일에 덧붙임 (ex: cat file1 >> file2)
// 4. 새로운 파일 생성
// ex: $ cat > file4
// 		hello
//		world
//		(작성이 끝나면 ctrl+d로 저장)

head
// 파일의 앞 부분을 보고싶은 줄 수만큼 보여줌
// 옵션 지정하지 않으면 파일 상위 10줄을 보여줌
// (ex: head -3 testfile)

tail
// 파일의 뒷 부분을 보고싶은 줄 수만큼 보여줌
// 옵션 지정하지 않으면 파일 하위 10줄을 보여줌
// (ex: tail -3 testfile)

find
// 특정 파일이나 디렉토리 검색
// find [검색경로] -name [파일명]
// 파일명은 풀네임, 조건검색 가능
// (ex: find ./ -name 'file1')
// (ex: find ./ -name '*.jpg')
//
// 1. 확장자가 .jpg인 파일만 찾아서 바로 삭제하기
// $ find ./ -name '*.jpg' -exec rm {} \;
// 2. 디렉토리나 파일만 지정해서 검색하기
// $ find ./ -type d
// $ find ./ -type f
// 3. 특정 디렉토리에 조건에 맞는 결과 값이 몇 개 존재하는지 숫자로 반환
// $ find ./ -type f | wc -l
// 4. 특정 조건에 해당하는 파일들의 내용을 전부 찾아서 바꾸기
// $ find ./ -name '*.txt' -exec sed -i 's/hi/hello/g' {} \;
// (txt 파일 안에 있는 'hi'라는 문자열을 모두 'hello'로 바꿈)

mount
// SD카드 및 USB 연결 시 파일 시스템으로 마운트

uname
// 시스템 정보 출력

ps
// 현재 시스템에서 실행중인 프로세스 시각화

kill
// 프로세스 강제 종료

service
// 시스템 전체 서비스 호출

batch
// 미리 정의된 일정에 따라 시스템 서비스 실행

shutdown
// 시스템 종료

locate
// 특정 파일의 위치 검색

grep
// 텍스트 파일에서 패턴 검색

clear
// 터미널 화면 초기화

sudo
// 관리자 권한

man
// 특정 터미널 명령 사용 방법 출력

ctrl + z
// 백그라운드로 작업창 이동

fg
// 백그라운드 작업창 불러오기

ctrl + p + q
// 컨테이너 종료하지 않고 도커 쉘 빠져나오기

tar -xvf [.tar file name]
tar 압축해제

tar -cvf [.tar file name] [zipping folder name]
tar 파일로 압축


```

