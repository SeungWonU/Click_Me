## Computer Boot Sequence(컴퓨터 부팅 과정)
### 1. 전원
&nbsp; 전원을 켜서 전기를 공급한다.

### 2. BIOS(Basic Input Output System)
&nbsp; 컴퓨터 메인보드에 메모리칩(ROM)안에 BIOS가 내재되어 있다. POST(Power On Self Test)작업을 한다 == 하드웨어가 정상적으로 동작하는지,오류가 없는지 검사를 한다.

### 3. Kenel Loading(커널 로딩)
&nbsp; 부트 로더(Boot Loader)를 실행한다 == 컴퓨터를 부팅하기 위한 운영체제를 메모리에 적재하는 프로그램. 커널 == 운영체제의 핵심 기능을 처리하는 프로그램.
 운영체제가 메모리에 올라오게 되면 다음 단계 실행

### 4.초기화
&nbsp; 각종 시스템, 설정등을 초기화 한다.

### 5.프로세스 시작
&nbsp; 프로그램이 시작된다.


// 노트북 켜지는 과정도 ! & start 1
 출처 : https://gomoveyongs.tistory.com/82