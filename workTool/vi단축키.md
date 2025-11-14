# 학습
vimtutor 사용

# 단축키
- w : 다음 단어 이동
- W : 화이트 스페이스 단위로 다음 글자 이동
- e : 단어의 마지막 글자로 이동하기
- b : 이전 단어 이동
- B : 백워드 방향으로 화이트스페이스 단위로 이동
- % : { } 열고 닫고 이동
- ][ : } 로 이동 forward
- [] : } 로 이동 backward
- $ : 문장 맨 끝으로 이동
- ^ : 문장 맨 처음으로 이동
- gg : 문서 맨 처음 이동
- G : 문서 맨 아래 이동

- shift + h : 현재 보이는 페이지를 기준으로 맨 위로 커서 이동
- shift - m : 현재 보이는 페이지를 기준으로 중간 라인으로 커서 이동
- shift + l : 현재 보이는 페이지를 기준으로 맨 아래로 커서 이동

- ctrl + f : 다음 페이지 이동
- ctrl + d : 다음 반 페이지 이동
- ctrl + b : 이전 페이지 이동
- ctrl + u : 이전 반 페이지 이동


- i : 현재 커서 위치에 insert 
- I : 현재 줄 앞에 insert
- a : 현재 커서 다음칸에 insert
- A : 현재 줄 맨 뒤에 insert
- O : 윗줄에 insert 하기
- o : 아랫줄에 insert 하기

- shift + j : 다음 줄을 현재 커서가 있는 줄의 끝으로 이어 붙이기

- ctrl + r : Redo

- vs 파일명 : 새로운 파일을 열때
- ^ww : 창간 이동


## 편집
- :%s/old/new/g : 문서 전체에서 문자열 치환 

## 비주얼 모드

### 여러 줄 에 텍스트 입력
1. `ctrl + v` 해서 비주얼 블록 모드
2. `shift + i` 누르고 텍스트 입력
https://vim.fandom.com/wiki/Inserting_text_in_multiple_lines
3. 입력 완료