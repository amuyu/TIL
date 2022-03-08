
토큰 표준이 굉장히 많구나..


# ERC165 (kip13)
어떤 인터페이스로 구현되어 있어라고 알려주는 표준
https://docs.openzeppelin.com/contracts/4.x/api/utils#ERC165Checker
특정 컨트랙트가 ERC165의 인터페이스를 구현하고 있는지, ERC165를 구현하고 있다면 추가로 어떤 다른 인터페이스를 구현하고 있는지 확인할 수 있습니다.


* ERC20Snapshot : 스냅샷을 찍어서 찍은 시점의 balances, totalsupply에 접근할 수 있습니다. voting, 에어드랍에 응용할 수 있습니다. balanceOfAt(address account, uint256 snapshotId) snapshot을 찍으면, 해당하는 Id를 얻을 수 있습니다. 이를 통해 특정 시점의 balance를 확인할 수 있습니다.
* ERC20Pausable : transfer, mint, burn 세 개의 함수에 대해 pause 할 수 있도록 장치를 추가하는 ERC20입니다. openzeppelin의 security 항목에서 제공하는 Pausable 라이브러리를 사용하면 해당 함수 이외에 다른 함수도 pause 하도록 만들 수 있습니다.
* ERC20Burnable : ERC20을 burn 할 수 있게 됩니다.
* ERC20Capped : Maximum totalSupply를 설정할 수 있어서 그 이상으로 mint 되지 않도록 할 수 있습니다.



# ref
[ERC20, 165, 721 살펴보기](https://tech.elysia.land/erc20-165-721-%EC%82%B4%ED%8E%B4%EB%B3%B4%EA%B8%B0-d5ebdb67bdd5)