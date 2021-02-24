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

  




### clone & get branches 

___

```
git clone [repo]
```

```
git remote update
```

```
git branch (-r:원격 브랜치 목록 -a:로컬+원격 브랜치 목록)
```





### git rebase

____

```

```

- **이미 공개 저장소에 Push 한 커밋을 Rebase 하지 마라**
  - Rebase는 기존의 커밋을 그대로 사용하는 것이 아니라 내용은 같지만 다른 커밋을 새로 만든다. 새 커밋을 서버에 Push 하고 동료 중 누군가가 그 커밋을 Pull 해서 작업을 한다고 하자. 그런데 그 커밋을 `git rebase` 로 바꿔서 Push 해버리면 동료가 다시 Push 했을 때 동료는 다시 Merge 해야 한다. 그리고 동료가 다시 Merge 한 내용을 Pull 하면 내 코드는 정말 엉망이 된다.

