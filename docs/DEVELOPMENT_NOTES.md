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