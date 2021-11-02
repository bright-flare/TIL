# ✨HTTP2 설정하기

## 🧑🏻‍💻 SSL 설정하기
- HTTP2를 설정하려면 먼저 SSL설정이 되어있어야 한다.
- Servlet Container로 Tomcat을 사용할 경우 8.5 이후 9 버전이상을 사용하는 것을 권장한다. (설정이 매우 어려움) 9 version 부터는 별도의 설정이 필요 없다.

- SSL 인증서 생성하기
```xml
# terminal
$ keytool -genkey
    -alias sseob
    -storetype PKCS12
    -keyalg RSA
    -keysize 2048
    -keystore keystore.p12
    -validity 4000
```

- application.properties에 설정 추가하기.
```xml
server.ssl.key-store=keystore.p12
server.ssl.key-store-type=PKCS12
server.ssl.key-store-password=123123
server.ssl.key-alias=sseob
server.port=8443
```

## SSL 설정후 확인하기.
- SSL 설정후 Connector를 하나더 생성하여 https, http 모두 사용하자.
- 확인하기
```xml
$ curl -I -k --http2 http://localhost:8080/hello
$ curl -I -k --http2 https://localhost:8443/hello
```

## HTTP2 설정

- application.properties에 한줄만 추가하면 간단하게 설정 된다 !
```xml
server.http2.enabled=true
```