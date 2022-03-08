
# Upgradable deploy
> When you use Upgradeability you need to deploy in a special way. 
> Remix will not do that for you. Find here our resources about upgrades. 
https://forum.openzeppelin.com/t/upgradeable-contracts-on-remix/16524/2


ERC1967Upgrade
ERC1967Proxy

# Minimal Beacon Proxy
https://medium.com/@NipolNIpol/minimal-beacon-proxy-af3cb0c529cf

eip-1822 Universal Upgradeable Proxy Standard, UUPS
`delegatecall` ??

Proxy Contrac 1..N - Logic Contract

Beacon Proxy Contract
모든 프로시 컨트랙트의 업그레이드를 한 번에 수행하기 위해서 비콘 컨트랙트를 사용한다
Proxy Contract - Beacon Contract 
               - Logic Contract

Minimal Proxy Contract
eip-1167 Minimal Proxy Contrac다
gas 비용을 줄일 수 있다

# ref
[Minimal Beacon Proxy](https://medium.com/@NipolNIpol/minimal-beacon-proxy-af3cb0c529cf)
[Upgrades Plugins](https://docs.openzeppelin.com/upgrades-plugins/1.x/hardhat-upgrades)
[Proxies](https://docs.openzeppelin.com/contracts/4.x/api/proxy)