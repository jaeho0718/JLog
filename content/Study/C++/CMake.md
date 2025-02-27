# CMake에 대하여
CMake는 프로젝트 관리를 용이하게한다.

다음 아래 3가지는 필수적으로 있어야한다.
```bash
cmake_minimum_required(VERSION 3.10) # 반드시 해당 명령어로 시작해야한다. Cmake버전 설정
add_executable(Tutorial tutorial.cpp) # 주어진 소스코드로 실행가능 파일을 생성하도록 지시
project(Tutorial) #프로젝트 이름 설정, 모든 프로젝트별로 있어야된다. 반드시 cmake_minimum_required()호출 후 사용 
```

1. `cmake "project dir"`: 프로젝트 설정 및 빌드 시스템 생성
2. `cmake --build .`:  생성된 빌드 시스템 호출 (컴파일 및 링크)

### C++ standard 설정
```bash
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

CMAKE_CXX_STANDARD # CMake에서 사용가능한 변수
CMAKE_CXX_STANDARD_REQUIRED
```