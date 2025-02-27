# Pintos 설치하기
[핀토스 공식 문서](https://web.stanford.edu/class/cs140/projects/pintos/pintos_1.html#SEC5) 

## 개발환경 설정
맥북에서 핀토스를 설치해보자. 맥북은 Arm아키텍쳐이기 때문에 핀토스를 바로 실행할 수 없으므로 **도커**를 이용한다.
```
brew install --casks docker
```

도커에서 우분투 bionic 버전 이미지를 설치하자
```bash
docker pull --platform linux/amd64 ubuntu:bionic # ubuntu 이미지를 도커로 가져온다.
docker run --platform linux/amd64 -it -d --name pintos ubuntu:bionic # pintos이름으로 컨테이너 생성 후 실행
docker attach pintos # 컨테이너 환경에 접속
```

[Dev Container](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)를 통해 우리에게 익숙한 vscode를 이용해 해당 컨테이너에 접속할 수 있다.
![[vscode-devcontainer.png]]설치후 위와 같이 현재 활성화되어있는 컨테이너가 나오고, 이를 더블클릭해 접속할 수 있다. *(접속시 지원하지 않은 버전이라고 나오나 그냥 무시하고 해도 된다.)*
핀토스는 다음 개발 환경이 필요하다.
1. GCC, GNU binutils
2. Perl
3. GNU make
4. QEMU (QEMU를 사용하기 힘들때 Bochs를 사용해도 된다.)
5. GDB
```bash
apt update && apt upgrade -y
apt install build-essential gcc binutils perl gdb qemu -y # 요구사항 설치
apt install vim wget unzip # 파일 편집, 압축 및 해제를 위해 설치
apt install git # github에서 pintos파일을 불러오기 위함.
```

## 핀토스 설치
위의 개발환경설정이 완료되었으면, 이제 Pintos를 다운받자. *(필자는 root 경로내에 다운받았다.)*
```
git clone https://github.com/WyldeCat/pintos-anon.git pintos
```
`src/utils/pintos` 를 통해 pintos 실행이 가능하다. 하지만 디버깅등을 좀 더 편리하게 하기 위해 추가적인 설정을 했다.
```bash
cd pintos/src/utils
chmod +x Pintos.pm  backtrace  pintos  pintos-gdb  pintos-mkdisk  pintos-set-cmdline # 해당 파일에 실행권한 부여
cp Pintos.pm  backtrace  pintos  pintos-gdb  pintos-mkdisk  pintos-set-cmdline /usr/local/bin # /usr/local/bin으로 복사, 이제 경로 지정 없이 pintos를 실행할 수 있다.
```

## 디버깅 도구 설정

```bash
cd pintos/src/misc
cp gdb-macros /usr/local/bin #gdb-macors를 /usr/local/bin에 복사
```
위에서 `gdb-macros`파일을 `usr/local/bin`에 복사했다. 이제 `pintos-gdb`파일에 해당 `gdb-macros`파일 경로를 수정해줘야한다. (필자는 vim을 이용해 수정했다. [vim](https://velog.io/@pond1029/Linux-Vim)을 모른다면 알아두도록 하자.)
```
cd /usr/local/bin
vim pintos-gdb # vim 편집기로 해당 파일 수정
```
아래 `GDBMACROS`변수를 현재 `gdb-mcros`의 경로로 바꿔주자.
```sh
#! /bin/sh
# Path to GDB macros file. Customize for your site.

GDBMACROS=/usr/local/bin # /usr/local/bin/gdb-macros로 수정

# Choose correct GDB.

if command -v i386-elf-gdb >/dev/null 2>&1; then

GDB=i386-elf-gdb

else

GDB=gdb

fi

  

# Run GDB.

if test -f "$GDBMACROS"; then

exec $GDB -x "$GDBMACROS" "$@"

else

echo "*** $GDBMACROS does not exist ***"

echo "*** Pintos GDB macros will not be available ***"

exec $GDB "$@"

fi
```
파일을 저장하고 `pintos-gdb`를 입력하면 다음과 같은 결과가 나온다. 만약 `pintos-gdb`를 찾을 수 없다는 문구가 나오면 경로를 제대로 설정했는지 확인해보자.
![[pintos-gdb.png]] 
## 핀토스 실행

```bash
cd pintos/src/threads
make # threads 경로내 코드 컴파일
cd build # 빌드 디렉토리로 이동
pintos --qemu -- run alarm-multiple # qemu를 이용해 실행하도록 설정
```
실행후 다음 화면이 나온다.![[result.png]]

> [!NOTE] 고생의 기록
> [공식 문서](https://web.stanford.edu/class/cs140/projects/pintos/)에 나와있는데로 하려했으나... 무한부팅, Pintos 코드 오류등등 수많은 오류로 인해 삽질을 많이했다. 아마 오래된 코드이다보니 호환성 이슈이지 않을까 생각한다. 이 글을 보는 여러분은 이러한 삽질을 하지 않았으면 한다.
