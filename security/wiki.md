
# 단방향 해시 함수의 문제
동일한 메시지가 언제나 동일한 다이제스트를 갖는다면 공격자가 전처리된 다이제스트를 가능한 많이 확보한 다음 탈취한 다이제스트와
비교해 원본 메시지를 찾거나 동일한 효과의 메시지를 찾을 수 있다. 이러한 다이제스트 목록을 레인보우 테이블(rainbow table) 이라고 한다.

해시 함수는 빠른 처리 속도로 인해 빠른 속도로 임의의 문자열의 다이제스트와 해킹할 대상의 다이제스트를 비교할 수 있다. 
(MD5 를 상용한 경우 1초당 56억개 비교)

# 단방향 해시 함수 문제 보완
## salting
솔트는 단방향 해시 함수에서 다이제스트를 생성할 때 추가되는 바이트 단위의 임의의 문자열
readfl0wer 에 솔트를 추가해 다이제스트를 생성할 수 있다. 솔트는 32바이트 이상 될 것을 권장
8zff4fgflgfd93fgdl4fgdgf4mlf45p1 + readfl0wer => digest

## key stretching
입력한 패스워드의 다이제스트를 생성하고, 생성된 다이제스트를 입력 값으로 다이제스트를 생성하고 이를 반복하는 방법으로 다이제스트 생성
동일한 횟수만큼 해시해야만 패스워드의 일치여부를 확인할 수 있다.
일반적인 장비로 1초에 50억개 이상의 다이제스트를 비교할 수 있지만, 키 스트레칭을 적용하여 동일한 장비에서 1초에 5번 정도만 비교할 수 있게 한다.
```java
//Java Code
//해당 키를 가지고 Salt 생성 후 SHA512 적용하여, 다이제스트 생성
MessageDigest digest = MessageDigest.getInstance("SHA-512");
byte[] keyBytes = password.getBytes("UTF-8");
byte[] saltBytes = digest.digest(keyBytes);

// in Java (65536번 해싱)
PBEKeySpec pbeKeySpec = new PBEKeySpec(password.toCharArray(), saltBytes, 65536, 256);
```

# 암호화 시스템
## Adaptive Key Derivation Functions
다이제스트를 생성할 때 솔팅과 키 스트레칭을 반복하며 솔트와 패스워드 외에도 입력값을 추가하여 공격자가
쉽게 다이제스트를 유추할 수 없도록 하고 보안의 강도를 선택할 수 있다.

*주요한 key derivation function*
### PBKDF2
솔트를 적용한 후, 해시 함수의 반복 횟수를 임의로 선택할 수 있다.
 NIST(National Institute of Standards and Technology, 미국표준기술연구소) 에 의해서 승인됨
```js
DIGEST = PBKDF2(PRF, Password, Salt, c, DLen)  
```
- PRF : 난수 (HMAC)
- c : 원하는 iteration 반복 수
- DLen : 원하는 다이제스트 길이

### bcrypt
보안에 집착하기로 유명한 OpenBSD 에서 기본 암호 인증 매커니즘으로 사용되고 있고 미래에 PBKDF2 보다 더 경쟁력이 있다고 여겨진다.
bcrypt 에서 `work factor` 인자는 하나의 해시 다이제스트를 생성하는데 얼마만큼의 처리 과정을 수행할지 결정한다.
다만 PBKDF2나 scrypt와는 달리 bcrypt는 입력 값으로 72 bytes character를 사용해야 하는 제약이 있다.
```java
// Sample code for jBCrypt is a Java
// gensalt is work factor and the default is 10
String hashed = BCrypt.hashpw(password, BCrypt.gensalt(11));

// Check that an unencrypted password matches one that has
// previously been hashed
if (BCrypt.checkpw(candidate, hashed))  
    System.out.println("It matches");
else  
    System.out.println("It does not match");
```

### scrypt
다이제스트를 생성할 때 메모리 오버헤드를 갖도록 설계되어, brute-force attack 을 시도할 때 병렬화 처리가 매우 어렵다.
미래에 bcrypt 에 비해 더 경쟁력이 있다고 여겨진다.
```js
DIGEST = scrypt(Password, Salt, N, r, p, DLen)  
```
- N : cpu 비용
- r : 메모리 비용
- p : 병렬화
- DLen : 원하는 다이제스트 길이

====

# AES (대칭키)
블록암호화
AES 알고리즘의 연산은 GF(Galois Field) 중 GF(2^8) 에서 이루어지며, AES 에서 사용하는 기약다항식은 m(x) = x^8+x^4+x^3+x+1 로 정해져 있다.
Add Round Key, Suab Byte, Shift Row, Mix Column 이 반복되어 이루어진다.
key size 에 따라 AES128, AES256....

- S-Box : GF(2^8) 을 이용한 치환연산
- Shift Row : 단순 자리바꿈
- Mix Column : GF(2^8) 을 이용한 치환연산
- Add RoundKey: XOR 연산을 이용

- iv(initialization vector) : ?
- padding : 평문이 블럭 크기의 배수가 되지 않을 경우, 마지막 블록이 데이터로 채워지는가 지정하는 방법
    - pkcs#5 : 암호 블록 사이즈가 8 바이트에 맞춰져 있다, 8 바이트 고정 길이
    - pkcs#7 : 패딩이 최대 16개까지 가능하다. 최대 가능한 패딩은 FF, 1~255 바이트 가변 길이

## paramerter
AES256 일 때,
- secret key length : key 256bit, 
- block size 128 bit ? iv 는 block size 와 같아야 함 (암호화 요청 시 마다 달라지는 것이 보안데 좋음)
- 암호화 모드 : CBC, ECB

random 한 iv 생성
```java
private static IvParameterSpec getRandomIvParameterSpec() {
  byte[] iv = new byte[16];
  new SecureRandom().nextBytes(iv);
  return new IvParameterSpec(iv);
}
```



# ECIES - Elliptic Curve Integrated Encryption Scheme
[Elliptic Curve Cryptography: ECDH and ECDSA](https://andrea.corbellini.name/2015/05/30/elliptic-curve-cryptography-ecdh-and-ecdsa/)



# Ref
[안전한 패스워드 저장](https://d2.naver.com/helloworld/318732)
[AES 암/복호화 알고리즘](https://newstein03.tistory.com/1)
[안전한 암호화를 위한 AES 알고리즘에 대한 이해와 구현코드](https://dailyworker.github.io/AES-Algorithm-and-Chiper-mode/)