CodeChain 
*Codechain Foundry
foundry
? Tendmint기준으로 작성할 때 Tendermint 알고리즘 .. ?
블록체인 같은 경우에는 어떤 노드가 다음 블록을 만드는지 몰라서 dos 공격이 성립되지 않지만 Tendermint 같은 경우 기본적인 순서는 네트워크 들 간에 가늠할수 있어 dos 공격이 가능  할수 있다.
모듈을 조합해서 어플리케이션을 만든다.
이때 목표 ? goal
기존에 사용하고 있는 모듈을 내리지 않고도 업데이트 할수 없는가 . 
다양한 프로그래밍 언어로 작성이 가능하고 호환이 되는가
isolation에 대한 보안 조치 (다양한 모듈이 저
sandbox 
object capability model 
long term goals 
determinism 
metering 
모듈간의 메세지 전달 및 퍼포먼스에 대한 능력의 비례 
model code format requried by sandbox 
sandbox를 다양한 프로그램/ 다양한 언어에 대해 제공할수 있는 형태 제공 
coordinator : modules에 대한 업데이트 , 예전 sendbox를 다운시켜 다이나믹하게 model을 업데이트 할수 있는 기능을 할수 있음 .
-> isolation 환경에서 modules을 생성해 내서 사용한다.?

ICS: 
다른 네트워크에 생성되어있는 블록체인 내에 사용할 수 있는 IBC  

cryptography: two curves :(?)

ed25519 : 핵심은 속도가 빨라야 된다.. (transaction signis)
BLS12-381 : 
둘에 대한 오버랩은 없음. 
(전반적인 아키택처와 구성 확인 - 개발 자체를 퍼블릭하게 진행을 함 . github 참고 )
Q&A 


*Randomized Leader Election in Tendermint (tenderand)
복습 
리더를  RR 방식으로 선택을 하여 진행을 한다. 
VRF foundation - 랜덤을 구현하는데 퍼블릭 키를 가지고 있으면서 프라이빗 키를 가지고 있는것을 확인할수 있는 뭐라고? ㅇ-ㅇ 다시 찾아보기 
sortirion 알고리즘 
apply to tendermint 
propose 과정에서 validation block 
구현 
SVDW method 사용 
ECVRF hash-to-curve :
블록을 계속 생산해야 되고 모든 노드가 같은 블록으로 합의하는 과정이 중요 한데 
다양한 valid proposal 이 있는데 nil ? 
암호학 라이브러리를 다룰때 tip 
결과 
평균적인 블록생성 시간 4.42초
리더가 나오지 않을 확률은 0.103 % : 이유는 가십하는데 있어서 노드들끼리 hight 조율하는 과정이 잘 이뤄지지 않을 경우로 생각한다 (개발자 생각 ) 

*Snapshot Sync(verisync)
-중요하지 않거나 잘못된 데이터를 받아 검증하는 과정에서 발생하는 네트워크 소비를 최소화 하는 방법 중 하나 (?)
복습
Goals : 서비스를 만들어 블록체인 풀 노드를 운영해야 되면 전체 history에 대한 정보를 받을 때 맨처음에 필요없는 데이터를 받지않는 것이 목표 (필요한것들만 받더라도, 전체 status 받고 검증하는것이 아니라 스냅샷을 사용하여 적절한 양을 받아 도 검증이 가능하게끔 한다.)
신뢰 할수 있는 소스에서 받아온다. 
스냅샷을 Bootstrap node 에서 받아온다. 
	branch node 값을 가지지 않고 파일드 노드를 가진 노드 
leaf node 는 branch  node 마지막 
chunk root? Merkle tree ? 
결과 
block sync time : 169 X
체인이 정상 작동하는것에 대한 스냅샷은 2.8MB
이슈
where to store precommits 
한블록을 결정하는 투표를 어디에 담을까에 대한 문제 
어떤블록에 대한 투표를 다음 블록에 기입 -> ㅇ-ㅇ 세미나 참고  (유투브) 
1) validator들의 보상에 대한 문제 
validator 에 대한 일에 보상을 다음 term끝날 시기에 계산에서 줄때 -> 다음 블록에 이전 블록의 정보가 있어야 제대로 된 보상을 줄수 있다. 
해결하기 위해 header에 있는 정보를 보지 않았지만 ?
2) Reading grandparent state to verify precommits ? 
-현재 set에 전체에 대한 정보를 담고 있다.
3) timelock
transfer transaction ?
Timelock issue
Q&A
1) 이더리움의 ??? 싱크랑 다른 점 : chunk자체로도 검증이 가능하다. 일부만 받아 검증이 가능하다.
2) 스냅샷 싱크에서 utxo도 같이 받는다.
 
fork?