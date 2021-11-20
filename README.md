# Node.js
## npm install
- 참고 사이트 [https://c17an.netlify.app/blog/node.js/npm-install-%EC%A0%95%EB%A6%AC/article/]
### npm install 과 패키지
- ``npm install``을 아래의 두가지 동작으로 구분
  - 패키지명을 명시해 특정 패키지를 설치하는 동작
  - ``package.json`` 파일의 의존성을 먼저 명시한 후, 설치하는 동작
- 사용예
```bash
# express 모듈이라는 특정 패키지를 설치
$ npm install express
# package.json 에 포함된 의존성 패키지들이 일괄적으로 설치
$ npm install
```
#### 특정 패키지를 설치할 때
- 특정 패키지를 설치할 때는 크게 두 가지 옵션으로 구분
  - 옵션1: 프로젝트를 구동할 때 필요한 ``dependencies`` 목록에 추가될 ``$npm install (프로젝트명)`` 으로 프로젝트를 설치하는 옵션
  - 옵션2: 개발 단계에서만 필요한 ``devDependencies`` 목록에 추가될 ``$npm install -D (프로젝트명)`` 옵션
- ``-D`` 와 같은 접미어를 “플래그” 라고 부르는데, 주로 사용되는 플래그는 다음과 같음

| 플래그     | 효과 |
| ---------  | ----------- |
| -P         | 패키지를 설치하고 프로젝트의 dependencies 목록에 추가|
| —save-prod | 패키지를 설치하고 프로젝트의 dependencies 목록에 추가|  
| -D         | 패키지를 설치하고 프로젝트의 devDependencies 목록에 추가|  
| —save-dev  | 패키지를 설치하고 프로젝트의 devDependencies 목록에 추가|
| -g         | 패키지를 프로젝트가 아닌 시스템의 node_modules 폴더에 설치|

##### ``-P, —save-prod`` 플래그를 사용할 때
- ``-P, --save-prod`` 플래그는 사용할 일이 많지 않음
  - 왜냐하면, ``-P`` 플래그의 효과는 기본 ``$ npm install (패키지)`` 와 완전히 동일하기 때문
  - ``-P`` 플래그는 패키지를 설치한 후, 프로젝트의 패키지 dependencies 목록에 추가
- 결론: ``-P`` 플래그(기본 옵션) 는 프로젝트의 의존성 패키지 dependencies 목록에 추가  
##### ``-D, —save-dev`` 플래그를 사용할 때
- ``-D`` 플래그는 기본 ``-P`` 와 동일하게 프로젝트의 node_modules 폴더에 패키지를 설치
- 하지만, 패키지명을 ``dependencies`` 가 아닌 ``devDependencies`` 목록에 추가함
- ``dependencies`` 와 ``devDependencies`` 의 차이
  - ``dependencies`` : ``express`` 패키지처럼 실제 코드에도 포함되며, 앱 구동을 위해 필요한 의존성 파일들
  - ``devDependencies`` : ``concurrently`` 패키지처럼 실제 코드에 포함되지 않으며, 개발 단계에만 필요한 의존성 파일들
- 결론: ``-D`` 플래그를 사용하면 개발 전용 패키지 devDependencies 목록에 추가
##### -g, —global 플래그를 사용할 때
- ``-g`` 또는 ``--global`` 플래그는 약간 다른 동작을 수행
  - ``$npm install (패키지명)`` 은 프로젝트 폴더에 패키지를 설치
  - 하지만, ``-g`` 플래그를 통해 패키지를 설치하면 시스템 폴더에 패키지를 설치
- 시스템 폴더는 ``Win10`` 기준으로는 ``(사용자명)\AppData\Roaming\npm\node_modules``
- 시스템의 ``node_modules`` 폴더 경로는 ``npm root -g`` 를 통해 찾을 수 있음
- ``-g`` 플래그를 사용할 경우, **package.json 의 의존성 목록에 기록되지 않음**
```bash
$ npm root -g
C:\Users\gusam\AppData\Roaming\npm\node_modules
```
- 결론: -g 플래그를 사용하면 패키지를 시스템 폴더에 설치

### 의존성 패키지를 설치할 때
- 패키지명을 붙이지 않고, ``$ npm install`` 만을 실행하게 되면 프로젝트의 ``package.json`` 에 기록된 모든 의존성 패키지들을 다운로드
  - 이런 경우에도 플래그 사용 가능
- devDependencies 파일은 개발에만 사용되니, 사용자들은 이 패키지를 내려받을 필요없음
  - ``-production`` 플래그를 붙이면, devDependencies 를 제외한 의존성 파일만을 내려받음
- 사용예제
  - 만약 아래의 ``packages.json``이 있고, ``$npm install -production``을 실행
    - 프로젝트는 ``concurrently`` 패키지는 무시하고, ``express`` 패키지만을 설치
```javascript
# packages.json
{
  "devDependencies": {
    "concurrently": "^5.3.0"
  },
  "dependencies": {
    "express": "^4.17.1"
  }
}
```
- 결론: ``-production`` 플래그를 사용하면, devDependencies 목록을 제외한 패키지들을 설치

### 결론
- ``npm install``할 때, 플래그를 사용해 ``dependencies`` 와 ``devDependencies``로 의존성 목록을 구분하는 것이 좋음