### WSL, Docker

- WSL2
  - 정식 버전 기준: gpu 가속 지원 안 됨 (21년 4월 예정)
  - Insider Preview 필요
  - https://docs.nvidia.com/cuda/wsl-user-guide/index.html#running-cuda



- WSL에서 Docker 실행 시

  - systemctl 사용 불가 => service 사용

    ```
    sudo systemctl restart docker  =>  sudo service docker restart
    ```

    

- Docker 컨테이너 실행

  ```
  docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
  ```

  - 자주 사용하는 옵션
    - -d: detached mode (백그라운드 모드)
    - -p: 호스트와 컨테이너의 포트를 연결 (포워딩)
    - -v: 호스트와 컨테이너의 디렉토리를 연결 (마운트)
    - -e: 컨테이너 내에서 사용할 환경변수 설정
    - -name: 컨테이너 이름 설정
    - -rm: 프로세스 종료시 컨테이너 자동 제거
    - -it: -i & -t, 터미널 입력을 위한 옵션
    - -link: 컨테이너 연결





- 이미지 로드 + 컨테이너 생성 + bash 실행 (-rm 적용할 경우 생성된 컨테이너가 종료 시 자동 삭제됨)

```
docker run -it docker.maum.ai:443/maum-sdn:1.0.4 /bin/bash
```

- 이미지 로드 + 컨테이너 생성 + bash 실행 (-rm 적용할 경우 생성된 컨테이너가 종료 시 자동 삭제됨)

```
docker run -it --name bbj docker.maum.ai:443/maum-sdn:1.0.4 /bin/bash
```

- 로컬(host)의 /home/mindslab/bbj 디렉토리를 bbj 컨테이너의 /var/lib/bbj로 연결하면서 maum-sdn:1.0.4 이미지 로드+컨테이너 생성+bash 실행

```
docker run -it -v /home/mindslab/bbj:/var/lib/bbj --name bbj docker.maum.ai:443/maum-sdn:1.0.4 /bin/bash
```

- 컨테이너 종료

```
exit
```

- bbj 컨테이너 실행

```
docker start bbj
```

- host OS로 이동(컨테이너 종료하지 않음)

```
ctrl + p + q
```

- 컨테이너로 복귀(컨테이너 실행되어 있을 경우에만)

```
docker attach bbj
```



- #### 포트

  - ```
    -p 80:5000
    > 호스트의 80번과 컨테이너의 5000번 포트를 연결
    > 외부에서 서버 80포트로 접근 => 컨테이너의 5000번 포트에 접속
    ```

  - 외부에서 컨테이너 안에서 실행중인 서비스에 접근하려면 해당 서비스가 사용하는 포트를 외부 호스트 포트와 연결해주어야 함 

____

- nlp 컨테이너 실행

```
docker container start nlp
```

- nlp 컨테이너 접속

```
docker exec -it nlp bash
```

- 컨테이너 생성

```
docker create
> 컨테이너를 만들고 아무것도 하지 않음

docker run
> 컨테이너를 만들고 실행
```

- 자원 할당

```
--memory 1g, 1024m (기본값 - 전체)
--cpu-shares <숫자. 기본값: 1024>
--cpuset-cpus <설정. ex) 2, "0,2", "0-2">
--gpus <설정. 기본값:사용x ex) all, 2, '"device=1,2"'>
```

- 네트워크 옵션

```
--net <mode>
> bridge: 기본값, host 안에 격리된 내부망
> host: host의 네트워크를 사용
> none: 네트워크를 사용하지 않음
> container:<name, id>: 다른 컨테이너의 네트워크를 사용
-p <host>:<container>
> host의 port를 container의 port에 연결하는 옵션
> netword mode가 bridge일 때 작동하며, port forwarding으로 이해하면 됨
```

- 볼륨 옵션

```
-v <host>:<container>
> host의 디렉토리를 container에 mount시키는 옵션
--volumes-from <name>
> 다른 container의 volume을 연결하는 옵션
```



___

- docker save (image→tar) 파일명 지정을 위해 -o 옵션을 이용한다.

```
\# docker save [option] [save_name] [image_name]
$ docker save -o docker_image.tar container1
$ docker save -o ratsgo_knlp.tar ratsgo/embedding-gpu:latest
```

- docker load (tar→image)

```
$ docker load -i docker_image.tar
```

- docker export (image+container → tar)

```
\# docker export [container name or ID] [save_name]
$ docker export container1 docker_container.tar
$ docker export nlp > ratsgo_knlp_0201.tar
```

- docker import (tar → image+container)

```
$ docker import docker_container.tar
$ docker import ratsgo_knlp_0201.tar
```



____

- 이미지 삭제

```
$ docker rmi 49c9db782d4d
```

- 새 이미지 태그

```
$ docker image tag 기반이미지명[:태그] 새이미지명[:태그]
```

____

- 커밋(변경사항 저장한 새 이미지 생성)

```
sudo docker commit -p -a [AUTHOR] -m [COMMIT MESSAGE] [CONTAINER ID] [NEW IMAGE NAME[:TAG]]
```

- commit - save - load - run
- 푸쉬(도커허브에 업로드)

```
docker push <Docker Hub 사용자 계정>/<이미지 이름>:<태그>
```

