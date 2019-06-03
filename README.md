# **Docker Guide**

1. 이미지 다운로드(pull)

   ```
   $ docker pull [OPTIONS] NAME[:TAG|@DIGEST]
   ```

   ex) docker pull -name ip_test pytorch/pytorch 

2. 생성된 이미지 확인 

   ```
   $ docker image ls
   $ docker images
   ```

3. 이미지를 컨테이너에 올리기

   ```
   $ nvidia-docker run -ti -p (외부포트번호):22 -v (마운트 할 폴더 경로):(컨테이너 내 마운트 된 폴더 저장 경로) --privileged --name (컨테이너 명) (이미지 명):(태그)
   ```

   - **-ti** : bash로 시작, 만약 되지 않는다면, 맨 끝에 /bin/bash 추가

   - **--p A:B** : host의 A포트를 컨테이어의 B포트에 연결

   - **-v A:B** : host의 저장소 A 위치를 컨테이어의 저장소 B위치에 연결

   - **--name A** : 해당 컨테이너의 별명을 A로 지음

   - **--privileged** : 컨테이너 안에서 호스트의 리눅스 커널 기능을 모두 사용할 수 있도록 함

   - **--expose=[]**: 컨테이너의 포트를 호스트와 연결만 하고 외부에는 노출하지 않습니다. 예) --expose=”3306”

   - ex) docker run -ti -p 8022:22 -p 8000:8000 -v /home/mmlab:/workspace --name ubuntu_example --network mmlabnet ubuntu:16.04

     ex) docker run - ti - p 8000:22 --name kmj-pytorch pytorch/pytorch

     - ubuntu:16.04 이미지를 컨테이너에 올리면서, 별명은 "ubuntu_example"로 한다. 이 컨테이너는 docker network 중 mmlabnet에 속하도록 한다. 이 때, host의 /home/mmlab 폴더를 컨테이너 안의 /workspace와 연결한다. 또한 컨테이너의 포트 중 22번은 호스트의 8022번으로, 8000번 포트는 8000번으로 각각 포트 매핑을 시켜준다

4. 컨테이너&이미지 삭제

   ```
   $ docker ps //동작중인 컨테이너 확인
   $ docker ps -a // 정지된 컨테이너 확인
   $ docker rm [container id] // 컨테이너 삭제
   ```

   ```
   $ docker rmi [image id]
   ```

5. 기본 명령어

   - **docker ps -a** : 현재 실행중인 모든 컨테이너 보기
   - **docker start (컨테이너 명)** : 컨테이너 시작하기
   - **docker attach (컨테이너 명)** : 컨테이너 접속하기
   - **docker stop (컨테이너 명)** : 컨테이너 중단하기
   - **docker rm (컨테이너 명)** : 컨테이너 지우기
   - **docker image ls** : 현재 가지고 있는 모든 이미지 보기
   - **docker image rm (이미지 명)** : 이미지 지우기
   - *Ctrl + P + Q* : 컨테이너 나가기
   - **exit** : 컨테이너 종료

# Docker Container 내부 설정

1. 업데이터 및 설치

   ```
   $ apt-get update
   $ apt-get install vim
   $ apt-get install ssh
   ```

2. ssh 키 생성

   ```
   $ cd ~/
   $ ssh-keygen -t rsa -P '' -f ~/.ssh/id_dsa
   ```

3. sshd를 위한 폴더 생성

   ```
   $ mkdir /var/run/sshd
   ```

4. ~/.bashrc 파일에 다음을 추가

   ```
   # autorun
   /usr/sbin/sshd
   ```

5. 변경된 사항 적용

   ```
   $ source ~/.bashrc
   ```


   ```

   7-1. 루트 계정 비밀번호 수정

   ```
   $ passwd
   ```

   7-2. ssh 루트 계정 접속제한 해제하기

   ```
   $ vi /etc/ssh/sshd_config
   ```

   ```
   # PermitRootLogin without-password 을 아래로 변경
   PermitRootLogin yes
   ```

   ```
   $ /etc/init.d/ssh restart
   ```

7. FileZilla 접속

   sftp://().sogang.ac.kr 로 접속


이 글은 다음과 같이 구성됩니다.

1. Deployment 설정
2. Interpreter 설정
3. 프로젝트 연결 설정



## 1. Deployment 설정

1. FILE → Setting 들어가기
2. Build, Execution, Deployment → Deployment 들어가기
3. ssh 서버 등록하기 (이미 등록한 서버 이용 시에는 목록의 서버들 중 하나를 택함)
   1. ':heavy_plus_sign:' 버튼을 눌러 서버를 추가한 후,
   2. Connection: SFTP host, Port, Root path(root 계정일 경우 경로를 '/'로 설정 권장) 및 나머지 설정
   3. Mapping: Local path(로컬에서 프로젝트 경로), Deployment path on server '(계정)@(서버호스트):(포트번호)'(서버에서 개발 프로젝트 경로) 설정
   4. Apply

## 2. Interpreter 설정

1. FILE → Setting 들어가기 (이전과 동일)

2. Project: (프로젝트 명) → Project Interpreter 들어가기

3. ':gear:' 버튼 누른 후, 바로 인터프리터 추가 시 'Add...', 기존의 인터프리터 목록을 보고싶을 땐 'Show All...' 클릭

4. 인터프리터 추가하기

   1. Add Python Interpreter 창에서 SSH Interpreter 들어가기

   2. 새 서버 등록시 New server configuration에서 요구하는 정보 입력, 이미 등록된 서버를 이용 시 Existing server configuration에서 등록된 서버 선택 후 Next

   3. 서버에서 사용하고 싶은 인터프리터 위치를 입력

   4. 서버에서 3번의 인터프리터를 이용해 실행시키려는 프로젝트 위치 입력 

      :star2:중요:star2:**<project root>→/tmp/pycharm_project_488 = (1.3.2에서 설정한 root path)/tmp/pycharm_project_488** 이므로 서버에서 이미 만들어진 프로젝트 경로가 있다면 다시 설정해야한다!

   5. Finish

### 인터프리터 위치 확인하기 (python)

```python
import sys

print(sys.executable)
```



## 3. 프로젝트 연결 설정

### 파일 동기화

1. Tools → Deployment에서 파일 Upload 및 Download
2. 혹은 Remote Host 창을 열어 선택한 파일에 오른쪽 클릭 후 다운로드

### 파일 자동 업로드 설정

1. Tools → Deployment에서 파일 자동 업로드 설정: Automatic Upload(always) 체크 설정 및 Options...



## 그 외

- Getting Access to the Remote Host Tool Window: View → Tool Windows → Remote Host



# 참고자료

- [PyCharm SSH 연결하기](https://simonjisu.github.io/datascience/2018/06/24/pycharmssh.html)
- [Pycharm 원격 서버 연결하기](https://pytogether.tistory.com/1)
