# Running Jenkins in Docker

docker container를 이용하여 jenkins를 실행하는 방법을 설명한다.

## Installing Jenkins

아래 내용은 [Installing Jenkins](https://jenkins.io/doc/book/installing/#setup-wizard)를 참고하여 요약한 것이므로 상세 내용은 링크를 참고한다.

On Windows:

1. bridge network를 하나 만든다. `jenkins`
2. TLS 인증 및 jenkins 데이터 저장을 위한 docker volume을 만든다. `jenkins-docker-certs` `jenkins-data`
3. jenkins node 안에서 docker 명령을 실행하기 위한 container를 만든다. `jenkins-docker`
4. jenkins blueocean container를 실행한다. `jenkins-blueocean`

```sh
$ docker network create jenkins
$ docker volume create jenkins-docker-certs
$ docker volume create jenkins-data
$ docker container run --name jenkins-docker --rm --detach --privileged --network jenkins --network-alias docker --env DOCKER_TLS_CERTDIR=/certs --volume jenkins-docker-certs:/certs/client --volume jenkins-data:/var/jenkins_home docker:dind
$ docker container run --name jenkins-blueocean --rm --detach --network jenkins --env DOCKER_HOST=tcp://docker:2376 --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 --volume jenkins-data:/var/jenkins_home --volume jenkins-docker-certs:/certs/client:ro --publish 8080:8080 --publish 50000:50000 jenkinsci/blueocean
```

완료되면 웹브라우저에서 `localhost:8080` 에 접속한다. 접속한 후 Jenkins 초기 설정을 진행하면 된다. [Post-installation setup wizard](https://jenkins.io/doc/book/installing/#setup-wizard)를 참고한다.
