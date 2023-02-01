# CentOS 7에 Oracle19c 설치

# Linux 환경 설정(root 계정)

## Oracle 관리 계정 및 그룹 생성

| 호스트명 | ora19c |
| --- | --- |
| IP | 192.168.56.101 |
| 설치 계정 | ora19c(UID: 1900) |
| 소속 그룹 | dba(GID: 1900) |
| 홈 디렉터리 | /home/ora19c |
| $ORACLE_BASE | /app/ora19c |
| $ORACLE_HOME | /app/ora19c/19c |

```bash
groupadd -g 1900 dba #그룹 생성
useradd -g dba -u 1900 ora19c #유저 생성
passwd ora19c #ora19c의 비밀번호 설정
mkdir -p /app/ora19c/19c #디렉터리 생성
mkdir -p /app/oraInventory #디렉터리 생성
chown -R ora19c.dba /app/ora19c #ora19c(소유자)dba(소유 그룹)로 /app/ora19c 과 하위 파일들의 권한 변경
chown -R ora19c.dba /app/oraInventory #위와 동일
chgrp -R dba /app #dba 그룹으로 /app 과 하위 파일의 사용자 그룹을 변경
chmod -R 775 /app #/app과 하위 파일의 퍼미션을 rwxr-xr-x로 변경

#확인용
ls -al /app

#아래와 같이 나오면 정상

#drwxrwxr-x   4 root   dba    40 ?월  2 ??:??  .
#dr-xr-xr-x  18 root   root  235 ?월  2 ??:??  ..
#drwxrwxr-x   3 ora19c dba    17 ?월  2 ??:??  ora19c
#drwxrwxr-x   2 ora19c dba     6 ?월  2 ??:??  oraInventory
```

## 리눅스 설정

```bash
vi /etc/hosts
#제일 밑 줄에 아래와 같은 형식으로 저장
#ex) 192.168.56.101 JIK.iarkdata.com JIK
```

## 추가 패키지 설치

```bash
yum -y install ksh
yum -y install libaio-devel
yum -y install compat-libcap1
yum -y install compat-libstdc++-33
yum -y install glibc-devel
yum -y install libstdc++-devel
yum -y install gcc-c++
yum install -y https://yum.oracle.com/repo/OracleLinux/OL7/latest/x86_64/getPackage/oracle-database-preinstall-19c-1.0-1.el7.x86_64.rpm
```

## ora19c 계정 설정(ora19c 계정)

- 설치될 오라클의 관리 계정에서 수행

```bash
vi .bash_profile
#제일 밑 줄에 아래의 내용 추가
export ORACLE_OWNER=ora19c
export ORACLE_BASE=/app/ora19c
export ORACLE_HOME=/app/ora19c/19c
export TNS_ADMIN=$ORACLE_HOME/network/admin
export ORACLE_SID=JIK
export NLS_LANG=AMERICAN_AMERICA.AL32UTF8
export ORACLE_HOSTNAME=JIK.iarkdata.com
export TMP=/tmp
export TMPDIR=$TMP
export PATH=$PATH:$ORACLE_HOME/bin:$ORACLE_HOME:/usr/bin:.

#재 로그인 후 설정 확인
env | grep ORACLE

#결과 예시

#ORACLE_OWNER=ora19c
#ORACLE_SID=JIK
#ORACLE_HOSTNAME=JIK.iarkdata.com
#ORACLE_BASE=/app/ora19c
#ORACLE_HOME=/app/ora19c/19c
```

# 설치용 패키지 준비

## 압축 해제

- 오라클 원본 파일은 오라클사 홈페이지에서 $ORACLE_HOME에 다운로드
    - 파일명: LINUX.X64_193000_db_home.zip
- 다운받은 파일을 unzip으로 압축 풀기

## 인스톨러 실행

```bash
cd /app/ora19c/19c

./runInstaller
```

# Universal Installer

- X windows가 없을 시 VM으로
1. 구성 옵션: 단일 인스턴스 데이터베이스 생성 및 구성(C)
2. 시스템 클래스: 데스크톱 클래스(D)
3. 기본 설치
    1. 전역 데이터베이스 이름(G): 호스트 명 (ex)JIK.iarkdata.com)
    2. 비밀번호 입력(ex) oracle)
    3. 컨테이너 데이터베이스로 생성(I) 체크 해제
4. 인벤토리 디렉터리 확인 후 다음
5. 자동으로 구성 스크립트 실행(A) 체크
    1. “루트” 사용자 인증서 사용(C)의 비밀번호 root 계정 비밀번호
6. 설치

# 오라클 실행

- ora19c 계정으로 로그인 후 진행

```bash
sqlplus / as sysdba

#실행 시 SQL> startup
#종료 시 SQL> shutdown immediate
#SQL 나갈 때 exit
```