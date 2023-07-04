# [TIL & Issue Note] 20230126


## Issue
- flutter 환경을 설정하던 중 안드로이드 라이센스 관련 명령어 실행시 에러
- UnsupportedClassVersionError while 'flutter doctor --android-licenses' MacOS

### 사용 환경
- MacOS Ventura (M1 Mac)
- Android Studio Electric Eel

### 에러 내용
```bash
Exception in thread "main" java.lang.UnsupportedClassVersionError: 
com/android/prefs/AndroidLocationsProvider 
has been compiled by a more recent version of the Java Runtime (class file version 55.0),
this version of the Java Runtime only recognizes class file versions up to 52.0
```

### 해결 내용
- [Agree to Andorid License](https://docs.flutter.dev/get-started/install/macos#agree-to-android-licenses)
```
1. Make sure that you have a version of Java 8 installed and that your JAVA_HOME environment variable is set to the JDK’s folder.

2. Android Studio versions 2.2 and higher come with a JDK, so this should already be done.
```
공식 문서 내용에 Java 8 설치와 `JAVA_HOME` 환경변수가 설정되어있는지 먼저 확인하라고 나와있었다. 내 맥북 글로벌 Java 버전은 19를 사용중이었다. 일단 Java 8 설치 후 글로벌 버전을 Java 8로 변경 후  `JAVA_HOME` 을 등록함.

```bash
brew tap AdoptOpenJDk/openjdk
brew install adoptopenjdk/openjdk/adoptopenjdk8

# jenv 
jenv add /Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home
jenv global 1.8
```
1.8 버전 확인
```bash
$ java -version
openjdk version "1.8.0_292"
OpenJDK Runtime Environment (AdoptOpenJDK)(build 1.8.0_292-b10)
OpenJDK 64-Bit Server VM (AdoptOpenJDK)(build 25.292-b10, mixed mode)
```
`~/.zshrc` 파일에 `JAVA_HOME` 환경 변수 추가
```bash
$ vi ~/.zshrc

export JAVA_HOME=$(/usr/libexec/java_home)
```

🙃🙃 개빡세네 

{{<figure src="/posts/images/TIL/20230126235111.png">}}
