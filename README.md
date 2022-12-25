# no matching host key type found. Their offer: ssh-rsa,ssh-dss [preauth] 오류 해결 방법

> 아이폰/패드 앱 중 Docker Management (by Nevis Shkenza) 사용 중
> Ubuntu 서버 등록 시 서버 접속이 안되고,
> 서버의 auth.log 파일을 살펴보면 아래와 같이 나온다.

```
sshd[11076]: Unable to negotiate with xx.xx.xx.xx port 49520: 
no matching host key type found. Their offer: ssh-rsa,ssh-dss [preauth]
```

해결 방법을 구글링 하였으나 모두 실패하고, 내가 성공한 방법은 아래와 같다.

> 서버: Ubuntu 22.04.01 LTS (Jammy Jellyfish)
> 클라이언트: 아이폰 앱 Docker Management

## 1. /etc/ssh/sshd_config 파일 수정

PasswordAuthentication no 를 yes로 수정한다.
```
# PasswordAuthentication no
PasswordAuthentication yes
```

## 2. /etc/ssh/sshd_config.d/ssh-rsa.conf 파일 생성

```
   Ciphers aes128-ctr,aes192-ctr,aes256-ctr,aes128-cbc,3des-cbc
   MACs hmac-md5,hmac-sha1,umac-64@openssh.com

   HostkeyAlgorithms ssh-dss,ssh-rsa
   KexAlgorithms +diffie-hellman-group1-sha1
```

## 3. 사용자 계졍(ubuntu) 패스워드 설정

```
$ sudo password ubuntu
New password:
Retype new password:
passwd: password updated successfully
```

## 4. sshd 데몬 재구동

```
$ sudo service sshd restart

$ sudo service sshd status
```

## 5. 아이폰 앱 Docker management 설정

```
Name : 서버 이름
Host : 서버 IP주소 또는 도메인 이름
Port : 22
Username : ubuntu
Password : 패스워드
```
[Test] 버튼 누르면 정상 접속 확인된다.
[Add] 버튼 눌러 서버를 추가한다.
