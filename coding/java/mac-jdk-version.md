
```
jhome () {
  export JAVA_HOME=`/usr/libexec/java_home $@`
  echo "JAVA_HOME:" $JAVA_HOME
  echo "java -version:"
  java -version
}
```

# ref
[맥 환경에서 여러 JDK 버전 설치하고 변경하기](https://advenoh.tistory.com/20)