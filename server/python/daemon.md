
## damoen.py 사용법
```py
#!/usr/bin/env python
import sys, time
from daemon import Daemon

class MyDaemon(Daemon):
    def run(self):
        while True:
            time.sleep(1)

if __name__ == "__main__":
        daemon = MyDaemon('/tmp/daemon-example.pid')
        if len(sys.argv) == 2:
                if 'start' == sys.argv[1]:
                        daemon.start()
                elif 'stop' == sys.argv[1]:
                        daemon.stop()
                elif 'restart' == sys.argv[1]:
                        daemon.restart()
                else:
                        print "Unknown command"
                        sys.exit(2)
                sys.exit(0)
        else:
                print "usage: %s start|stop|restart" % sys.argv[0]
                sys.exit(2)
```
출처: http://milkteakang.tistory.com/110

# 참고
[python 데몬 예제](http://pydjango.tistory.com/entry/CODE-PYTHON-데몬-예제)
[Python Daemon 생성 예제 코드](http://thdev.net/287)
[python-daemon](https://pypi.python.org/pypi/python-daemon)
[crawling 어플리케이션 만들기](https://www.inflearn.com/course/웹-크롤링web-crawling-어플리케이션-만들기/)
[웹 크롤러 만들기](http://creativeworks.tistory.com/entry/PYTHON-3-Tutorials-24-웹-크롤러like-Google-만들기-1-How-to-build-a-web-crawler)
[python 스케줄러 만들기](http://egloos.zum.com/mcchae/v/11030317)
