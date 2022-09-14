
MMORPG 서버구조 (tucker programming - golang)
https://www.youtube.com/watch?v=yk-HD8YoyZg

유저가 많다 - 서버가 감당해야할 일이 많다 - 작업량이 많다
Actor 가 많다 - 매 프레임마다 처리해줘야 한다.
- player
- npc
- control zone

서버
1. 패킷 처리가 필요하다 (read/write)
2. Tick 처리가 필요하다 (heartbeat), tick마다 user의 위치 업데이트
3. 그외 system

서버 - 5000 사용자 감당
FPS tick 10FPS - 1tick 100ms 에 처리 - 1 actor당 0.02ms 안에 업데이트를 처리해야한다.
MMORPG에서는 멀티쓰레드가 필수! - 좋은 구조를 만들어야 한다
MMORPG 는 할일이 많다, 시간이 짧다, 멀티쓰레드가 필요하다, 효율적인 구조가 필요하다

Single thread
Single thread + dedicate thread (특정한 일을 하는 thread) -> Simple!! 감당할 수 있는 Actor수가 줄어든다
단순 Multi Thread - Queue,... locking -> dead lock
Actor간에 interaction이 빈번하게 일어난다
구획화 Multi Thread