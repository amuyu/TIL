# tps 측정?
①   TPS = (Active User) / (Average Response Time) – F1 
②   TPS = (Concurrent User) / (Request Interval) – F2 
③   Active User = TPS * (Average Response Time) – F3 
④   Active User = (Concurrent User) * (Average Response Time) / (Request Interval) – F4 
⑤   Active User = (Concurrent User) * (Average Response Time) / [ (Average Response Time) + (Average Think Time) ] – F5 

## example
100개의 Agent를 만들고 응답을 받은 예시로 
1번째 Agent 응답시간 time:0.064000 
100번쨰 Agent 응답시간 time:0.873000 
total time: 46.594996 
Average time 0.465950 
running time :0.954 

8개의 working Thread가 돌아가서 8개씩 처리를 하고 있어서 9번째 응답시간부터 점점 체크시간이 늘어나는 현상?

100(Agent수)/0.465950(Average time)=214.6 이 맞나요?

1번 Agent는 처음 1번 부하를 주고, 0.89초 동안 Think Time(Running Time - Response Time)
0.954 - 0.064 = 0.89

100번 Agent의 경우 약 0.081초 동안 기다리다가(Think Time) 부하
0.954 - 0.873 = 0.081

(1번 Think Time + 100번 Think Time) / 2개 = 평균 Think Time이라 하면 약 0.4855라는 값이 나옵니다. 

Request Interval = (Average Response Time) + (Average Think Time)
0.465950 + 0.4855 = 0.954

TPS = 100 (Concurrent User) / 0.954 (Request Interval) = 104.82 

## 기본 공식
TPS = Transaction / Seconds = 100건 요청 / 0.954 = 104.82

40ms , 25 건 * 10 250 tps?
170ms , 5 건 * 10 50 tps?


# 참고
[ngrinder qna](http://ngrinder.642.n7.nabble.com/TPS-td1676.html)