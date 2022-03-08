
# linux2
```
# docker 설치
sudo yum update -y
sudo amazon-linux-extras install docker
sudo service docker start
sudo usermod -a -G docker ec2-user

# docker-compose 설치
sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compoe
// logout
service docker restart
docker-compose -v
```

# ref
[aws에서 docker 설치](https://docs.aws.amazon.com/ko_kr/AmazonECS/latest/developerguide/docker-basics.html)