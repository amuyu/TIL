
최종원 (https://github.com/lastone9182)
Data Engineer

# intro
 "학습이 되지 않은 인공지능은 갓난아기나 다름없다."

# MNIST 데이터
인공지능을 시작할 때 주어지는 computer vision data set
(tensorflow 의 경우 미리 제공)

# 어떻게 수집할 건가?
파일, 이미지, 음성 등의 데이터를 API, 웹페이지 등을 통해서 데이터 셋으로 얻는 방법은 다양


# 블로그/게시판 데이터
## How To
### 주의사항
- 민감한 개인 정보
- 부하
### 설계
### selenium headless
phantom.js 와 비슷한거
### 수집을 위한 데이터베이스 사용법
### AWS 로 병렬 수집하기



## 주의사항
크롤링에 정답은 없습니다만...
타이밍이 중요합니다.



## 설계
에러캐치 > 단순하게 > 복잡하게(병렬수집)
### 에러캐치
critical,
unknown,
이 외에는 발견될 때마다 잡아야 한다.
selenium 에서 발견되는 에러 no such exception, timeout
에러는 미리미리
### 단순하게
#### 닭 잡는 데 소 잡는 칼을 쓸 필요는 없습니다.
자바스크립트에서 웹 엘리먼트를 가져오는데,,,
구체적이고 자세할 수록 에러 발생률도 높아지고,, 하니 간단하게
#### page 크롤링
page list를 1 depth 링크만 추출해서 database table 에 넣는다.
### 복잡하게
병렬수집은 돈이 많이 듭니다.
sleep 을 많이 준다.....
병렬수집은 IP 를 얻기 위해서 (아마존 인스턴스 추가)

### key point
데이터베이스에서 url row를 가져온다.
다중 인스턴스에서 데이터베이스에 저장하려면 Hadoop, Queue를 사용하는 등 여러가지 방법이 있다.
여기서는 데이터베이스 성질로 접근한다.
#### Quiz
transaction,,, ACID(atomicity consistency isolation durability)
Isolation 에 집중
데이터 무결성, 데이터를 가져왔는지 가져온 데이터에 중복은 없는지



## 구현
### selenium
웹 테스트 목적 > 웹 자동화 도구
페이지 수집
#### chrome headless
chrome 웹 드라이버 사용
headless 옵션 GUI 환경이 아니어도 실행 가능
가상 모니터(Xvfb)를 만들고 같은 작업을 합니다.
#### Ajax 요청도 가능합니다.

### 병렬수집
#### 데이터베이스 lock
##### Isolation level
- read uncommitted
- read committed
- repeatable read - mysql default
- serializable - 병렬 수집에 사용
##### status row
ok, pending
##### select for update :
트랜잭션 종료 시까지 다른 세션은 SELECT 불가


## example
### Amazon Web Services
20개의 인스턴스와 하나의 데이터베이스
IAM EC2 RDS
### Boto3
클라우드를 한낱 리모컨으로
병렬수집이 깔끔하게 된다.
### 프로세스
1. IAM 을 통해 아마존 서비스를 프로그래밍으로 다루는 권한을 부여
2. Python BOto 2으로 제어
### rc.local
부팅 시 실행되는 스크립트 rc.local, 서비스 systemd
중앙 인스턴스 1개가 나머지 인스턴스를 알아서 제어하는 마법
IP 가 부팅 시마다 바뀌는 건 덤~


## TIP
- 수집하면서 분석하면 안된다. : 데이터 추출은 최소화, 페이지 전체를 저장 후 파싱
- 중복, GroupBy, Join : 가공된 데이터가 아닌데 데이터베이스에서 조인하면 엄청 느리다
  -> Python 에 Pandas 라는 라이브러리가 있다. 쿼리로 데이터 가져온 후, 가공해서 새 테이블로 저장하는 것 추천
- 전처리가 중요합니다. : 데이터를 잘 가져오는 지 꾸준히 확인하세요
- Tesseract OCR : image to string
- 보통 수집할 때? 20s sleep 을 건다..
- 텔레그램 봇, 슬랙 봇
