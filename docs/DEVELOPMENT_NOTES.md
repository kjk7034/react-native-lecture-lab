# 진행 과정 메모

## Ch 1. 웹뷰 기초 이론

네이티브 앱, 웹 앱, 하이브리드 앱, 웹뷰에 대한 설명

## Ch 2. React Native CLI를 이용한 기초 웹뷰 개발

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

## Ch 3. Expo를 이용한 기초 웹뷰 개발
