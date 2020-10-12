

# MyBatis Generator - MBG 사용법



## 프로젝트를 통해 살펴본 파일들
- DatabaseConfig
- generatorConfiguration.xml
- mapper clas
- mapper.xml
- dao == javaModelGenerator

## Target Runtime Information
db 로부터 데이터를 읽어와서 역으로 코드를 생성하는 듯?
`targetRuntime` 이라고 부르는 설정 파일을 생성
다음과 같은 형태를 갖고 있으며 프로젝트에서 필요한 쿼리를 생성하기 위한 기본 정보들을 설정한다.
```xml
<!DOCTYPE generatorConfiguration PUBLIC
 "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
 "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
  <context id="dsql" targetRuntime="MyBatis3DynamicSql">
    <jdbcConnection driverClass="org.hsqldb.jdbcDriver"
        connectionURL="jdbc:hsqldb:mem:aname" />

    <javaModelGenerator targetPackage="example.model" targetProject="src/main/java"/>

    <javaClientGenerator targetPackage="example.mapper" targetProject="src/main/java"/>

    <table tableName="FooTable" />
  </context>
</generatorConfiguration>
```

sqlMapGenerator : sql xml 생성
javaModelGenerator : model 생성
javaClientGenerator : mapper 생성


## xml element
generatedKey : insert 할 때, 증가
columnOverride : 컬럼 type 변경, 예를 들어 0,1 => false, true



## Document 는?
[MyBatis Generator - Quick Start Gudie](https://mybatis.org/generator/quickstart.html)
[MyBatis Dynamic SQL Usage Notes](https://mybatis.org/generator/generatedobjects/dynamicSqlV2.html)




