# 진행 과정 메모

## Part 1. React Native를 이용한 기초 웹뷰 개발

### Ch 1. 웹뷰 기초 이론

네이티브 앱, 웹 앱, 하이브리드 앱, 웹뷰에 대한 설명

### Ch 2. React Native CLI를 이용한 기초 웹뷰 개발

강의에 있는 0.73버전 문서를 확인.

[React Native > Environment setup > Setting up the development environment](https://reactnative.dev/docs/0.73/environment-setup?guide=native)을 참고하여 환경 설정

macOS iOS, Android 모두 확인

1. Node(강의에서는 Homebrew 통해서 설치)
   - `brew install node`
2. Watchman
   - `brew install watchman`
3. Xcode 다운로드
   - 상세 설정은 문서 참조
   - 시뮬레이터 추가
4. Ruby 설치 ([rbenv github](https://github.com/rbenv/rbenv) 참고)
   - `brew install rbenv`
   - `rbenv install -l`
   - `gem install bundler`
5. Java Development Kit
   - `brew install --cask zulu@17`
   - `brew info --cask zulu@17`로 확인
6. Android Studio 설치
   - 상세 내용은 문서 참조
   - 환경 변수 추가
   ```
   export ANDROID_HOME=$HOME/Library/Android/sdk
   export PATH=$PATH:$ANDROID_HOME/emulator
   export PATH=$PATH:$ANDROID_HOME/platform-tools
   ```
7. 프로젝트 설치
   - `npx react-native@0.73.8 init Ch2ProjectSetup --version 0.73.8`
   - Ch2ProjectSetup에서 `bundle install` 실행(`Gemfile` 내용 설치)
   - Ch2ProjectSetup/ios/에서 `bundle exec pod install` 실행(`Podfile` 내용 설치)
8. 환경에 맞게 start `yarn ios`, `yarn android`

설치 시 변경되는 환경 변수들에 대해서 `source ~/.zprofile`(`zshrc`) 실행

#### 경험한 문제

1. `yarn ios`대신 xcode에서 실행 시 Xcode > `Ch2ProjectSetup.xcodeproj`으로 Open 시 `Library 'CocoaAsyncSocket' not found` 오류사 발생. `Ch2ProjectSetup.xcworkspace`으로 실행해야 함.

2. Webview uri 중 일부 uri가 `NSURLErrorDomain` 관련 이슈가 발생했는데, 회사에서 테스트 시 ssl 문제로 확인

#### 웹뷰로 웹 사이트 로드하기

[React Native WebView Getting Started Guide](https://github.com/react-native-webview/react-native-webview/blob/master/docs/Getting-Started.md)를 보면서 진행.

가이드에는 ios에서 `pod install`을 하지만 `bundle exec pod install`을 실행.
(사유: 시스템에 설치된 다른 버전의 CocoaPods와의 충돌을 방지, Gemfile.lock에 명시된 버전을 정확히 사용하여 일관성 유지)

### Ch 3. Expo를 이용한 기초 웹뷰 개발

- 관련문서 : [create-expo-app - Expo Documentation](https://docs.expo.dev/more/create-expo/#--template)

- `yarn create expo --template expo-template-blank-typescript@50`으로 테스트를 했으나, `Error: Cannot find module 'math-intrinsincs/max'` 오류가 발생
  - 가이드 문서처럼 최신 버전으로 `yarn create expo-app --template blank-typescript` 설치(blank-typescript@50도 했지만 동일한 오류가 발생.)
- `yarn expo install react-native-webview`으로 설치가 진행되지 않아서 `npx expo install react-native-webview`으로 진행함.

## Part 2. 웹뷰 띄우기 학습. 네이버 앱 클론 코딩 프로젝트

- npx react-native@0.73.8 init NaverAppClone --version 0.73.8
- ios에서 `No bundle URL present. Make sure you're running a packager server or have included a .jsbundle file in your application bundle.` 이슈가 발생함.
  - [[React-Native Error Log] No bundle URL present 이슈](https://velog.io/@haerim95/React-Native-Error-Log-No-bundle-URL-present-%EC%9D%B4%EC%8A%88) 링크의 내용과 하단 유튜브 영상을 참고하여 해결.
- Navigation 관련해서는 [React Navigation](https://reactnavigation.org/docs/getting-started) 설치.
  - `npx pod-install ios` 대신 `bundle exec pod install`으로 설치
  - Android 설치 시 `sdk/ndk/25.1.8937393 did not have a source.properties file` 오류가 발생
    - Android Studio > Tools > SDK Manager > SDK Tools > NDK (Side by side)에서 **해당 버전**을 찾아서 설치.
  - `FAILURE: Build failed with an exception. * What went wrong: Execution failed for task ':react-native-safe-area-context:compileDebugKotlin'. > A failure occurred while executing org.jetbrains.kotlin.compilerRunner.GradleCompilerRunnerWithWorkers$GradleKotlinCompilerWorkAction > Compilation error. See log for more details` 위 이슈는 `react-native-safe-area-context`가 ^5.0.0으로 설치되어 발생한 이슈로 ^4.0.0으로 수정하여 해결. (5버전은 react-native 0.74부터 지원: 프로젝트는 0.73.8)

### Part 2. 학습 리뷰

#### 홈 스크린/쇼핑 스크린

- 탭 네비게이션 구현
  - react-navigation
- 아이콘 라이브러리 사용하기
  - react-native-vector-icons
- 웹뷰에 웹사이트 로드하기
- Pull To Refresh 구현하기
  - ScrollView
  - RefreshControl
- 웹사이트 리퀘스트 핸들링
  - onShouldStarLoadWithRequest

#### 브라우저 스크린

- 웹 사이트 현재 주소 보여주기
  - onNavigationStateChange
    - 현재주소, 뒤로가기 가능, 앞으로가기 가능 상태 접근
- 웹 사이트 로딩 바 구현
  - onLoadProgress, onLoadEnd
- 브라우저 네비게이션 기능 구현 (앞으로, 뒤로, 새로고침)
  - Webview 인스턴스 접근
  - 웹뷰 새로고침 화면 수정 : renderLoading
- iOS, Android 네이티브 공유 기능 구현
  - Share API

#### 로그인 스크린

- 로그인 후 웹뷰 새로 고침 기능 구현
  - Context를 이용한 전역적인 상태 관리
- 쿠키 읽기 및 쓰기
  - @react-native-cookies/cookies
  - document.cookie

#### 웹앱 최적화

- 핀치 줌/아웃 비활성화 하기
- 링크 롱 프레스 프리뷰 비활성화 하기
- 안드로이드 백버튼과 웹뷰 연결 하기
- 텍스트 롱 프레스 액션 비활성화 하기

#### 앱 설정

- 앱 이름 변경하기
  - Info.plist, strings.xml
  - app.json
- 앱 아이콘 변경하기
  - AppIcon 이미지 셋 변경, ic_launcher.png 변경

## Part 3. 웹뷰와 RN 앱 통신을 학습 - 유튜브 SDK 활용 학습 앱 프로젝트

### Part 3. 학습 리뷰

#### 링크 컴포넌트 구현

- TextInput을 이용한 입력 컴포넌트 구현
- query-string 패키지를 이용한 유튜브 id 파싱

#### 유튜브 영상 로드

- Static HTML을 이용한 유튜브 웹 API 사용
- 뷰포트 크기 설정하기
- 영상 전체 화면 재생 비활성화
  - allowsInlineMediaPlayback
- 영상 자동 재생 활성화
  - mediaPlaybackRequiresUserAction=false

#### 재생 / 멈춤 기능 구현하기

- injectJavascript 함수를 이용해 앱에서 웹으로 통신하기
- window.ReactNativeWebView.postMessage / onMessage를 통해서 웹에서 앱으로 통신하기
- 웹에서 보내는 다양한 메시지 타입 제어하기

#### Seek Bar 구현하기

- PanResponder를 이용하여 사용자 제스처 처리
- Animated.timing을 이용하여 부드러운 애니메이션 구현

#### 구간 반복 기능 구현하기

- 앱과 웹이 서로 통신하여 반복 기능 구현하기
- 웹뷰 디버깅 하기

## Part 4. 앱 리소스 사용 학습 - 챗GPT API 활용 클로바ST 녹음요약 앱 프로젝트

### Part 4. 학습 리뷰

#### 웹-녹음 기능 구현

- Nextjs를 이용한 웹 프로젝트 초기화
- Tailwind를 이용한 CSS 작업
- MediaRecorder API를 이용한 웹 녹음 기능 구현

#### 웹 – 스크립트 추출 및 요약

- OpenAI의 Whisper API를 이용하여 Speech To Text 구현
- OpenAI의 ChatGPT API를 이용하여 텍스트 요약 기능 구현

#### 앱 – 웹 사이트 로드하기

- 웹뷰를 이용하여 웹 사이트 로드
- 디바이스에서 테스트 하기 위해서 터널링 서비스이용 (ngrok)

#### 앱 – 녹음 기능 구현

- 웹과 앱 동작 분기하여 웹 프로젝트 구현
  - window.ReactNativeWebView 존재 여부 확인
- 웹에서 앱으로 녹음 요청
- 앱의 녹음 API를 이용하여 오디오 녹음
  - react-native-audio-recorder-player / expo-av
  - react-native-permissions
  - react-native-fs / expo-file-system
- 앱의 녹음 파일을 웹으로 전달
  - Base64 인코딩
  - webview.postMessage

#### 앱 – 사진 첨부 기능 구현

- 웹에서 앱으로 사진 촬영 요청
- 앱의 카메라 API를 이용하여 사진 촬영
  - react-native-vision-camera / expo-camera
  - react-native-fs / expo-file-system
- 앱의 사진 파일을 웹으로 전달
  - Base64 인코딩
  - webview.postMessage

#### 앱 – 데이터 저장 기능 구현

- 웹의 데이터를 앱 스토리지에 영구 저장
  - react-native-async-storage
