

### ssl 생성
```
// 1
openssl req -x509 -out localhost.crt -keyout localhost.key -newkey rsa:2048 -nodes -sha256 -subj "/C=KR/ST=Seoul/L=Gang-nam/O=Home/OU=Dev/CN=local.iskra.world"

// 2
openssl genrsa -des3 -out private.key 2048
openssl req -new -key private.key -out out.csr -subj "/C=KR/ST=Seoul/L=Gang-nam/O=SecureSignKR/OU=Dev Team/CN=local.iskra.world"

```