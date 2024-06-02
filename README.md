# top, ps, jobs, kill commands


## top 명령어


리눅스 시스템의 CPU 사용량, 메모리 사용량, 현재 시스템의 상태, 실행 중인 프로세스 목록 등 전반적인 상황을 실시간으로 모니터링할 수 있는 명령어이다. 윈도우의 작업관리자랑 비슷한 역할을 하는 도구이며, 리눅스를 사용하는 서버의 성능이나 현재 돌아가고 있는 상황을 볼 때 사용한다. 


사용법: 터미널에서 top 입력 후 Enter


**명령어 사용시**

요약 영역 디테일 영역이 출력된다.

요약영역 : 다양한 정보를 요약하여 가장 상단에 출력

- 시스템 시간, Uptime, 유저 세션 수：시스템 현재 시간, OS가 구동된 시간, 현재 접속중인유저 세션 수

- Load Average: 1분, 5분, 15분 동안의 평균 시스템 부하를 나타냄. 리눅스에서 부하란 해당 시간에 Running 혹은 D 상태(uninterruptable sleep, I/O작업대기)인 프로세스의 개수를 뜻한다

- Tasks: 현재 프로세스들의 상태를 출력

- CPU 사용량

- 메모리 사용량


top 명령어 화면에서 사용할 수 있는 특정 키 커맨드:

    q: top 명령어 종료
    h: 도움말
    k: 특정 프로세스를 종료 (프로세스 ID 입력 필요)
    1: 각 CPU 코어별 사용률을 표시
    P: CPU 사용량으로 정렬
    M: 메모리 사용량으로 정렬
    N: PID 순으로 정렬
    T: Running Time으로 정렬
    R: 오름차순과 내림차순 토글 변경
    space bar: 정보 바로 업데이트

## 2. ps 명령어 (Process State)

현재 실행중인 프로세스 목록과 상태를 출력하여 보여주는 기능을 한다. 다양한 옵션을 통해 특정 프로세스 정보를 필터링하여 확인할 수 있다.


명령어 사용법:

    ps: 현재 쉘에서 실행 중인 프로세스 목록 출력
    ps ax: 시스템의 모든 프로세스를 BSD 포맷으로 출력
    ps aux: 시스템의 동작중인 모든 프로세스를 소유자 정보와 함께 다양한 정보를 출력
    ps aux | grep word: word를 찾아 출력해주는 명령을 예시로 듦
특정 프로세스에 대해서 보고싶을 경우 ‘grep’을 하께 활용한다
    ps auxf: 실행중인 프로세스를 트리구조로 출력
    ps auxfww: 실행중인 프로세스를 트리구조 + 모든 실행 중인 옵션 확인 가능
    ps –ef: 시스템에 동작중인 모든 프로세스를 자세하게 출력
    ps –el: 긴 포맷으로 출력하여 보고싶을 경우 –l 명령어를 사용한다. ef 명령에서 보이지 않았던 많은 정보들이 출력된다.


주요 옵션

    -A:모든 프로세스를 출력
    -a: 세션 리더를 제외하고 데몬 프로세스처럼 터미널에 종속되지 않은 모든 프로세스를 출력
    -u: 프로세스의 소유자를 기준으로 출력
    -x: 로그인 상태에 있는 동안 아직 완료되지 않은 프로세서들을 출력

## 3. jobs 명령어

현제 쉘 프로세스에서 백그라운드로 실행되거나 중지되어 있는 프로세스 목록을 조회하는 명령어


**사용법**: 터미널에서 jobs [option] [JOB] 입력 후 Enter
jobs 명령어는 옵션이나 인자 없이 주로 사용한다. 단, 실행중인 작업이 없다면 아무것도 출력되지 않는다


옵션과 의미

    -p 백그라운드에 있는 프로세스의 프로세스 아이디(PID)만 출력한다
    -l 백그라운드에 있는 프로세스의 프로세스 아이디(PID)를 함께 출력한다
    -s 백그라운드에 있는 프로세스 중 멈춰있는 프로세스만 출력한다
    -r 백그라운드에 있는 프로세스 중 실행중인 프로세스만 출력한다

출력 예시:

    [1]+ Stopped nano test.txt
    [2]- Running python script.py &

첫 번째 열의 대괄호 안의 숫자는 JOB ID가 된다. 이 번호는 현재 셀에서 유효한다. 그 다음에 있는 +는 bg나 fg 명령 실행시 기본 인자로 사용된다는 의미이다. -는 +로 표시된 프로세스가 종료시 기본 값으로 사용될 프로세스를 의미한다

    sleep 100 &: 프로세스를 실행한 명령어를 나타낸다

jobs 명령어에 인자를 지정할 경우 특정 프로세스만 확인할 수 있다. 인자는 %<JOB_ID>형식으로 지정한다.

입력 출력 예시:

    $ sleep 100 &
    [2] 99242
    $ jobs %2
    [2]+ Running        sleep 100 &

jobs 명령에 대한 내용에서 포그래운드 정보는 맨 앞의 숫자로 실행된 순서를 알 수 있고, ‘&’가 있음과 없음에 따라 멈춰(Stopped)있는 상태와 실행 중(Running)인 상태인 것을 알 수 있다.

**출력되는 백그라운드 작업의 상태값 설명**

|상태값|의미|
|-----|-----|
|Running| 작업이 계속 진행중임|
|||
|Done| 작업이 완료되어 0을 반환|
|||
|Done(code)| 작업이 종료되었으며 0이 아닌 코드를 반환|
|||
|Stopped| 작업이 일시 중단|
|||
|stopped(SIGTSTP)| SIGTSTP 시그널이 작업을 일시 중단|
|||
|stopped(SIGSTOP)| SIGSTOP 시그널이 작업을 일시 중단|
|||
|stopped(SIGTTIN)| SIGTTIN 시그널이 작업을 일시 중단|
|||
|stopped(SIGTTOU)| SIGTTOU 시그널이 작업을 일시 중단|


## 4. kill 명령어
kill 명령어는 특정 프로세스를 종료하는 데 사용된다. 프로세스 ID(PID)를 사용하여 프로세스를 식별한다.

형식:

    kill [OPTIONS] [PID] // 지정된 PID의 프로세스를 종료

예시:

    kill -9 [PID]: 강제로 프로세스를 종료 (SIGKILL 신호)
    kill 1234: PID가 1234인 프로세스를 종료
    kill -9 1234: PID가 1234인 프로세스를 강제로 종료
    kill –1 :프로세스를 다시 로드
    kill –15: 프로세스를 정상적으로 종료를 요청
    kill –l: 사용 가능한 모든 신호 목록 표시
    kill –s: 지정한 시그널을 보냄

**주요 시그널**

SIGHUP 세션 종료 시그널

SIGINT 인터럽트 시그널

SIGKILL 강제 종료 시그널

SIGTERM 정상 종료 요청 시그널

SIGSTOP 프로세스 일시 정지 시그널


**각 신호를 사용하는 적절한 상황**

SIGTERM 신호를 먼저 사용하는 것이 좋음

프로세스에게 종료 준비를 할 수 있는 시간을 제공함

만약 프로세스가 반응하지 않는다면 SIGKILL 신호를 사용하여 강제로 종료함


kill –9 명령어는 SIGKILL 신호를 보내 프로세스를 즉시 종료시킨다

프로세스가 정상적으로 종료할 기회를 제공하지 않기 때문에 파일 손상이나 데이터 손실을 일으킬 수 있다. 따라서 다른 방법이 모두 실패했을 때만 사용해야 한다.

**추가 정보**
프로세스 ID (PID) 찾기: ps 명령어를 사용하여 특정 프로세스의 PID를 찾는다.

    ps aux | grep process_name
    
process_name과 관련된 모든 프로세스를 검색할 수 있다
