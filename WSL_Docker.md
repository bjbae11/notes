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

- 로컬(host)의 /home/mindslab/bbj 디렉토리를 컨테이너의 /var/lib/bbj로 복사하면서 이미지 로드+컨테이너 생성+bash 실행

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



____

- nlp 컨테이너 실행

```
docker container start nlp
```

- nlp 컨테이너 접속

```
docker exec -it nlp bash
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