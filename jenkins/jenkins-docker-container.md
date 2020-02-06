# Run Jenkins on Docker Container

docker container 안에서 jenkins를 실행하는 방법을 설명한다.

## Examples

jenkins image를 pull하고 container를 실행한다. 이 때 종료가 되더라도 변경된 내용이 지워지지 않도록 하려면 volume을 설정한다. Windows에서 정상적으로 volume이 연결되려면 `Shared Drives`를 설정해야 한다. 작업표시줄 우측 Docker Desktop 트레이 아이콘에 마우스 우클릭으로 **_settings_**를 선택하고 **_Shared Drvies_** 탭을 선택한다. 원하는 드라이브(아래 예시에서는 C:)를 선택하고 **_Apply_** 버튼을 누른 다음 Login ID/Password를 입력하면 설정이 완료된다.

> jenkins official docker image는 `jenkins/jenkins` 이다. 다른 이미지를 받는 경우 최신 버전이 아니므로 설치가 실패하는 등 문제가 발생할 수 있으므로 주의해야 한다. [Official Jenkins image to use from Docker Hub](https://jenkins.io/blog/2018/12/10/the-official-Docker-image/)를 참고하자.

```sh
$ docker pull jenkins/jenkins
$ docker run --name jenkins -p 8080:8080 -v %USERPROFILE%\var\jenkins_home:/var/jenkins_home jenkins/jenkins
```

컨테이너가 실행되면 웹브라우저에서 `localhost:8080` 에 접속한다. 접속한 후 Jenkins 초기 설정을 진행하면 된다.
