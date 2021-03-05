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

  - 실시간 리로드 기능을 갖춘 개발 서버 (≒ VScode Live Server extension)
  - 디스크에 저장되지 않는 메모리 컴파일을 사용하기 때문에 컴파일 속도가 빠름
  - 동작 원리
    1. 서버 실행 시 소스 파일들을 번들링하여 메모리에 저장소스 파일을 감시
    2. 소스 파일이 변경되면 변경된 모듈만 새로 번들링
    3. 변경된 모듈 정보를 브라우저에 전송
    4. 브라우저는 변경을 인지하고 새로고침되어 변경사항이 반영된 페이지를 로드