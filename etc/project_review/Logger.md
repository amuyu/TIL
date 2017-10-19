# logger
log 저장하는 라이브러리 프로젝트
dependencies가 없다(dependencies 어떻게 하는지 보려고 했는데..)
## 클래스 호출
Logger > LoggerPrinter > LogAdapter > FormatStrategy
Logger에서는 LoggerPrinter를 사용해서 log message print
LoggerPrinter에서는 List<LogAdapater> 에 log 메시지 전달
LogAdapter에서 FormatStrategy 설정에 따라 log 메시지 print
### Logger 클래스
사용자가 호출하는 인터페이스, 호출하기 쉽도록 static 메소드/변수 사용
### LoggerPrinter
사용자가 호출하는 메소드를 LogAdapter에 전달할 수 있도록 정리
(여러 종류의 LogAdapter 사용 가능)
### LogAdapter
출력 장소에 따라 클래스를 분리 AndroidLogAdapter, DiskLogAdapter
실제 출력은 FormatStrategy 에서 이루어지지만 사용자가 용도에 따라 사용할 있도록 인터페이스 용도로 사용
인터페이스 클래스보다는 추상 클래스로 사용이 더 나은 듯....
#### AndroidLogAdapter
Logcat 에 출력되는 메시지 PrettyFormatStrategy 클래스에 메시지 전달
### FormatStrategy
실제 메시지 출력 담당
### PrettyFormatStrategy
log를 예쁘게 출력
