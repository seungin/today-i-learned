# How to manage the GCC >= 5 ABI

[how-to-manage-the-gcc-5-abi](https://docs.conan.io/en/latest/howtos/manage_gcc_abi.html#how-to-manage-the-gcc-5-abi)

분명히 conan을 이용해 Build Type도 동일하게 Debug로 맞추고 protobufd 라이브러리도 링크되는데 undefined reference 에러가 발생하였다. 이상한 것은 seungin 계정에서는 정상적으로 빌드가 되는데 gitlab-runner 계정에서는 빌드가 실패하였다. 수행된 명령어를 비교하던 중 설치된 protobuf 라이브러리 체크섬이 서로 다른 걸 발견하였고 이는 뭔가 빌드 profile에 차이가 있는 것이었다. 기본 profile을 확인해보니 딱 하나 다른 게 있었는데 그 차이는 아래와 같았다.

`/home/seungin/.conan/profiles/default`

```
[settings]
compiler.libcxx=libstdc++11
```

`/home/gitlab-runner/.conan/profiles/default`

```
[settings]
compiler.libcxx=libstdc++
```

이는 gcc 5 이상 버전에서 변경된 ABI 때문인데 conan에서는 호환성을 위해 default로 old ABI를 사용하도록 되어 있다. CI 빌드가 돌아가는 Linux PC에서는 new ABI를 사용하는데 conan에서 설치한 ABI는 old ABI 이므로 서로 호환되지 않아 undefined reference 링크 에러가 발생한 것이었다.

CI Linux PC는 기본으로 gcc7 이 설치되어 있으므로 gitlab-runner 계정 홈 디렉터리에 있는 default 설정값을 new ABI를 사용하도록 변경해주어 해결하였다.
