스트림 에디터라 부르며 지정한 지시에 따라 파일이나 파이프라인 입력을 편집해서 출력해주는 명령어


# 옵션 설명
- n : 패턴을 포함하는 행만 출력한다.
- e : 하나의 지시

# command 설명
- d : 명령줄 삭제

# example
`p`:출력, 
```sh
sed '/north/p' datafile
```
기본적으로 모든 줄을 출력하고, 패턴과 일치하는 줄을 한번 더 출력
```sh
sed -n '/north/p' datafile
```

# ref
[리눅스 명령어 정리](https://jupiny.com/2017/07/10/linux-command-5-sed/)