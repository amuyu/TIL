

# GraalVM 이 왜?
`GraalVM for Microservices` 
기동 시간이 50배, 메모리도 5배 작게 사용한다고 한다
Microservice 를 위한 vm인가
무/유료 구분을 하는데 당연히 유료가 성능이 좋다

# 기능
* Ahead-of-Time Compilation : 타킷 OS에 맞는 실행 파일을 만든다 -> OS에 java를 설치할 필요 없음, 빠르게 실행 가능
* Language Choice : Ruby, Pytohh, R 등 모든 언어를 실행할 수 있게 해준다.
* Advaned Tools : Dashboard, Chrome Debugger, VisualVM 등

JIT? Just-in-Time? 바이트코드를 CPU로 보낼 수 있는 명령어로 바꾸는 프로그램 -> 성능 최적화를 위해 애플리케이션 런타임을 향상시킬수 있다.? ->바이트 코드를 기계 언어 또는 네이티브 코드로 바꾸기 때문에 실행이 빨라진다

# 설치 (macOS)
1. Download package
https://github.com/graalvm/graalvm-ce-builds/releases 에서 os에 맞는 파일을 다운로드 받는다.
2. unzip the archive
```shell
tar -xzf graalvm-ce-java<version>-darvin-amd64-<version>.tar.gz
```
3. move
Move the downloaded package to its proper location
```shell
sudo mv graalvm-ce-java<version>-<version> /Library/Java/JavaVirtualMachines
```



# ref
[GraalVM, Spring Native 맛보기](https://meetup.toast.com/posts/273)