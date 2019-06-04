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
   ```

3. ssh 키 생성

   ```
   $ cd ~/
   $ ssh-keygen -t rsa -P '' -f ~/.ssh/id_dsa
   ```

4. sshd를 위한 폴더 생성

   ```
   $ mkdir /var/run/sshd
   ```

5. ~/.bashrc 파일에 다음을 추가

   ```shell
   # autorun
   /usr/sbin/sshd
   ```

6. 변경된 사항 적용

   ```
   $ source ~/.bashrc
   ```

7. (일반) 유저 추가

   ```
   $ adduser (유저명)
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

8. FileZilla 접속

   sftp://().sogang.ac.kr 로 접속

# 참고자료

- [가장 빨리 만나는 도커(Docker)](http://pyrasis.com/private/2014/11/30/publish-docker-for-the-really-impatient-book)
- [초보를 위한 도커 안내서](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)
- [컨테이너 기반 가상화 플랫폼 ‘도커(Doker)’의 이해](https://tacademy.sktechx.com/live/player/onlineLectureDetail.action?seq=125)
- [Docker 설치문서](https://github.com/sogang-mm/lab/wiki/Docker-%EC%84%A4%EC%B9%98-%EB%AC%B8%EC%84%9C) 
- [66. [Docker] Docker 컨테이너로의 sftp 사용 - filezilla를 통한 예제](https://m.blog.naver.com/PostView.nhn?blogId=alice_k106&logNo=220650722592&proxyReferer=https%3A%2F%2Fwww.google.com%2F) 
- [ssh 루트(root) 접속 제한(Permission denied) 해제하기](http://blog.naver.com/PostView.nhn?blogId=chandong83&logNo=220919303234&categoryNo=16&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=section)
