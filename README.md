---

# 1. 리눅스 명령어
__리눅스 명령어 중 다음의 명령어를 조사하여 작성할 예정임.__
1. top
2. ps
3. jobs
4. kill

---

## 1.1 top
top은 현재 운영체제의 전반적인 상태를 나타냅니다. 실행결과는 다음과 같습니다.

![top](https://user-images.githubusercontent.com/104444048/171154190-1e02158f-ede1-49f5-885e-9a57b65c2913.JPG)

명령어 옵션은 다음과 같습니다(주요 옵션)
|옵션|인자|사용례|설명|
|:-----:|:-----:|:----:|---------|
|-b||`top -b`|배치 모드|
|-c||`top -c`|명령어 라인 표시|
|-d|업데이트 간격|`top -d 5`|업데이트 간격 설정|
|-H||`top -H`|모든 개별 상태 스레드 표시|
|-i||`top -i`|유휴(IDLE) 상태 프로세스 무시|
|-n|반복횟수|`top -n 5`|반복의 최대 횟수 지정|
|-p(PID)||`top -p2031`|지정한 프로세스 번호(PID) 만 보여주기|

-p 옵션은 유용합니다. -p 옵션의 사용례를 다음 사진으로 살펴봅시다.

![image](https://user-images.githubusercontent.com/104444048/171904414-2d80d03c-a9b5-47cc-9052-f54b61f05f64.png)

우선 위와 같이 ps 명령어를 통해 __PID__ 를 확인합니다. ps 명령어는 목차 2에서 다룰 것입니다. 저 사진의 프로세스 중 PID가 ___488___ 인 프로세스의 정보만 보도록 합시다. 이를 위해

`top -p488`

이라고 치고 실행해봅시다. 결과는 다음과 같습니다.

![image](https://user-images.githubusercontent.com/104444048/171905009-51bb10b9-0942-4ca3-9bc3-a675701a9330.png)

정상적으로 실행되었습니다. 

---

top 명령어가 실행되는 중에도 특정 키를 눌러 명령을 실행할 수 있습니다(vi 에디터의 ZZ와 같은)

실행 중의 명령어는 다음과 같습니다
|키|설명|
|:-:|---|
|스페이스바|화면 새로고침|
|k|kill 명령|
|r|우선순위 변경(-20~20)|
|Z|화면 색상 변경|
|B|글자 굵게|
|q|종료|

---

### 1.1.1 첫 번째 줄
top 명령어를 실행했을 때 가장 처음 보이는 숫자는 __시스템의 현재 시간__ 입니다. 캡처했을 당시의 시스템 시간인 ___오후 7시 35분 13초___ 가 24시간제로 표시됩니다.

다음으로 표시되는 시간은 __운영체제 부팅 후 지금까지의 시간__ 입니다. 위 사진에서는 부팅 후 약 17분이 지났음을 표시합니다.

그 다음에 이어지는 내용은 접속한 사용자 세션 수와 로드 애버리지입니다. 이 중에서 로드 애버리지는 __CPU 로드의 이동 평균__ 을 나타냅니다. 총 3개의 소수점 값이 보이는데, 앞에서부터 1분, 5분, 15분에 대한 평균 CPU 로드를 나타냅니다. 윈도우 작업관리자의 '성능'탭에도 이와 비슷한 기능이 있습니다.  CPU 코어가 1개라면 1.0일 경우 CPU를 100% 활용하고 있다는 의미입니다. CPU 코어가 n개인 경우에는 각 수치를 n으로 나눈 값이 한 코어당 평균사용량이 됩니다.

각각 __0.04, 0.02, 0.04__ 이므로 1분 평균 4%, 5분 평균 2%, 15분 평균 4%임을 알 수 있습니다.

---

### 1.1.2 두 번째 줄
두 번째 줄은 Tasks, 즉 현재 프로세스의 상태를 보입니다. 프로세스의 총 개수와 각 프로세스를 4개의 상태로 구분하여 나타냅니다. 다음 표를 참고하십시오.

|프로세스 상태|설명|
|:-----:|-------------------------------------------------------------------------------|
|running|현재 작업 중인 프로세스|
|sleeping|대기 상태인 프로세스|
|stopped|종료된 상태의 프로세스|
|zombie|좀비 상태의 프로세스|

총 2개의 프로세스 중 1개 프로세스가 작업 중이고, 1개 프로세스가 대기 상태임을 확인할 수 있습니다.

---

### 1.1.3 세 번째 줄
세 번째 줄은 CPU의 사용률을 각 영역별로 보여줍니다. 각 영역의 총합은 100%입니다.

|영역|설명|
|:----:|----|
|us|프로세스의 유저 영역 CPU 사용률|
|sy|프로세스의 시스템 커널 영역에서의 CPU 사용률|
|ni|프로세스의 우선순위 설정에 사용되는 CPU 사용률|
|id|전체 사용률 중 유휴 상태 비율(IDLE 상태)|
|wa|입출력이 완료될 때까지 대기하기 위해 대기하는 CPU 사용률|
|hi|하드웨어 인터럽트에 사용되는 CPU 사용률|
|si|소프트웨어 인터럽트에 사용되는 CPU 사용률|
|st|CPU를 VM에서 사용하여 대기하는 CPU 사용률|

---

### 1.1.4 네 번째 줄
네 번째 줄은 램의 메모리 영역으로 ___MiB___ 단위로 표기됩니다.(1MiB = 1024킬로바이트)

|영역|설명|
|:----:|----|
|total|메모리의 총량|
|free|가용 메모리 양|
|used|현재 사용중인 메모리 양|
|buff/cache|각각 커널 버퍼에서 사용되는 메모리와 디스크의 페이지 캐시(입출력 시 사용)|

---

### 1.1.5 다섯 번째 줄
다섯 번째 줄은 스왑(Swap) 메모리 영역으로 이 역시 MiB단위로 표기됩니다.

이 영역은 __디스크 영역__ 이고 메모리의 가용 공간이 부족할 때 사용합니다. 네 번째 줄과 비슷하나, buff/cache가 없고 avail Mem이 있습니다. __avail Mem__ 은 스왑 메모리 없이 사용 가능한 메모리 양입니다.

---

### 1.1.6 나머지 줄
나머지 줄이 나타내는 영역은 __디테일 영역__ 입니다. 각 프로세스에 대한 상새 정보가 표시됩니다.

다음 표는 해당 영역의 각 요소별 설명입니다.

|영역|설명|
|:----:|----|
|PID|프로세스 ID|
|USER|실행 또는 영향받는 사용자 이름|
|PR|우선순위(Priority)|
|NI|PR에 영향을 미치는 값(nice)|
|VIRT|프로세스가 사용중인 메모리|
|RES|램이 사용중인 메모리의 크기|
|SHR|공유메모리|
|S|프로세스의 현재 상태(status)_(1.1.2 참고)_|
|%CPU|CPU 점유율|
|%MEM|메모리 점유율|
|TIME+|프로세스가 CPU를 사용한 시간|
|COMMAND|프로세스를 실행시킨 명령어|

---

## 1.2 ps
ps 명령어는 __프로세스 상태 (Process Status)__ 의 약자로, 말 그대로 현재 프로세스들의 상태를 보여줍니다. 

실행결과는 다음과 같습니다.
![image](https://user-images.githubusercontent.com/104444048/171908223-915e7a21-63d4-47ce-8cc2-1d2603f86b56.png)

---

다음의 옵션들이 있습니다.

|옵션|인자|사용례|설명|
|:-----:|:-----:|:----:|---------|
|-a||`ps -a`|세션 리더와 터미널과 무관한 프로세스를 제외한 모든 프로세스 표시|
|-A||`ps -A`|모든 프로세스 표시|
|-d||`ps -d`|세션 리더를 제외한 모든 프로세스 표시|
|-T||`ps -T`|현 터미널의 모든 프로세스 표시|
|-i||`ps -i`|유휴(IDLE) 상태 프로세스 무시|
|-r||`ps -r`|현재 실행중인 프로세스 표시|
|-p|"PID 번호"|`ps -p2031`|지정한 프로세스 번호(PID) 만 보여주기|

-p 옵션의 사용례를 보겠습니다. 다음과 같이 입력합니다.

`ps -A`

이 명령어와 옵션은 모든 프로세스를 보여줍니다. 실행결과는 다음과 같습니다.
![image](https://user-images.githubusercontent.com/104444048/171908669-00a65df9-0a4f-46a8-aaf6-772e292b8b51.png)

도커 관련 프로세스 두 개를 대상으로 해 봅시다. 각각 ___211___ 과 ___221___ 의 PID를 가집니다. 다음과 같이 두 개의 PID를 한꺼번에 실행합시다.

`ps -p "211 221"`

결과는 다음과 같습니다.
![image](https://user-images.githubusercontent.com/104444048/171909109-9ee5c485-5c2e-4eeb-9b43-fc9adbc13e20.png)

두 프로세스 정보가 정상적으로 확인됩니다.

---

ps 명령어를 실행했을 시 각 영역이 있습니다. 각 영역별 설명은 다음 표에서 확인할 수 있습니다.

|영역|설명|
|:----:|----|
|PID|프로세스 ID|
|TTY|프로세스가 연결된 터미널|
|TIME|CPU 사용 시간|
|CMD|프로세스를 실행시킨 명령어|

---

## 1.3 jobs
명령어 jobs는 __작업의 상태__ 를 표시합니다. 현재 쉘 세션에서 실행시킨 백그라운드 작업 목록을 볼 수 있습니다. 각 작업의 앞에는 번호가 있으며, kill과 같은 명령어의 인자로 전달할 수 있습니다. 실행결과는 다음과 같습니다.
![image](https://user-images.githubusercontent.com/104444048/171916823-39ea85ec-0db9-4a04-a693-2c994d9d47bf.png)

명령어 실행의 확인을 위해 vim 에디터를 ___Ctrl+Z___ 키로 강제로 정지시켜 jobs를 통해 정지(Stopped)된 상태의 백그라운드 작업을 확인할 수 있도록 해 놓았습니다.

---

다음 표는 jobs의 옵션입니다.

|옵션|사용례|설명|
|:----:|:----:|--|
|-l|`jobs -l`|프로세스 ID를 포함한 출력|
|-n|`jobs -n`|상태가 변경된 프로세스 출력|
|-p|`jobs -p`|프로세스 ID만 출력|
|-r|`jobs -r`|실행 중인 작업만 출력|
|-s|`jobs -s`|정지된 작업만 출력|

---

## 1.4 kill
jobs에 이어 __작업을 죽이는__ kill 명령어를 실행해 보겠습니다. vim이 정지된 상태라고 가정하고, 다음과 같이 입력합니다.

`kill -9 %1`

이렇게 하면 숫자 1이 붙은 작업이 죽습니다. jobs를 입력해 확인합니다.
![image](https://user-images.githubusercontent.com/104444048/171919805-8442fee0-62d4-4432-9379-b8ee2f56f97c.png)

성공적으로 죽였음을 확인할 수 있습니다 _(Killed)_

---

kill 명령어의 옵션은 많지만, 실사용에서 자주 쓰는 옵션 두 가지만 알아보는 것으로 하겠습니다.

|옵션|인자|사용례|설명|
|:-----:|:-----:|:----:|---------|
|-9|작업번호 또는 PID|`kill -9 %1` 또는 `kill -9 201`|작업 강제 종료|
|-15|_위와 동일_|`kill -15 %1` 또는 `kill -15 201`|작업 정상 종료|

---

# 2. vim 에디터의 매크로 사용법
vim 에디터로 편집을 하다 보면 특정한 작업을 반복해서 수행할 필요가 있을 수 있습니다. 이럴 때 특정 키에 __매크로__ 를 지정해 둔다면 수월한 작업이 가능합니다. 매크로를 수행하기 위해 필요한 키는 __q__ 니다. vim의 기본 모드에서 q를 누르고 a를 눌러보겠습니다.

![image](https://user-images.githubusercontent.com/104444048/171925595-76183649-5899-4af3-87f9-b8da10da1a5f.png)

보다시피 키 a에 대한 매크로 기록이 시작되었습니다. 현재 vim 에디터에는 다음과 같은 python 코드가 작성되어 있습니다.
```python
print("Hello World")
print("Hello World")
```

여기서 "Hello World" 사이에 있는 공백을 없애고, 뒤에 느낌표를 추가한 뒤 주석 처리하는 매크로를 작성해 보겠습니다. 다음의 키를 순서대로 입력합니다.

* ^ : 앞으로 이동
* i : 삽입 모드
* '#' : 주석 처리를 위한 '#' 삽입
* Esc : 일반 모드 전환
* f키 누른 뒤 스페이스 : 공백으로 이동
* x : 공백 삭제
* f키 누르고 " : 두 번째 쌍따옴표 직전으로 이동
* i : 삽입 모드
* ! : 느낌표 추가
* Esc : 일반 모드 전환

위와 같은 과정을 전부 마쳤으면, q를 눌러 매크로 기록을 종료합니다. 그리고 두 번째 줄로 이동해서 @a를 입력합니다. 그러면 다음과 같이 됩니다.

```python
#print("HelloWorld!")
#print("HelloWorld!")
```

![image](https://user-images.githubusercontent.com/104444048/171932610-f3ec724d-b893-4dbb-8ec9-a56611abd585.png)

위와 같이 정상 실행됨을 확인할 수 있었습니다.
