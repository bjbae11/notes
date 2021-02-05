### Git Submodules

___

- git repo와 연동된 main프로젝트 내에 또 다른 git repo(submodule)를 연동시키려고 할 때

  1. submodule을 담을 폴더 생성

  2. 생성된 폴더 내에서

     ```
     git submodule add <submodule repository>
     ```

  3. 다른 장소에서 main 프로젝트 clone 시 submodule은 따라오지 않음

  4. submodule 따로 불러오기(submodule 경로에서)

     ```
     git submodule init
     git submodule update
     ```

     또는 main 프로젝트 clone 시

     ```
     git clone --recurse-submodules <mainpjt repository>
     ```

     => submodule 내용물까지 모두 clone해온다



- submodule 최신 상태로 갱신

  1. submodule 경로로 이동

     ```
     git fetch
     git merge origin/master
     ```

  2. 또는 main 프로젝트 경로에서

     ```
     git submodule update --remote
     ```

     

  