# Qt Installer Framework Tutorial

여기서는 Qt Installer Framework를 사용하여 설치 파일을 만드는 가장 단순한 예제를 설명한다.  

작성하며 참고했던 링크들을 추가해둔다.  
[Tutorial: Creating an Installer](http://doc.qt.io/qtinstallerframework/ifw-tutorial.html)
[Qt Installer Framework 를 이용한 Installer 만들기](https://www.qtocube.co.kr/qt-forum/bbs/board.php?bo_table=tip&wr_id=37)


## Get the Qt Installer Framework

Qt maintenance tool을 실행하여 Qt Installer Framework를 설치할 수 있다.  
설치가 완료되면 준비는 끝난 것이다.  
다만 설치된 bin 디렉터리를 Path에 추가해두면 편리할 수 있다.  

## Make an installer example

설치 파일을 만드는데 알아야 할 많은 사항이 있지만  
최종적으로는 Installer Framework에서 제공하는 binary 파일을 실행함으로써 쉽게 installer를 만들 수 있다.  
Offline 용으로 만들 때는 `binarycreator.exe`를, Online 용으로 만들 때는 `repogen.exe`를 이용한다.  

자, 이제 Qt Installer Framework 설치 경로에 제공되는 예제 Package Directory를 이용해 테스트 해보자.  
나의 경우는 3.0 버전을 설치했기 때문에 아래 경로에 설치되었다.  

> C:\Qt\Tools\QtInstallerFramework\3.0

하위 폴더 구성은 아래와 같다.  

```sh
bin/
doc/
examples/
README
```

제공되는 가장 기본적인 예제 폴더는 examples/tutorial 이다.  
이 폴더를 이용하여 설치 파일을 만들어볼 것이다.  

```cmd
> cd C:\Qt\Tools\QtInstallerFramework\3.0
> cd examples\tutorial

> ..\..\bin\binarycreator.exe -c config\config.xml -p packages YourInstaller.exe
```

이와 같이 실행하면 현재 폴더에 YourInstaller.exe 라는 이름의 설치 파일이 생성됨을 확인할 수 있다.  
한번 실행하여 설치를 진행해보면 Qt Maintenance Tool과 유사한 인터페이스를 가짐을 알 수 있다.  
