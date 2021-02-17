### ratsgo 한국어 임베딩 실습 환경 셋팅

___

0. establish wsl ubuntu + gpu environment
    - subscribe for Windows 10 Insider Preview
    - enable WSL2
    - get latest WSL patch from Windows Update 
    - install Ubuntu
    - get CUDA for WSL2
      
      - ~~**nvidia-smi is not included**~~
    - get Docker-CE for Linux

      - CUDA for WSL:

        - **1/28/2020**: CUDA Toolkit 11.2 support, nvidia-smi, NVML support and critical performance improvements

          - Note that NVIDIA Container Toolkit does not yet support [Docker Desktop WSL 2](https://docs.docker.com/docker-for-windows/wsl/) backend. Use Docker-CE for Linux instead inside your WSL 2 Linux distribution.

          - nvidia-smi is now supported but in order to use it, please copy it to /usr/bin and set appropriate permissions with the below commands:

            ```
            cp /usr/lib/wsl/lib/nvidia-smi /usr/bin/nvidia-smi
            chmod ogu+x /usr/bin/nvidia-smi
            ```

            

1. get ratsgo/embedding-gpu image

   ```
   docker pull ratsgo/embedding-gpu
   ```

   

2. create container and load

   ```
   docker run -it --name [CONTAINER NAME] --gpus all -p 8888:8888 -v [{HOST DIRECTORY}]:[/notebooks/embedding/{DIRECTORY}] ratsgo/embedding-gpu /bin/bash
   ```

   

3. in the container,

   ```
   jupyter notebook --allow-root
   ```



4. copy & paste given url to your browser