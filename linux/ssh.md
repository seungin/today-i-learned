# SSH (Secure Shell)

위키피디아 정의를 보면 아래와 같다.

Secure Shell is a cryptographic network protocol for operating network services securely over an unsecured network. Typical applications include remote command-line, login, and remote command execution, but any network service can be secured with SSH.

- [Linux SSH 사용법](https://jdm.kr/blog/212)

## How to Connect with Remote SSH Server

기본적인 명령어 사용법은 아래와 같다. 원격 서버의 사용자명은 seungin, IP 주소는 192.168.0.2라고 가정한다. 명령을 입력하면 password를 묻게 되고 password를 입력하고 엔터 키를 누르면 접속에 성공한다.

```sh
seungin@Local-PC$ ssh seungin@192.168.0.2

seungin@Remote-PC$

```

## Register Authorized Key

ssh 접속은 신뢰할 수 없는 호스트에 한해서 항상 비밀번호를 묻게 되어 있다. 하지만 쉘스크립트 등으로 일괄 처리를 할 때에는 비밀번호 입력이 불가능하고 이를 위해 스크립트에 비밀번호를 넣어두는 것도 말이 안된다. ssh 서버에서 ssh key를 생성하여 ssh 클라이언트에 제공하면 이러한 불편함을 해소할 수 있다. 보안 상 rsa key를 사용할 것을 권장하며 생성된 private key가 노출되지 않도록 유의해야 한다.

```sh
\# ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
Created directory '/root/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:3NQPGQnZAIA8sO3jvgY02JqhpbYgewxTYC/P8mn6yrY root@9d6a9c8a90a1
The key\'s randomart image is:
+---[RSA 2048]----+
|  .o .....o=..   |
|.. o+     ..oo   |
|.+o ..    . +    |
|o.*o   . o   o   |
|.O+.o   S .   .  |
|Xo.+ .           |
|+=+.o            |
|ooo=.            |
|.E*oo.           |
+----[SHA256]-----+
```

이렇게 key를 생성하면 `.ssh` 디렉터리에 2개의 파일이 생성된다.

- `id_rsa` 비밀키
- `id_rsa.pub` 공개키

이제 신뢰할 수 있는 Client에 공개키를 제공해주면 된다. 클라이언트의 홈 디렉터리 내 `.ssh/authorized_keys` 파일 내부에 공개키를 복사해 넣으면 서버로 접속할 때 비밀번호를 묻지 않게 된다.

authorized_keys 파일 내에는 여러 개의 키를 넣을 수 있는데 아래와 같이 파일 내부에 복사해 넣으면 해당 호스트 모두에 접속 가능하다.

```sh
$ cat ~/.ssh/authorized_keys
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDYiW92JunVCOq7qfto4LChZVrz4/TsZirxja9rWkAQT4Umy0ZNDanJpdDl2oqpjyhKZ3WoQ2Egun0iUvkPNF27eXNqeRiFUeZGc+8H/7OHnMyTIuO9h8ZYL4AqwYD2doDd5zKWkrWk9neVF/2kX4mimoW7b1/KbX+W8lX85GA8NawuMBpQsCqeH3IXtj4BGjL7iVyJWldbHHJ5ICtXHIuXkk6nep5kqEDvzNRXqsg0UTx+Ukqzc88L6/iZnMogBVd0/RwEugLsq1niJ0T56fwqIZDdkyJ6glUy5yF0TqqEeLKKrQhG6N0pKknYMlmNWfrUlEUInr+XZ+X6/BMjXp7x root@9d6a9c8a90a1


```