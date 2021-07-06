### React

___

- ##### node.js

  - babel (compiler): for jsx => js

  ```jsx
  # jsx example
  return (
    <h1>Greetings, {this.props.name}!</h1>
  );
  ```

  ```javascript
  # converted to pure js
  return React.createElement('h1', null, 'Greetings, ' + this.props.name + '!');
  ```

  - webpack (bundler)
    - webpack-dev-server
    - 파일을 import/export 하면서 분리해 작성하고 이후 webpack을 통해 하나의 파일로 합침

  - npm (package manager)
    - easier package management

  
  
- ##### webpack features

  - **js bundling**: 자바스크립트를 모듈로 작성할 수 있기 때문에 각각의 파일에 대해서 script 태그를 별도로 작성할 필요가 없음
  - **Hot Module Replacement**: 전체 새로고침 없이 모든 모듈을 런타임 시점에 업데이트
  - **LESS/SCSS to CSS**
  - **코드 압축/최적화**

  

- ##### webpack-dev-server

  - 실시간 리로드 기능을 갖춘 개발 서버 (≒ VS Code Live Server extension)
  - 디스크에 저장되지 않는 메모리 컴파일을 사용하기 때문에 컴파일 속도가 빠름
  - 동작 원리
    1. 서버 실행 시 소스 파일들을 번들링하여 메모리에 저장소스 파일을 감시
    2. 소스 파일이 변경되면 변경된 모듈만 새로 번들링
    3. 변경된 모듈 정보를 브라우저에 전송
    4. 브라우저는 변경을 인지하고 새로고침되어 변경사항이 반영된 페이지를 로드
  
  
  
- setState() callback

  ```react
  this.setState({
      key1: value1,
  }, function () {console.log(this.state.key1)})
  
  // arrow function
  this.setState({
      key2: value2,
  }, () => console.log(this.state.key2));
  ```



- 검색 로직

  ```react
  // Chat.js
  
    constructor(props) {
      super(props);
      this.state = {
        userList: [],
        searchKeyword: "",
        searchResult: [],
      }
  
    onSearchKeywordChange = (e) => {
      const userList = this.state.userList;
      this.setState({
        searchKeyword: e.target.value 
      }, () => {
        this.searchCaller(userList, this.state.searchKeyword)
      });
    }
  
    searchCaller = (originalList, keyword) => {
      if (keyword) {
        let userFound = originalList.some(user => user.caller.includes(keyword));
        if (userFound) {
          let tmpUserList = originalList.filter(user => user.caller.includes(keyword));
          this.setState({
            searchResult: tmpUserList
          });
        } else {
          this.setState({
            searchResult: []
          });
        }
      } else {
        this.setState({
          searchResult: []
        });
      }
    }
    
    render() {
      const dataList = this.state.dataList;
      const userList = this.state.userList;
      const selectedUserIdx = this.state.selectedUserIdx;
      const selectedCaller = this.state.selectedCaller;
      const searchKeyword = this.state.searchKeyword;
      const searchResult = this.state.searchResult;
  
      return (
        <div>
          <section
            className="discussions"
            style={{
              visibility: this.state.discussionsVisibility,
              width: this.state.discussionsWidth,
              transition: "0.3s",
            }}
          >
            <div className="discussion search">
              <div
                className="searchbar"
                style={{
                  visibility: this.state.searchbarVisibility,
                }}
              >
                <i className="fa fa-search" aria-hidden="true"></i>
                <input 
                  type="text" 
                  placeholder="Search..."
                  value={this.state.searchKeyword}
                  onChange={this.onSearchKeywordChange}
                  onBlur={(e) => {
                      this.onSearchOff()
                    }}
                >
                </input>
              </div>
            </div>
  
            <Discussion
              searchKeyword={searchKeyword}
              searchResult={searchResult}
              userList={userList}
              selectedUserIdx={selectedUserIdx}
              selectedCaller={selectedCaller}
              onClick={this.selectedUserHandler}
            />
          </section>
        </div>
      );
    }
  }
  ```

  ```react
  // Discussion.js
    
    render() {
      const userList = this.props.userList;
      const selectedUserIdx = this.props.selectedUserIdx;
      const searchKeyword = this.props.searchKeyword;
      const searchResult = this.props.searchResult;
  
      if (searchKeyword && searchResult.length > 0) {
        return (
          <div className="disc-msg-container">
            {}
            {searchResult.map((user, idx) => {
              return (
                <div
                  className={
                    idx === selectedUserIdx
                      ? "discussion message-active"
                      : "discussion"
                  }
                  key={idx}
                  onClick={this.selectedUserInfoHandle(idx)}
                >
                  <div className="desc-contact d-flex align-items-center">
                    <p className="name">{user.caller}</p>
                  </div>
                  <div className="dropdown">
                    <i
                      className="dropdown-icon clickable fa fa-ellipsis-v"
                      data-toggle="dropdown"
                      aria-hidden="true"
                      aria-haspopup="true"
                      aria-expanded="false"
                    ></i>
                    <div
                      className="dropdown-menu"
                      aria-labelledby="dropdownMenuButton"
                    >
                      <p className="dropdown-item">세션전환</p>
                    </div>
                  </div>
                </div>
              );
            })}
          </div>
        );
      } else if (searchKeyword && searchResult.length === 0) {
        return (
          <div className="disc-msg-container">
            <div className="discussion">
              <div className="d-flex align-items-center">
                <p className="name">No Search Results</p>
              </div>
            </div>
          </div>
        )
      } else {
        return (
          <div className="disc-msg-container">
            {userList.map((user, idx) => {
              return (
                <div
                  className={
                    idx === selectedUserIdx
                      ? "discussion message-active"
                      : "discussion"
                  }
                  key={idx}
                  onClick={this.selectedUserInfoHandle(idx)}
                >
                  <div className="desc-contact d-flex align-items-center">
                    <p className="name">{user.caller}</p>
                  </div>
                  <div className="dropdown">
                    <i
                      className="dropdown-icon clickable fa fa-ellipsis-v"
                      data-toggle="dropdown"
                      aria-hidden="true"
                      aria-haspopup="true"
                      aria-expanded="false"
                    ></i>
                    <div
                      className="dropdown-menu"
                      aria-labelledby="dropdownMenuButton"
                    >
                      <p className="dropdown-item">세션전환</p>
                    </div>
                  </div>
                </div>
              );
            })}
          </div>
        );
      }
    }
  ```

  

