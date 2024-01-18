![Entry Logo](src/renderer/resources/images/about/logo.png)
---
# What is Entry-Offline ?

엔트리 오프라인은 [엔트리 웹 사이트](https://playentry.org/)에 접속할 수 없는 오프라인 환경에서도 엔트리를 사용할 수 있도록 제작된 프로그램입니다.  
엔트리 오프라인은 [Electron](https://electronjs.org/) 기반으로 만들어졌으며,
[entryjs](https://github.com/entrylabs/entryjs) 와 [entry-hw](https://github.com/entrylabs/entry-hw) 프로젝트를 [bower](https://bower.io/) 를 통해 내장하고 있습니다.

```
https://docs.playentry.org/guide/index.html
```

# JINIROBOT Entry Offline
**Entry에서 제공하는 가이드를 따라서 환경을 준비했지만 실행이 되지 않아 새로 작성함.**
<br>
**(Entry 가이드의 일부 내용을 업데이트 하지 않아 실행이 안되는 것으로 추정 됨.)**
## 사전 준비(개발 환경 구축 당시 버전)
* node 16.19.0 설치
    ```html
    https://nodejs.org/en/blog/release/v16.19.0
    ```

* python 3.7.0 설치 (3.2.x 버전 이상이면 상관 없는 듯)
    ```html
    https://www.python.org/downloads/release/python-370/
    ```

* electron 설치
    ```bash
    npm install --global electron
    ```

* yarn 설치
    ```bash
    npm install --global yarn
    ```

* node-gyp 설치
    ```bash
    npm install --global node-gyp
    ```

## 개발 환경 설정
### 1. 코드 준비
* Git 원격저장소에서 코드 다운로드
    ```bash
    git clone https://github.com/JINI-Robot/entry-offline.git
    ```

* 의존성 모듈 설치 (entry-offline 폴더에서)
    ```bash
    yarn
    ```
### 2. 테스트 실행
* 웹팩 번들링
    ```bash
    # 한 번만 실행하는 경우
    yarn webpack:dev
    
    # 코드 변경점을 지속적으로 감시하는 경우
    yarn webpack:watch
    ```
* 일렉트론 실행
<br>
(코드 변경점을 지속적으로 감시하는 경우 새 명령프롬프트에서 일랙트론을 실행할 것)
    ```bash
    yarn start
    ```

* 하드웨어 선택 후 앱이 멈춘다면 아래 명령 실행 후 웹팩 번들링부터 재시작
    ```bash
    yarn rebuild
    ```

## 코드 준비(커스터마이징)
### 1. entryjs
* Entry JS 개발폴더에서 빌드
    ```bash
    yarn dist
    ```

* Entry JS의 아래 폴더를 entry-offline 폴더로 이동
    ```bash
    # entryjs의 폴더
      entryjs\dist
      entryjs\extern
      entryjs\images
      entryjs\src

    # 복사할 폴더 위치
      entry-offline\node_module\entry-js
    ```
### 2. entry-hw
* entry-hw의 아래 폴더를 entry-offline 폴더로 이동
    ```bash
    # entry-hw의 폴더
      entry-hw\app\modules\[하드웨어 파일 이름].js
      entry-hw\app\modules\[하드웨어 파일 이름].json
      entry-hw\app\modules\[하드웨어 파일 이름].png
    
    # 복사할 폴더 위치
      entry-offline\node_module\entry-hw\app\modules
    ```

* 주의사항
    * 2024.01.02 현재<br>- Entry 추천 방식인 ES6 문법 class를 활용하는 방법으로 코드 생성 시 하드웨어가 동작하지 않음.<br>- 기존 자바스크립트 모듈화 기법으로 생성 시 하드웨어가 동작 함.
    * ES6문법(Entry 추천 방식: entry-offline에서 동작하지 않음)
        ```javascript
        const BaseModule = require('./baseModule');

        class [모듈명] extends BaseModule {        
            constructor() {
                super();            
            }

            init(handler, config) {
                this.handler = handler;
                this.config = config;
            }

            //...생략
        }

        module.exports = new [모듈명]();
        ```
    
    * 기존 자바스크립트 모듈화(entry-offline에서 동작 함)
        ```javascript    
        function Module() {
        }

        Module.prototype.init = function(handler, config) {
        };

        //...생략

        module.exports = new Module();
        ```

## Entry-Offline 빌드
### 1. 빌드 (entry-offline\dist에 생성 됨)
* entry-offline 빌드
    ```bash
    # windows
    yarn dist:win

    # windos x32    
    yarn dist:win32

    # windos x64
    yarn dist:win64

    # mac
    yarn dist:mac
    ```

### 2. 설치파일 생성 준비
* 아래 가이드 참고
    ```http
    https://docs.playentry.org/guide/entry-offline/2018-01-09-build.html
    ```

* nsis-3.07-setup.exe 설치파일 실행
    ```bash
    # 설치파일 위치
      NAS 서버\HDD1\Backup\LNY\04.엔트리개발환경설정\설치파일\nsis-3.07-setup.exe
    ```

* NsProcess.zip 압축 해제 후 install.exe 설치파일 실행
    ```bash
    # 압축파일 위치
      NAS 서버\HDD1\Backup\LNY\04.엔트리개발환경설정\설치파일\NsProcess.zip
    ```

* nsProcess.dll 파일을 c드라이브의 nsis 폴더로 복사
    ```bash
    # nsProcess.dll 파일 위치
      NsProcess\Plugin\nsProcess.dll
    
    # 복사할 폴더 위치
      C:\Program Files (x86)\NSIS\Plugins\x86-unicode
    ```
### 3. Entry-offline 설치파일 생성
* entryx86.nsi 또는 exntryx64.nsi 파일을 오른쪽 클릭
    ```bash
    # entry-offline(x86) setup 생성 시
      entry-offline\build\entryx86.nsi

    # entry-offline(x64) setup 생성 시
      entry-offline\build\entryx64.nsi
    ```


* 드랍다운 목록의 Compile NSIS Script 를 클릭하여 setup 파일 생성 시작

* 생성 완료 후 setup 파일 확인
    ```bash
    # setup 파일 생성 위치
      entry-offline\build
    ```