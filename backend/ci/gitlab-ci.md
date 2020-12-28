
# stages
여러개의 stage 를 등록하고 순차적으로 실행할 수 있다.




# ssh key 사용하기
ssh pk 등록
https://docs.gitlab.com/ee/ci/ssh_keys/

host key verification failed error 
https://stackoverflow.com/questions/13363553/git-error-host-key-verification-failed-when-connecting-to-remote-repository


# runner ?
docker command 설치
https://docs.gitlab.com/runner/install/docker.html
```
docker run --detach \
--name gitlab-runner \
--restart always \
--volume /srv/gitlab-runner/config:/etc/gitlab-runner: \
--volume /var/run/docker.sock:/var/run/docker.sock \
gitlab/gitlab-runner:latestdocker run --detach \
--name gitlab-runner \
--restart always \
--volume /srv/gitlab-runner/config:/etc/gitlab-runner: \
--volume /var/run/docker.sock:/var/run/docker.sock \
gitlab/gitlab-runner:latest
```
docker-compose 
```
gitlab-runner:
 container_name: gitlab-runner
 image: 'gitlab/gitlab-runner:latest'
 restart: always
 volumes:
  - '/srv/gitlab-runner/config:/etc/gitlab-runner'
  - '/var/run/docker.sock:/var/run/docker.sock'
```

# gitlab runner 연동
```
gitlab-runner register -n \
--url http://<IP> \
--registration-token EBLy_mzmz98M9rfN9e1W \
--description gitlab-runner \
--executor docker \
--docker-image docker:latest \
--docker-volumes /var/run/docker.sock:/var/run/docker.sock
```

# gitlab-runner local 
## install
https://docs.gitlab.com/runner/install/osx.html
```sh
# install
brew install gitlab-runner
# run
brew services start gitlab-runner
```

# ref
[gitlab runner 를 사용하여 gitlab ci 구성하기](https://hihellloitland.tistory.com/65)