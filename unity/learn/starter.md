# 유니티 처음 시작하신다고요?
https://www.youtube.com/watch?v=MqB0485r838
URP Sample Project

# interface 설명
## menu
build setting
GameObject 
Component - GameObject에 붙여서 사용한다.

## interface
preset - layout 변경

## Scene view
Transform Tools : 화면 이동, 오브젝트 이동 등 (q,w,e,r,t,y)
option : view?
command + option : move
y축이 위
gizmo : 효과? game object 와 관련된 그래픽

## inspector
선택한 object 의 속성을 볼 수 있음


## Project
image, 3d model(3d max)

## Console
필요한 정보 출력

## hotkey
shift + space : 창 확대

## Game
실제 게임
cmd + p : 실행
play mode 에서는 저장이 되지 않는다.

## view point navigation
마우스 휠버튼을 꾹 누르면 : hand mode
alt / option : view 를 돌려볼수 있다. (eye tool)
마우스 우클릭 : view 를 돌리고 wasd 키로 이동

## 실제 object 이동
새로운 scene을 만들어서 시작
object 추가
기본 제공하는 cube unit 추가 : 1 unit 1meter
move tool : object gizmo 축 별로 이동할 수 있다.
rotate tool : 축 별로 회전
scale tool : 축 별로 확대 축소
inspector - transform - reset 을 선택하면 처음으로 돌아간다.
Rect Tool : UI 만들때 많이 사용한다.

## snap?
move tool 에서 v 키를 누르면 snap 꼭지점으로 이동한다.

## camera 이동
main camera를 선택하고 gizmo? 로 이동?
camera 도 마찬가지로 tool을 통해 이동 
scene 에서 보는 뷰와 game에서 보는 뷰를 같게 하고 싶으면?
align with view : camera를 누르고 ctrl/cmd + shift + f (많이 사용한대요)

## package manager

## asset store
asset 을 살 수 있는 Store
assetStore 에서 검색해서 unity 에서 열기

## script
게임 내에서 작동하는 모든 기능을 script로 작성
script가 hierarchy view에 있어야 사용할 수 있다 == scene에 포함되어 있어야 사용할 수 있다
### MonoBehaviour
* Awake

# 용어
* Scene : 게임의 장면이나 상태를 저장하는 단위
* GameObject 속성
    * Sprite Renderer: 2D
    * Mesh Renderer: 3D
    * Mesh filter : 외형
    * Box Collider : gameobject 의 충돌 범위
    * Audio Source
    * Script
* Asset : 프로젝트 내부에서 사용하는 모든 리소스
* 프리팹(Prefab)
* 좌표 : 왼손 좌표계

