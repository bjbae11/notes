### Shell Script

____

- 쉘에서 사용할 수 있는 명령어들의 조합을 모아서 만든 batch 파일
- 리눅스에서는 pipe, redirection, filter 등으로 연결된 명령어 조합이 반복적으로 사용됨 => 쉘 스크립트로 단일 명령화





____

- 실행 권한 부여

```
$ chmod +x shell_script_practice.sh
```

- 스크립트 작성

```bash
# 첫 줄: #!<인터프리터 이름>, #!만 쓸 경우에는 /bin/sh로 인식
#!/bin/bash
```

- 기본 출력

  - echo

  ```bash
  echo "Echo Test"
  ```

  - printf

  ```bash
  printf "printf Test"
  ```

- $#: 스크립트에 전달되는 인자들의 수

- $0: 실행하는 스크립트 파일명

- $1, $2, $3...: 스크립트로 전달된 인자들

  ```bash
  printf "%d arguments %s %s\n" $# $1 $2
  ```

- 주석: # 사용

- 변수 선언

  - =를 이용해서 선언하고 $를 이용해서 사용

  - =는 공백없이 붙여써야함

  - 기본적으로는 전역변수이고 함수 내의 지역변수에는 local 붙임

  - {}는 $와 함께 감싼 부분에 변수를 대입해줌

  - 변수가 선언되지 않았을 때 사용할 기본값 설정 가능

    ```bash
    default_value=${default_value:="example default value"}
    ```

  - ""로 감싸서 사용하면 문자열에 공백도 포함해서 값을 이용할 수 있기 때문에 더 안전함

    ```bash
    $ex => "${ex}"
    ```

  - 예시

    ``` bash
    #!/bin/bash
    
    # shell script variable
    test="abc"
    num=100
    
    # variable usage
    echo ${test}
    echo ${num}
    echo "${test}"
    echo "${num}"
    
    # local variable
    local local_val="local one"
    
    # If variable default_value is not set, set it to "example default value" and assign again
    default_value=${default_value:="example default value"}
    ```

    

  - 변수 명 앞에 export를 붙여주면 환경 변수로 설정되어 자식 스크립트에서 사용 가능

  - 환경 변수는 ./bash_profile에서 정의

  - 환경 변수 사용시 예약 변수에 주의

  - 예시

    ```bash
    # 전역 변수 지정
    string="hello world"
    echo ${string}
    
    # 지역 변수 테스트 함수
    string_test() {
    	# 전역 변수와 동일하게 사용하며, local을 지우면 전역 변수에 덮어씌워짐
    	local string="local"
    	echo ${string}
    }
    
    # 지역 변수 테스트 함수 호출
    string_test
    
    # 지역 변수 테스트 함수에서 동일한 변수 명을 사용했지만 값이 변경되지 않음
    echo ${string}
    
    # 환경 변수 선언
    export hello_world="hello world..."
    
    # 자식 스크립트 호출은 스크립트 경로를 쓰면 됨
    /home/export_test.sh
    ```

  - 예약 변수(Reserved Variable) 목록

    ```bash
    HOME: 사용자의 홈 디렉토리
    PATH: 실행 파일을 찾을 경로
    LANG: 프로그램 사용시 기본 지원되는 언어
    PWD: 사용자의 현재 작업중인 디렉토리
    FUNCNAME: 현재 함수 이름
    SECONDS: 스크립트가 실행된 초 단위 시간
    SHLVL: 쉘 레벨(중첩된 깊이)
    SHELL: 로그인해서 사용하는 쉘
    PPID: 부모 프로세스의 PID
    BASH: BASH 실행 파일의 경로
    BASH_ENV: 스크립트 실행시 BASH 시작 파일을 읽을 위치 변수
    BASH_VERSION: 설치된 BASH 버전
    MAIL: 메일 보관 경로
    MAILCHECK: 메일 확인 시간
    OSTYPE: 운영체제 종류
    TERM: 로그인 터미널 타입
    HOSTNAME: 호스트 이름
    HOSTTYPE: 시스템 하드웨어 종류
    MACHTYPE: 머신 종류(HOSTTYPE보다 좀 더 상세히 표시)
    LOGNAME: 로그인 이름
    UID: 사용자 UID
    EUID: su 명령에서 사용하는 사용자의 유효 아이디 값
    USER: 사용자의 이름
    USERNAME: 사용자 이름
    GROUPS: 사용자 그룹
    HISTFILE: history 파일 경로
    HISTFILESIZE: history 파일 크기
    HISTSIZE: history 저장되는 개수
    HISTCONTROL: 중복되는 명령에 대한 기록 유무
    DISPLAY: X 디스플레이 이름
    IFS: 입력 필드 구분자
    VISUAL: VISUAL 편집기 이름
    EDITOR: 기본 편집기 이름
    COLUMNS: 현재 터미널이나 윈도우 터미널의 컬럼 수
    LINES: 터미널의 라인 수
    LS_COLORS: ls 명령의 색상 관련 옵션
    PS1: 기본 프롬프트 변수
    PS2: 보조 프롬프트 변수
    PS3: 쉘 스크립트에서 select 사용시 프롬프트 변수
    PS4 쉘 스크립트 디버깅 모드의 프롬프트 변수
    TMOUT: 0이면 제한이 없으며 time 시간 지정시 지정한 시간 이후 로그아웃
    ```

  - 위치 매개 변수(Positional Parameters)

    ```bash
    $0: 실행된 스크립트 이름
    $1: 인자 순서대로 번호가 부여됨, 10번째부터는 ${10}처럼 감싸줘야 함
    $*: 전체 인자 값
    $@: 전체 인자 값($*와 동일하지만 쌍따옴표로 변수를 감싸면 다른 결과 나옴)
    $#: 매개 변수의 총 개수
    ```

  - 특수 매개 변수(Special Parameters)

    ```bash
    $$: 현재 스크립트의 PID
    $?: 최근에 실행된 명령어, 함수, 스크립트 자식의 종료 상태
    $!: 최근에 실행한 백그라운드(비동기) 명령의 PID
    $-: 현재 옵션 플래그
    $_: 지난 명령의 마지막 인자로 설정된 특수 변수
    ```

  - 매개 변수 확장(Parameter Expansion)

    ```bash
    ${변수}: $변수 와 동일하지만 {} 사용해야만 동작하는 것들이 있음
    => echo ${string}
    ${변수:위치}: 위치 다음부터 문자열 추출
    => echo${string:4}
    ${변수:위치:길이}: 위치 다음부터 지정한 길이 만큼의 문자열 추출
    => echo ${string:4:3}
    ${변수:-단어}: 변수 미선언 혹은 NULL일 때 기본값 지정, 위치 매개 변수는 사용 불가
    => echo ${string-HELLO}
    ${변수-단어}: 변수 미선언시만 기본값 지정, 위치 매개 변수는 사용 불가
    => echo ${string-HELLO}
    ${변수:=단어}: 변수 미선언 혹은 NULL일 때 기본값 지정, 위치 매개 변수 사용 가능
    => echo ${string:=HELLO}
    ${변수=단어}: 변수 미선언시만 기본값 지정, 위치 매개 변수 사용 가능
    => echo ${string=HELLO}
    ${변수:?단어}: 변수 미선언 혹은 NULL일 때 단어 출력 후 스크립트 종료
    => echo ${string:?HELLO}
    ${변수?단어}: 변수 미선언시만 단어 출력 후 스크립트 종료
    => echo ${string?HELLO}
    ${변수:+단어}: 변수 선언시만 단어 사용
    => echo ${string:+HELLO}
    ${변수+단어}: 변수 선언 혹은 NULL일 때 단어 사용
    => echo ${string+HELLO}
    ${#변수}: 문자열 길이
    => echo ${#string}
    ${변수#단어}: 변수의 앞부분부터 짧게 일치한 단어 삭제
    => echo ${string#a*b}
    ${변수##단어}: 변수의 앞부분부터 길게 일치한 단어 삭제
    => echo ${string##a*b}
    ${변수%단어}: 변수의 뒷부분부터 짧게 일치한 단어 삭제
    => echo ${string%b*c}
    ${변수%%단어}: 변수의 뒷부분부터 길게 일치한 단어 삭제
    => echo ${string%%b*c}
    ${변수/찾는단어/변경단어}: 처음 일치한 단어를 변경
    => echo ${string/abc/HELLO}
    ${변수//찾는단어/변경단어}: 일치하는 모든 단어를 변경
    => echo ${string//abc/HELLO}
    ${변수/#찾는단어/변경단어}: 앞부분이 일치하면 변경
    => echo ${string/#abc/HELLO}
    ${!단어*}, ${!단어@}: 선언된 변수중에서 단어가 포함된 변수 명 추출
    => echo ${!string*}, ${!string@}
    ```

  - 배열

    ```bash
    # 배열 변수 사용은 반드시 괄호를 사용해야함
    # 1차원 배열만 지원
    # 배열의 크기 지정없이 배열 변수로 선언
    # 참고: 'declare -a' 명령으로 선언하지 않아도 배열 변수 사용 가능함
    declare -a array
    
    # 4개의 배열 값 지정
    array=("hello" "test" "array" "world")
    
    # 기존 배열에 1개의 배열 값 추가(순차적으로 입력할 필요 없음)
    array[4]="variable"
    
    # 기존 배열 전체에 1개의 배열 값을 추가하여 배열 저장(배열 복사 시 사용)
    array=(${array[@]} "string")
    
    # 위에서 지정한 배열 출력
    echo "hello world 출력: ${array[0]} ${array[3]}"
    echo "배열 전체 출력: ${array[@]}"
    echo "배열 전체 개수 출력: ${#array[@]}"
    
    printf "배열 출력 %s\n" ${array[@]}
    
    # 배열 특정 요소만 지우기
    unset array[4]
    echo "배열 전체 출력: ${array[@]}"
    
    # 배열 전체 지우기
    unset array
    echo "배열 전체 출력: ${array[@]}"
    ```

  - 변수 타입 지정

    ```bash
    # Bash 변수는 타입을 구분하지 않고 기본적으로 문자열이지만 문맥에 따라서 연산 처리 가능
    # 불완전한 형태의 declare, typeset 타입 지정 명령을 지원함
    
    # 읽기 전용
    # readonly string_variable="hello world" 문법과 동일
    declare -r string_variable
    
    # 정수
    # number_variable=10 문법과 동일
    declare -i number_variable=10
    
    # 배열
    # array_variable=() 문법과 동일
    declare -a array_variable
    
    # 환경 변수
    # export export_variable="hello world" 문법과 동일
    declare -x export_variable="hello world"
    
    # 현재 스크립트의 전체 함수 출력
    declare -f
    
    # 현재 스크립트에서 지정한 함수만 출력
    declare -f 함수이름
    ```

  - 논리 연산자

    ```bash
    &&, -a: 논리 AND
    ||, -o: 논리 OR
    ```

  - 산술 연산자

    ```bash
    +: 더하기
    -: 빼기
    *: 곱하기
    /: 나누기
    **: 거듭제곱
    %: 정수 나누기에서 나머지 값
    +=: 상수값만큼 증가
    -=: 상수값만큼 감소
    *=: 상수값을 곱함
    /=: 상수값으로 나눔
    %=: 상수값으로 나눈 나머지
    ```

  - 비트 연산자

    ```bash
    <<: 비트 왼쪽 쉬프트(쉬프트 한 번당 2를 곱하는 것과 동일)
    <<=: left-shift-equal
    >>: 비트 오른쪽 쉬프트(쉬프트 한 번당 2로 나눔)
    >>=: right-shift-equal
    &: 비트 and
    &=: 비트 and-equal
    |: 비트 or
    |=: 비트 or-equal
    ~: 비트 negate
    !: 비트 not
    ^: 비트 xor
    ^=: 비트 xor-equal
    ```

  - 기타 연산자

    ```bash
    ,: 콤마 연산자, 2개 이상의 산술 연산을 묶어줌
    ```

  - 정수 비교

    ```bash
    -eq: 같음
    -ne: 같지 않음
    >, -gt: 더 큼(> 이중 소괄호에서 사용 가능)
    >, -ge: 더 크거나 같음(>= 이중 소괄호에서 사용 가능)
    <, -lt: 더 작음(< 이중 소괄호에서 사용 가능)
    <=, -le: 더 작거나 같음(<= 이중 소괄호에서 사용 가능)
    ```

  - 문자열 비교

    ```bash
    =, ==: 같음
    !=: 같지 않음
    <: ASCII 알파벳 순서에서 더 작음
    >: ASCII 알파벳 순서에서 더 큼
    -z: 문자열이 NULL, 길이가 0
    -n: 문자열이 NULL이 아님
    ${변수}: 문자열이 NULL이 아님
    ```

  - 파일 비교

    ```bash
    -e: 파일이 존재
    -f: 파일이 존재하고 일반 파일인 경우(디렉토리 혹은 장치파일이 아닌 경우)
    -s: 파일이 존재하고 0보다 큰 경우
    -d: 파일이 존재하고 디렉토리인 경우
    -b: 파일이 존재하고 블록장치 파일인 경우
    -c: 파일이 존재하고 캐릭터 장치 파일인 경우
    -p: 파일이 존재하고 FIFO인 경우
    -h: 파일이 존재하고 한 개 이상의 심볼릭 링크가 설정된 경우
    -L: 파일이 존재하고 한 개 이상의 심볼릭 링크가 설정된 경우
    -S: 파일이 소켓 디바이스인 경우
    -t: 파일이 디스크립터가 터미미널 디바이스와 디바이스와 연관이 있음
    -r: 파일이 존재하고 읽기 가능한 경우
    -w: 파일이 존재하고 쓰기가 가능한 경우
    -x: 파일이 존재하고 실행 가능한 경우
    -g: 파일이 존재하고 SetGID가 설정된 경우
    -u: 파일이 존재하고 SetUID가 설정된 경우
    -k: 파일이 존재하고 Sticky bit가 설정된 경우
    -O: 자신이 소유자임
    -G: 그룹 아이디가 자신과 같음
    -N: 마지막으로 읽힌 후에 변경 됐음
    file1 -nt file2: file1이 file2보다 최신임
    file2 -ot file2: file1이 file2보다 오래됨
    file1 -ef file2: file1과 file2가 같은 파일을 하드 링크하고 있음
    !: 조건이 안 맞으면 참(ex: ! -e file)
    ```

  - 반복문
  
    ```bash
    # 반복문을 빠져 나갈때: break
    # 현재 반복문이나 조건문을 건너뛸 때: continue
    
    # 지정된 범위 안에서 반복문 필요 시 좋음
    for string in "hello" "world" "..."; do;
        echo ${string};
    done
    
    # 수행 조건이 true 일때 실행됨 (실행 횟수 지정이 필요하지 않은 반복문 필요 시 좋음)
    count=0
    while [ ${count} -le 5 ]; do
        echo ${count}
        count=$(( ${count}+1 ))
    done
    
    # 수행 조건이 false 일때 실행됨 (실행 횟수 지정이 필요하지 않은 반복문 필요 시 좋음)
    count2=10
    until [ ${count2} -le 5 ]; do
        echo ${count2}
        count2=$(( ${count2}-1 ))
    done
    ```
  
  - 조건문
  
    ```bash
    string1="hello"
    string2="world"
    if [ ${string1} == ${string2} ]; then
        # 실행 문장이 없으면 오류 발생함
        # 아래 echo 문장을 주석처리하면 확인 가능함
        echo "hello world"
    elif [ ${string1} == ${string3} ]; then
        echo "hello world 2"
    else
        echo "hello world 3"
    fi
    
    # AND
    if [ ${string1} == ${string2} ] && [ ${string3} == ${string4} ]
    ..생략
    
    # OR
    if [ ${string1} == ${string2} ] || [ ${string3} == ${string4} ]
    ..생략
    
    # 다중 조건
    if [[ ${string1} == ${string2} || ${string3} == ${string4} ]] && [ ${string5} == ${string6} ]
    ..생략
    ```
  
  - 선택문
  
    ```bash
    # 정규식을 지원하며 | 기호로 다중 값을 입력 가능하며 조건의 문장 끝에는 ;; 기호로 끝을 표시
    
    # case문 테스트를 위한 반복문
    for string in "HELLO" "WORLD" "hello" "world" "s" "start" "end" "etc"; do
    
        # case문 시작
        case ${string} in
            hello|HELLO)
                echo "${string}: hello 일때"
                ;;
            wo*)
                echo "${string}: wo로 시작하는 단어 일때"
                ;;
            s|start)
                echo "${string}: s 혹은 start 일때"
                ;;
            e|end)
                echo "${string}: e 혹은 end 일때"
                ;;
            *)
                echo "${string}: 기타"
                ;;
        esac
        # //case문 끝
    
    done
    ```



