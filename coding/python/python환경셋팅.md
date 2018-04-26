# python 개발 환경
파이썬에서는 프로젝트별로 독립된 가상 환경을 만들어주는 virtualenv 툴을 제공한다.
독립된 가상 환경이 필요한 이유는 인터넷에서 다운로드한 파이썬 라이브러리들이 충돌을 일으키는 것을 방지하기 위함이다.

pip : python package index
virtualenv : 수 많은 프로젝트와 프로젝트별 버전관리 패키지들의 의존성 관리를 위해 사용

– pyenv: 로컬에 다양한 파이썬 버전 설치
– virtualenv: 로컬에 다양한 파이썬 환경 구축. 패키지 의존성 해결
– autoenv: 프로젝트 폴더 들어갈때마다 자동으로 개발환경 세팅됨.

# pyenv
```sh
brew update
brew install pyenv

# bash_profile에 추가. 나는 zsh라 ~/.zshrc에 추가하였다.
echo 'eval "$(pyenv init -)"' >> ~/.bash_profile  

# pyenv 사용하기. 현재 설치한 버전들이 다 나온다.
pyenv version

#설치할 수 있는 파이썬 리스트를 보여주고, 거기서 골라서 설치
pyenv install -list
pyenv install 2.7.10
python -version #버전확인
pyenv global 2.7.10 #설치한 파이썬 버전 사용

#env 리스트 보여주기
pyenv virtualenvs
```


# virtualenv
독립된 실행 환경을 구성한다.
구성한 가상환경에 패키지들이 설치되기 때문에 프로젝트 별로 다른 환경을 구성할 수 있다.
프로젝트 별로 사용하는 패키지들의 버전이 다를 수 있다. 그러면 프로젝트가 바뀔 때마다 패키지의 버전을 변경해야 하는 번거로움이 발생한다.

## 사용방법
가상환경을 만들고 싶은 폴더로 가서 다음 명령을 실행한다.

### pyenv 를 사용하지 않을 때,
```sh
virtualenv [env_name]
virtualenv -p python3 [env_name] # 파이썬2가 기본인 환경에서 파이썬3을 기본으로 하는 환경을 만들고 싶은 경우
```
만든 가상환경을 활성화한다.
```sh
source [env_name]/bin/activate
```
설정 후에는 모든 파이썬 관련 환경은 설정한 내용을 기준으로 하기 때문에
다 사용했다면 해제를 해줘야 한다.
```sh
deactivate
```

### pyenv 를 사용할 때
```sh
brew install pyenv-virtualenv

# pyenv init 안했으면 위에것도 bash_profile이나 zshrc에 추가해준다.
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"

# 2.7.10을 사용한 pinkfong-tv라는 프로젝트 만들기, version 은 생략 가능
pyenv virtualenv 2.7.10 pinkfong-tv

# 만든 이름으로 activate하기
pyenv activate pinkfong-tv

# install된 패키지들을 보여준다.
pip freeze

# django 설치하기
pip install django

# pip upgrade
pip install --upgrade pip

# deactivate하기
pyenv deactivate

pyenv uninstall env_name
```

[python 개발환경 셋팅하기](https://milooy.wordpress.com/2015/07/31/python-set-environments/)
[pyenv로 쉽게 python 환경 관리하기](https://cookieshake.github.io/posts/pyenv로-쉽게-python-환경-관리하기)
[python tutorial](https://www.learnpython.org/)
[python style guide](https://www.python.org/dev/peps/pep-0008/)
[virtualenv document](https://virtualenv.pypa.io/en/stable/)
