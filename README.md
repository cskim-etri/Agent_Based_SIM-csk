# Agent_Based_SIM 개념

* Agent based Simulator
  * 교통 시뮬레이션 코어 엔진 라이브러리와 교통 시뮬레이션 코어 기능을 외부 응용 프로그램 (i.e. 파이썬 기반 응용 프로그램) 에서 호출 사용 가능한 API를 제공함.


# Agent_Based_SIM 컴파일 방법

## (참고) 정적라이브러리 생성 환경
* Ubuntu 20.04.3 LTS (cuda 11.4.2 버전 설치) 환경에서 컴파일 생성
  
## 사전 환경
* cmake 3.14.0 이상 설치 필요
  * centos 7 기준
    * $ yum install cmake3
    * $ alternatives --install /usr/bin/cmake cmake /usr/bin/cmake3 10 --slave /usr/bin/ctest ctest /usr/bin/ctest3 --slave /usr/bin/cpack cpack /usr/bin/cpack --slave /usr/bin/ccmake ccmake /usr/bin/ccmake3 --family cmake

  * 소스 다운로드 및 설치
    * Goto https://cmake.org/download/ and download a tarball (e.g. cmake-3.18.2.tar.gz)
      * $ wget https://github.com/Kitware/CMake/releases/download/v3.18.2/cmake-3.18.2.tar.gz
    * $ tar xvfz    cmake-3.18.2.tar.gz
    * $ cd cmake-3.18.2
    * $ ./bootstrap
    * $ make -j16            # 병렬 컴파일 (16 쓰레드)
    * $ sudo make install   # 필요 시 설치
    * 설치를 생략한 경우, PATH에 cmake 실행 파일 경로 추가
      * $ export PATH="$PATH:$HOME/cmake-3.18.2/bin"   # 예

  * 필요시 OpenSSL Development Package 설치
    * $ sudo apt install libssl-dev   # Ubuntu 20.04

* boost 설치 필요
  * centos 7 기준
    * $ yum install boost boost-devel boost-build boost-static

  * ubuntu (최신 버전의 boost 설치 방법, salt 컴파일 시 버전이 1.79 이상이면 에러 발생)
    * $ sudo add-apt-repository ppa:boost-latest/ppa
    * $ sudo apt-get update
    * $ sudo apt-get install libboost-all-dev

  * 소스 다운로드 및 설치 : ubuntu & centos (salt는 boost 1.78 이하 버전 필요)(현재 라이브러리는 boost 1.74 기준으로 빌드됨) 
    * $ wget -O boost_1_74_0.tar.bz2 https://boostorg.jfrog.io/artifactory/main/release/1.74.0/source/boost_1_74_0.tar.bz2
    * $ tar xvfj boost_1_74_0.tar.bz2
    * $ cd boost_1_74_0
    * $ ./bootstrap.sh --prefix=/usr
    * $ ./b2 -j 16 cxxflags=-fPIC cflags=-fPIC link=static -a
      * -j N 옵션: 병렬 빌드 
    * $ sudo ./b2 install
    
* swig 3.0 이상 설치 필요
  * ubuntu 기준
    * $ sudo apt install swig

* Python3 설치 필요
  * ubuntu 기준 (현재 라이브러리는 3.8 기준으로 빌드됨) 
    * $ sudo apt install python3
    * $ sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 1
    
* Python3 관련 도구 및 라이브러리 설치 필요
  * ubuntu 기준
    * $ sudo apt-get install python3-setuptools python3-dev

## CMakeLists.txt 수정
* $ vi src/CMakeLists.txt
* 튜토리얼: https://www.tuwlab.com/ece/27234
* 컴파일할 소스파일, 링크할 헤더파일, 타겟 묶어주기 등

## 소스 컴파일
### cmake 수행 -> Makefile 생성
* $ cd src; cmake CMakeLists.txt
#### 또는
* $ mkdir build; cd build; cmake ../src

### compile sources -> make binaries
* $ make libsalt  # cmake 버전이 낮은 경우, make _libsalt 수행 (underscore('_') 문자 필요)
#### 또는
* $ make all
#### 또는
* $ make

### cmake & compile source 한 번에 수행 (권장)
* $ ./c.sh 수행

# Agent_Based_SIM 실행
## 파이썬 스크립트 작성 및 수행
### 시뮬레이션 수행용 디폴트 스크립트 사용 (권장)
* $ cd script
* 파일 편집
  * default.sh 파일: PYTHONPATH를 로컬 환경에 맞게 수정하고, -s 옵션 뒤에 시나리오 파일 위치를 로컬 환경에 맞게 수정
* 시험 수행
  * $ sh default.sh
  * 시험 수행 전, 필요 시, 시나리오 파일 내 시뮬레이션 입력 데이터 파일 확인 (위치, 파일존재 여부 등)

