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

    

