# Ubuntu의 crontab에서 GUI 프로그램 실행 스케쥴 등록하기(ubuntu 22.04 기준)

고객사 쪽의 요청으로 자사의 프로그램이 항상 켜져있도록 기능 개발을 해야했다.
24시간 항상 켜져있어야 하는 관제 프로그램이라서 비정상 종료시에도 켜질 수 있도록,
앱이 시스템에서 작동하는지 확인 후, 꺼져 있으면 자동으로 실행하는 스크립트를 python으로 구현하였다.


그리고 그 python이 살아있는지 주기적으로 확인하고 죽었으면 살리는 쉘 스크립트(.sh)를 작성하고
이를 crontab에 등록하여 주기적으로 쉘 스크립트를 실행시키도록 스케쥴링을 걸었다.

* 참고: electron 기반으로 구동되는 프로그램(javascript 기반)

## 스크립트의 작동 구조

```jsx
쉘스크립트.sh(python 스크립트가 살아있는지 확인 후 없으면 .py 실행) -> 자동실행스크립트.py(프로그램이 실행 중인지 확인 후 꺼져있으면 프로그램 자동 실행)

```

## crontab 초기 설정

```shell
* * * * * sh /home/사용자폴더/.../쉘스크립트.sh
* * * * * sleep 10; sh /home/사용자폴더/.../쉘스크립트.sh
* * * * * sleep 20; sh /home/사용자폴더/.../쉘스크립트.sh
* * * * * sleep 30; sh /home/사용자폴더/.../쉘스크립트.sh
* * * * * sleep 40; sh /home/사용자폴더/.../쉘스크립트.sh
* * * * * sleep 50; sh /home/사용자폴더/.../쉘스크립트.sh
```

그런데, 이렇게 설정했더니 원하는대로, 프로그램이 실행되지 않았다.

스크립트 자체는 동작하고 있음을 확인하였다.
```shell
ps -ef | grep 자동실행스크립트명.py  # 실행되고 있음
```
그런데, 일렉트론 앱을 살펴 보니 프로그램이 올라오다가 자꾸 죽는 것을 확인하였다.

```shell
ps -ef | grep 프로그램명  # 마운트되다가 계속 죽어버림

```
![crontab_troubleshooting1](https://user-images.githubusercontent.com/84123052/199933301-51f863ae-41fc-472c-865f-f71b96db5452.png)

해결하기 위해 열심히 구글링하고 스택오버플로우를 찾아보고 별짓을 다 하다가, crontab log를 저장하도록 설정 후 확인해 보았다


### 해결 과정

```shell
# 로그를 저장하도록 지정

* * * * * sh /home/사용자폴더/.../쉘스크립트.sh > 쉘스크립트.sh.log 2>&1
* * * * * sleep 10; sh /home/사용자폴더/.../쉘스크립트.sh > 쉘스크립트.sh.log 2>&1
* * * * * sleep 20; sh /home/사용자폴더/.../쉘스크립트.sh > 쉘스크립트.sh.log 2>&1
* * * * * sleep 30; sh /home/사용자폴더/.../쉘스크립트.sh > 쉘스크립트.sh.log 2>&1
* * * * * sleep 40; sh /home/사용자폴더/.../쉘스크립트.sh > 쉘스크립트.sh.log 2>&1
* * * * * sleep 50; sh /home/사용자폴더/.../쉘스크립트.sh > 쉘스크립트.sh.log 2>&1

```
![image](https://user-images.githubusercontent.com/84123052/199934158-3458038d-333d-40d1-b59a-1cafb62d3635.png)

로그를 확인해보니, `Missing X server or $DISPLAY` 와 `$XDG_RUNTIME_DIR` 관련해서 존재하지 않거나 정의되지 않았다는 메시지를 확인할 수 있었음
찾아보니, cron 으로 GUI 프로그램을 실행시켰을 때 GUI 이므로, 어느 디스플레이에 표시해야할지를 정해줘야 한다고 한다.

참고: 스택오버플로우
(https://askubuntu.com/questions/514167/how-to-start-a-gui-application-from-cron/514172#514172)

결국, 표시할 디스플레이를 지정해주지 않아서 발생한 문제인 것으로 확인됨!

### 해결법

환경변수 `DISPLAY=:0` 을 넣어주면 된다는 간단한 해결법...

```shell
# 해결 예시
* * * * * export DISPLAY=:0;sh /사용자의/지정된/경로/쉘스크립트.sh > 쉘스크립트.sh.log 2>&1
* * * * * sleep 10; export DISPLAY=:0;sh /사용자의/지정된/경로/쉘스크립트.sh > 쉘스크립트.sh.log 2>&1
* * * * * sleep 20; export DISPLAY=:0;sh /사용자의/지정된/경로/쉘스크립트.sh > 쉘스크립트.sh.log 2>&1
* * * * * sleep 30; export DISPLAY=:0;sh /사용자의/지정된/경로/쉘스크립트.sh > 쉘스크립트.sh.log 2>&1
* * * * * sleep 40; export DISPLAY=:0;sh /사용자의/지정된/경로/쉘스크립트.sh > 쉘스크립트.sh.log 2>&1
* * * * * sleep 50; export DISPLAY=:0;sh /사용자의/지정된/경로/쉘스크립트.sh > 쉘스크립트.sh.log 2>&1

# 참고: crontab은 최소 단위 지정이 분(min) 단위라서 초 단위로 적용하기 위해서 sleep 명령어를 이용했다
# 위는 10초 단위로 스케쥴링한 것!
```








