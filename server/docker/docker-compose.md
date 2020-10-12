# docker-compose 에서 -i 와 -t 옵션
```yml
version: "3"
services:
  app:
    image: app:1.2.3
    stdin_open: true # docker run -i
    tty: true        # docker run -t
```
https://stackoverflow.com/a/39150040