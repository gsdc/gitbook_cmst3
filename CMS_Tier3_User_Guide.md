# KISTI-GSDC CMS Tier-3 사용자 가이드

*메뉴얼 페이지를 방문하여주신 연구자 여러분을 환영합니다.*

이 문서는 사용자들이 KISTI CMS Tier-3를 보다 쉽게 사용할 수 있도록 돕기 위해 작성되었습니다. 혹시 Tier-3 사용 중에 어려움을 겪으시면 언제든 [gsdc-support@kisti.re.kr](mailto:gsdc-support@kisti.re.kr?subject=\[CMS])로 시작하는 제목으로  연락 주시기 바랍니다.

**처음 사용하시는 분은 1장부터 순서대로 따라오시면 됩니다.** 오른쪽 '이 문서의 목차'에서 원하는 장으로 바로 이동할 수 있습니다.

| 단계 | 장 |
| --- | --- |
| 시작하기 | 1. CMS 서버 접속 |
| 시작하기 | 2. 초기 환경 설정 |
| 환경 이해 | 3. CMS Tier-3 컴퓨팅 환경 |
| 배치 작업 | 4. 배치 작업 사용하기 (HTCondor 기초) |
| 배치 작업 | 5. HTCondor를 이용한 배치 작업 수행 |
| 배치 작업 | 6. create-batch를 이용한 배치 작업 수행 |
| 데이터 | 7. 데이터 전송 및 데이터셋 요청 |
| 데이터 | 8. 스토리지 접근 방법 |
| 그리드/분석 | 9. CRAB3 사용 관련 |
| 그리드/분석 | 10. 빅데이터 분석 환경 구축하기 |
| 문제 해결 | 11. FAQ 및 문제 해결 |

---

## 1. CMS 서버 접속

*이 페이지에서는 KISTI CMS Tier-3에 접속 하는 방법에 대해서 설명을 드립니다.*

### 접속 정보

KISTI CMS Tier-3 사용자는 아래 UI(User Interface) 서버로 접속합니다.

| 항목 | 내용 |
| --- | --- |
| 호스트 이름 | cms-ui.sdfarm.kr |
| SSH 포트 | 48217 |
| 운영체제 | AlmaLinux 9 |
| 계정 | 포털에서 발급받은 사용자 ID |

> **⚠️ 주의**
>
> 기존 UI 서버인 ui20은 곧 폐기될 예정입니다. 앞으로는 cms-ui.sdfarm.kr 로 접속해 주시기 바랍니다. 이 문서의 일부 실행 예시에 남아 있는 `[geonmo@ui20 ~]$`, `[geonmo@ui10 ~]$` 같은 프롬프트는 예전 서버에서 실행한 예시이며, 명령어 사용법은 cms-ui.sdfarm.kr 에서도 같습니다.

또한, 내부에서 접속 가능한 UI 서버인 cms-gpu01가 있습니다. 해당 서버는 NVidia P100이 설치되어 있어 기계학습을 이용한 프로그램을 사용하실 때에 권장됩니다. (cms-ui.sdfarm.kr 에 먼저 접속한 뒤 접속하세요.)

### 리눅스 혹은 macOS 환경에서의 접속 방법

리눅스 혹은 macOS에서는 운영체제에 내장된 ssh 클라이언트 프로그램을 이용하여 UI 서버에 접속하실 수 있습니다.

```bash
ssh -p 48217 [사용자 ID]@cms-ui.sdfarm.kr
```

> **💡 참고**
>
> ssh 명령어를 이용하여 접속을 하실 때, X-Windows 그래픽 환경을 사용하시려면 옵션으로 -X 혹은 -Y 를 추가하시기 바랍니다. 접속 후 $DISPLAY 환경변수 값을 확인하여 X11 Forwarding이 정상적으로 이루어졌는지 확인이 가능합니다.

### 윈도우에서 접속 방법

윈도우는 OS 차원에서 ssh 프로그램을 제공하고 있지 않습니다. 윈도우 개발자 모드를 사용하여 bash 를 띄우지 않는 한 별도의 프로그램을 이용하여 접속을 하시기 바랍니다. 가장 유명한 프로그램으로 putty와 MobaXterm 등 다양한 프로그램이 있습니다. 여기서는 MobaXterm을 기준으로 설명드립니다.

#### MobaXterm

##### 배경

예전부터 윈도우 환경에서 ssh 터미널 환경을 사용하기 위해서 putty 프로그램을 주로 사용하였습니다. 하지만, putty 프로그램은 X-Windows 프로그램이 포함되어 있지 않기 때문에 X11 Forwarding을 사용하려면 X-ming 등의 별도의 X11 지원 프로그램을 설치하여야 했습니다. 이는 사용자가 프로그램을 별도로 설치하고 추가적인 설정 작업이 필요하기 때문에 현재는 초보자들에게 추천되지 않습니다.

##### 라이선스

MobaXterm 프로그램은 사용자 본인이 직접 홈페이지에서 파일을 다운로드 받아 설치할 경우 어디서나 무료입니다. 이는 내장된 X-Windows Server도 포함됩니다. 따라서, 학교 및 회사 등에서도 안심하고 다운로드 받으셔서 설치하실 수 있습니다.

단, 재배포(받은 파일을 공유해주는 경우)는 라이선스 위반으로 간주하고 있으니 반드시 홈페이지에서 파일을 다운로드 받도록 안내하시기 바랍니다.

##### 접속방법

설치한 MobaXterm을 실행한 후 \[Session] - \[SSH] 버튼을 클릭하시고 아래 정보를 기입하시면 접속을 할 수 있는 세션을 생성하실 수 있습니다. 암호를 입력한 이후에는 암호 입력 없이 접속이 가능합니다.

* Remote host : cms-ui.sdfarm.kr
* Specify username : 사용자 ID
* Port : 48217

![그림1. \[세션\]을 클릭합니다.](/wiz/api/portal.gsdcadmin.manual.image/download?community=CMS&path=access-mobaxterm-session.png)

![그림2. \[SSH\]를 클릭합니다.](/wiz/api/portal.gsdcadmin.manual.image/download?community=CMS&path=access-mobaxterm-ssh.png)

> **💡 참고**
>
> 윈도우 환경의 경우 개별 클라이언트 프로그램에서 X11 Forwarding 기능을 활성화 혹은 비활성화를 할 수 있습니다. 해당 기능이 비활성화된 경우에는 -X 혹은 -Y 옵션 없이 접속한 것처럼 X-Windows 그래픽 환경을 사용하실 수 없습니다.

##### 홈페이지 정보

[https://mobaxterm.mobatek.net/](https://mobaxterm.mobatek.net/)

---

## 2. 초기 환경 설정

*여기서는 접속 후 초기 설정에 관한 내용입니다. 리눅스 환경을 처음 써보시는 경우 아래의 환경설정을 따라하시는 것을 추천드립니다. 필수적인 것은 아니며 본인의 취향대로 설정하셔도 무방합니다.*

### OTP 토큰 등록

#### OTP 등록 절차 안내

첫 접속을 하시면 바로 OTP 등록 작업을 진행해주시기 바랍니다. OTP가 등록되지 않은 계정은 그 다음 영업일에 계정이 비활성화될 수 있습니다.

토큰 발급은 아래와 같이 진행합니다.

```bash
## 언어 혹은 인코딩 설정이 C 로 되어 있는 경우에는 정상적으로 OTP QR 코드를 확인할 수 없습니다. 
## 따라서, 이를 해제해야 합니다. 비트워든 같은 프로그램은 QR코드가 아니라 URI로 등록이 가능하므로
## 이를 이용하실 때에는 무시하셔도 됩니다.
export LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8
## 토큰을 발급합니다.
ipa otptoken-add
```

### 그리드 인증서 설치

#### 인증서 발급과 VO CMS 가입

인증서는 CERN CA 홈페이지([https://ca.cern.ch/ca/](https://ca.cern.ch/ca/))에서 \[New Grid User certificate] 항목을 이용하여 발급 받을 수 있습니다. 인증서는 발급 받은 직후에는 아무런 권한이 부여되어 있지 않습니다.  발급 후 VO CMS ([https://lcgvoms24.cern.ch:8443/voms/cms/user/home.action](https://lcgvoms24.cern.ch:8443/voms/cms/user/home.action)) 를 방문하여 CMS 자원 활용을 허가 받을 수 있습니다.

사용자는 VO CMS에 승인된 그리드 사용자 인증서를 이용하여 CMS 자원들에 접근할 수 있습니다. 예외적으로 KISTI CMS Tier-3 사용자 분들은 KISTI CMS Tier-3에 저장된 데이터에 접근을 할 때 해당 인증 없이 사용이 가능하도록 설정되어 있습니다. 하지만, 외부에 존재하는 데이터에 접근하기 위해서는 반드시 사용자 인증서가 있어야 합니다. 또한, 사용자가 그리드 작업을 제출하려면  본인의  인증서를 제출하려는 서버에 설치하여야 합니다.

#### 인증서 매핑 작업

KISTI에서도 인증서를 발급하고 있습니다. ([http://ca.gridcenter.or.kr/](http://ca.gridcenter.or.kr/)) 해당 인증서를 발급 받고 CERN 인증서와 동일하게 사용하시려면 인증서 매핑 작업을 수행하여야 합니다.  인증서를 CERN 컴퓨팅 계정과 동기화해야하기 때문에 일반적인 경우에는 필요가 없습니다. 하지만, CERN 이외의 프로젝트를 같이 수행할 경우 KISTI 인증서가 편리할 수도 있습니다. (타 프로젝트에서 인증서 매핑을 지원하지 않을 경우)

CERN 인증서는 CERN 컴퓨팅 계정과 자동으로 매핑되기 때문에 별도의 설정이 필요 없으나 KISTI에서 인증서를 발급 받으시면 반드시 CERN CA 홈페이지에 접속하셔서 매핑 작업을 하셔야 합니다.

![KISTI 인증서는 CERN CA 홈페이지에 계정과 매칭을 시킬 수 있습니다.](/wiz/api/portal.gsdcadmin.manual.image/download?community=CMS&path=setup-cern-ca-mapping.png)

CERN CA나 KISTI CA를 통해 발급 받은 인증서를 브라우저의 인증서 관리를 통해 .pfx 파일로 추출할 수 있습니다. 이를 CMS Tier-3 UI 서버로 업로드하실 수 있습니다.

#### 인증서 복사

##### 리눅스 혹은 MacOS 사용자

scp 명령어를 통해 파일을 UI 서버로 전송하실 수 있습니다.

```bash
## ssh 명령어와 달리 대문자 P로 포트 번호를 지정합니다.
scp -P 48217 [인증서 파일] [사용자 ID]@cms-ui.sdfarm.kr:~/
```

##### 윈도우 사용자

윈도우 사용자들은 MobaXterm의 \[Session] - \[SFTP] 를 이용하여 파일을 전송하실 수 있습니다.

![MobaXterm의 \[SFTP\] 세션 화면. 드래그를 통해 파일을 전송할 수 있습니다.](/wiz/api/portal.gsdcadmin.manual.image/download?community=CMS&path=setup-mobaxterm-sftp.png)

##### 인증서 변환 및 설정

복사된 인증서 .pfx  혹은 .p12 파일을 이용하여 그리드 작업 프로그램에서 사용하는 PEM 형식으로 파일을 변환합니다.

```bash
mkdir $HOME/.globus
chmod 755 $HOME/.globus
openssl pkcs12 -in myCert.p12 -clcerts -nokeys -out $HOME/.globus/usercert.pem
openssl pkcs12 -in myCert.p12 -nocerts -out $HOME/.globus/userkey.pem
chmod 644 $HOME/.globus/usercert.pem
chmod 400 $HOME/.globus/userkey.pem
```

### (향후 사용예정) OIDC를 이용한 scitoken 등록

oidc-agent는 사용방법이 기존 SSH 인증서를 등록하는 ssh-agent와 사용방법이 유사합니다. 사용방법을 잊어버렸을 경우를에대비하여   ssh-agent 사용방법과 유사하게 사용을 할 수 있도록 구성을 하시는 것을 추천드립니다.

#### 1. oidc-agent 설정 및 oidc-gen으로 신규 scitoken 등록 및 사용

```bash
eval $(oidc-agent)
oidc-gen -w device [token_name]

# Issuer [https://iam-test.indigo-datacloud.eu/]: 
# Issuer 로 https://cms-auth.web.cern.ch/ 입력
https://cms-auth.web.cern.ch/

# Scopes로 max 입력. 향후 scope 정보를 잘 알게되면 조정할 것
max
```

#### 2. https://cms-auth.web.cern.ch/device 접속 후 코드 입력

```bash
Registering Client ...
Warning: The registered client does not have all the requested scopes. The following are missing: storage.create:/store/temp/sitetest storage.modify:/store/temp/sitetest
To update the client to have all the requested scope values, please contact the provider.
Generating account configuration ...
accepted

Using a browser on any device, visit:
https://cms-auth.web.cern.ch/device

And enter the code: ?????
Alternatively you can use the following QR code to visit the above listed URL.
```

#### 3. scitoken의 비밀번호 입력

```bash
Enter encryption password for account configuration 'geonmo_scitoken': 
Confirm encryption Password: 
Everything setup correctly!
```

#### 4. 토큰 사용을 위한 oidc 토큰 등록 및 임시토큰 발행

```bash
oidc-add [Token Name]

export BEARER_TOKEN=$(oidc-token --scope=storage.read:/ --scope=compute.read --time=3600 [Token Name])
```

#### 5. 토큰을 이용한 테스트

```bash
## XRootD
XrdSecPROTOCOL="ztn,unix";export XrdSecPROTOCOL

xrdfs cms-xrdr.sdfarm.kr ls /xrd
xrdfs cms-t2-se01.sdfarm.kr:1095 ls /

## WebDAVs
davix-ls  --capath /etc/grid-security/certificates -H "Authorization: Bearer ${BEARER_TOKEN}" davs://cms-t2-se01.sdfarm.kr:2880//store/user/geonmo

```

### CMSSW 환경 설정

CMS 연구 프레임워크인 CMSSW는 CERN에서 제공하는 CVMFS(CERN Virtual Machine File System)에 의해 제공됩니다. 따라서, 아래 명령어를 실행하여 CMSSW 사용 환경을 구축할 수 있습니다.

```bash
## bash 사용자
source /cvmfs/cms.cern.ch/cmsset_default.sh
## tcsh 사용자
source /cvmfs/cms.cern.ch/cmsset_default.csh
```

환경변수들이 올바르게 적용되면 scram, cmsenv 등 CMSSW 명령어들을 사용하실 수 있습니다. 아래와 같은 방법으로 CMSSW 버전을 확인할 수 있습니다.

```bash
## cms-ui.sdfarm.kr (AlmaLinux 9)
export SCRAM_ARCH=el9_amd64_gcc12
scram list CMSSW
```

UI 서버는 AlmaLinux 9(EL9)이므로 EL9용 SCRAM_ARCH(el9_amd64_gccXX)를 사용합니다. SLC6/CentOS7(slc6_*, slc7_*) 용 구버전 CMSSW가 필요한 경우에는 CVMFS에서 제공하는 컨테이너 명령어(cmssw-el6, cmssw-el7)로 해당 OS 환경에 들어간 뒤 작업하시기 바랍니다.

```bash
## 예) CentOS7 환경이 필요한 구버전 CMSSW 사용
cmssw-el7
export SCRAM_ARCH=slc7_amd64_gcc700
scram list CMSSW
```

> **💡 참고**
>
> CMSSW 소프트웨어의 버전은 연구별로 상이하므로 개별적으로 확인 바랍니다. SCRAM\_ARCH 변수를 통해 설치 가능한 CMSSW의 버전이 변동되므로 참고 바랍니다. 자세한 버전 설명은 CMSSW GitHub 홈페이지([http://cms-sw.github.io/showIB.html](http://cms-sw.github.io/showIB.html))를 참고하시거나 Release Map([https://cmssdt.cern.ch/SDT/releases.map](https://cmssdt.cern.ch/SDT/releases.map))을 참고하시기 바랍니다. (참고 : type=Production;state=Announced;prodarch=1)

### CMSSW 작업 디렉토리 생성

CMSSW 버전을 확인한 후에 다음 명령어로 CMSSW 작업 디렉토리를 생성할 수 있습니다.

```bash
## SCRAM_ARCH를 별도로 지정하는 경우,
export SCRAM_ARCH=slcX_amd64_gccXXX
## 작업 디렉토리 생성
cmsrel CMSSW_10_4_0
### 디렉토리 이름을 지정할 경우,
scram p -n workspace CMSSW CMSSW_10_4_0
```

작업 디렉토리가 생성되면 해당 디렉토리를 이용하여 환경변수를 설정할 수 있습니다. 이를 실행하여야 CRAB이 사용가능합니다.

```bash
cd CMSSW_10_4_0
cd src
cmsenv
```

### CRAB 환경 설정

CRAB은 CMS 연구자들이 사용하는 그리드 작업 제출 프로그램입니다.

CRAB을 사용하려면 CRAB과 관련한 환경변수들을 적용하여야 합니다. CVMFS를 통해 해당 환경변수들을 설정하실 수 있습니다.

```bash
### 올바른 CMSSW 작업 디렉토리에서 cmsenv를 수행한 후에,
### For bash users,
source /cvmfs/cms.cern.ch/crab3/crab.sh
```

그리드 작업을 제출하기 앞서 반드시 본인이 T2\_KR\_KISTI와 T3\_KR\_KISTI에 쓰기 권한이 있는지를 확인하셔야 합니다.

다음 명령어로 이를 확인하실 수 있습니다.

```bash
## To T2_KR_KISTI
crab checkwrite --site=T2_KR_KISTI

## To T3_KR_KISTI
crab checkwrite --site=T3_KR_KISTI
```

> Checkwrite Result:\
> Success: Able to write in /store/user/geonmo on site T2\_KR\_KISTI

만약, 쓰기가 불가능하시면  [cmst3-support@kisti.re.kr](mailto:cmst3-support@kisti.re.kr) 로 이메일을 보내주시기 바랍니다.

### Rucio 환경 설정

Rucio는 ATLAS실험에서 데이터셋 관리를 위해 작성된 데이터 관리 프로그램입니다. Rucio를 사용하려면 아래 설정이 필요합니다.

```bash
## No cmsenv environment
source /cvmfs/cms.cern.ch/cmsset_default.sh
source /cvmfs/cms.cern.ch/rucio/setup.sh
export RUCIO_USER=$(whoami)

## or T3_KR_KISTI에서는 위 명령어를 다음 예약어를 설정하여 사용 중입니다.
### rucioenv는 cmsenv 설정 후에는 작동하지 않습니다. 
### 별개의 세션(신규 접속)에서 실행하는 것을 추천드립니다.
rucioenv
```

### /xrootd\_users 디렉토리 생성

KISTI CMS Tier-3에서 제공해드리는 장기 보관용 저장소인 /xrootd 공간은 OS에서의 직접 접속을 금지하고 대신 xrdfs, gfal-copy 등의 전송 프로그램을 쓰도록 안내드리고 있습니다. 하지만, 사용자의 편의성을 증대하기 위해 신청자를 대상으로 /xrootd\_users 디렉토리에 본인의 저장소 공간을 UI 서버들에 마운트를 해드리고 있습니다. (WN에는 마운트해드리지 않습니다.)

신청을 하실 때에는 CERN 계정과 KISTI 계정 정보를 알려주셔야 합니다. CERN 컴퓨팅 계정을 잘 모르실 경우 아래의 CRAB 명령어를 통해 확인하실 수 있습니다.

```bash
crab checkusername
```

> Retrieving DN from proxy... \
> DN is: /C=KR/O=KISTI/O=KISTI/CN=58079576 Geonmo Ryu Retrieving username for this DN... \
> username is geonmo

위 명령어로 확인한 CERN 계정과 KISTI 계정을 관리자에게 보내주시면 /xrootd\_users/\<userid> 디렉토리 밑에 사용자 저장소를 마운트해드립니다.

> **💡 참고**
>
> 디렉토리가 생성된 이후에는 일반 디렉토리를 사용하시듯이 cp, rm 명령어로 파일을 관리하시면 됩니다. 단, mv 명령어는 자제해주시기 바랍니다.

해당 디렉토리는 사용자가 접근하면 자동으로 마운트되도록 autofs로 설정되어 있습니다. 하지만, 마운트가 안된 상태에서 여러 차례 접근을 시도하면 마운트 충돌로 인해 아래의 메시지가 표시되면서 사용이 불가능해집니다. 아래의 메시지가 나타나면 즉시 관리자에게 연락을 주시기 바랍니다.

> 전송 종료지점이 연결되어 있지 않습니다 / Transport endpoint is not connected

### GitHub 사용을 위한 공개키 등

CMS 연구에 사용되는 많은 코드들은 현재 GitHub에 등록되어 있습니다. 해당 코드를 UI 서버로 다운로드 받으실 때에는 저장소에서 제공되는 HTTPS 명령어를 사용하시면 편리하게 이용하실 수 있습니다. 하지만, 해당 저장소의 코드를 수정한 후 다시 업로드를 원하신다면 GitHub에 공개키를 등록하시기를 권장드립니다.

SSH 공개키를 만들기 위해서는 다음과 같은 작업이 필요합니다.

```bash
[geonmo@ui10 .ssh]$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/share/geonmo/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /share/geonmo/.ssh/id_rsa.
Your public key has been saved in /share/geonmo/.ssh/id_rsa.pub.
The key fingerprint is:
1c:17:f5:77:f0:77:06:f6:8e:3e:32:49:24:2c:e9:cf geonmo@ui10.sdfarm.kr
The key's randomart image is:
+--[ RSA 2048]----+
|          ... +  |
|         o . o = |
|        + + . . O|
|       o + o   =+|
|        S   . . .|
|         o . o   |
|          E + o  |
|             o . |
|                 |
+-----------------+

```

> **⚠️ 주의**
>
> 암호를 입력하지 않은 공개 및 비밀키를 만드시려면 Enter passphrase 항목에서 내용 없이 엔터를 입력하시면 됩니다. 해당 ssh 키 중 비밀키(id\_rsa)는 절대 유출이 되면 안됩니다. 암호 없이 만들어지기 때문에 외부 유출이 되면 해킹의 위험이 크므로 관리를 철저히 해주시기 바랍니다. (패스워드 유무와 상관없이 비밀키는 유출되면 안됩니다.)

> **💡 참고**
>
> 만약 암호를 설정하셨다면 ssh-agent를 이용하여 한번만 암호를 입력하시고 해당 세션 정보로 다른 서버로 접속하는 등의 기능을 사용하실 수 있습니다. 해당 내용은 여기서 다루지 않습니다.

생성된 공개키를 GitHub에 등록하시려면 해당 공개키의 정보를 확인하셔야 합니다. 간단하게 cat 명령어를 이용하여 내용을 확인하실 수 있습니다.

```bash
[geonmo@ui10 .ssh]$ cat id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAx3hsrp0tvlSt/Icp2hGQxeRmlIejJTugj/v4DnoztKRQIIiRgWKHDpxKji1eR7A9YusN51G6bZL5RZzJSfgbWiVBfmAKNfiQjHMte80vQlWdtTVXXff05k+aslqUWUTnuquhRnbS0hngamFrISW5/cQMDWUXEi0E4+UKcweG4pWsAYOaFgshj9dQ63JtOMOmbamhXNMxwhtFViQMSHVrEikHXsVIQx6bt64CYjQoGkkyXyqb5AdOtUtwDgRp/GIJSTia78NugdlGY/SdGmYlFR00LpNOqVoJ1tDRS6TniCmx26gTiUUYVDZ84WyNtvwFDaSphdBDDn3rXlgEuf9iBQ== geonmo@ui10.sdfarm.kr
```

GitHub에 계정 가입 후 로그인을 한 후 \[프로필 사진] - \[계정 설정]을 클릭합니다.

![\[계정 설정\] 항목 위치 ](/wiz/api/portal.gsdcadmin.manual.image/download?community=CMS&path=setup-github-account-settings.png)

\[Settings] - \[SSH and GPG keys] - \[New SSH Key] 항목을 선택한 후 위에서 확인한 키 정보를 입력해줍니다.

![SSH and GPG Keys 정보 ](/wiz/api/portal.gsdcadmin.manual.image/download?community=CMS&path=setup-github-ssh-gpg-keys.png)

등록이 완료되면 해당 저장소의 권한 설정에 따라 내용 수정 (commit)후 업로드(git push)가 가능해집니다. 물론, 저장소에 대한 권한이 없다면 불가능하오니 참고 바랍니다.

### 참고문헌

1. CERN CA 홈페이지([https://ca.cern.ch/ca/Help/?kbid=024010](https://ca.cern.ch/ca/Help/?kbid=024010))
2. VO CMS 가입 홈페이지([https://lcgvoms24.cern.ch:8443/voms/cms/user/home.action](https://lcgvoms24.cern.ch:8443/voms/cms/user/home.action))
3. CMS 인증서 발급 안내 TWiki page ([https://twiki.cern.ch/twiki/bin/view/CMSPublic/SWGuideLcgAccess](https://twiki.cern.ch/twiki/bin/view/CMSPublic/SWGuideLcgAccess))
4. CMS 워크북 TWiki Page\
   ([https://twiki.cern.ch/twiki/bin/view/CMSPublic/WorkBook](https://twiki.cern.ch/twiki/bin/view/CMSPublic/WorkBook))

---

## 3. CMS Tier-3 컴퓨팅 환경

*이 페이지에서는 KISTI-GSDC CMS Tier-3의 컴퓨팅 환경 및 구성에 대하여 알려드립니다.*

### 컴퓨팅 자원 소개

![KISTI CMS Tier-3 구조도](/wiz/api/portal.gsdcadmin.manual.image/download?community=CMS&path=cms-tier3-architecture.png)

#### 계산 자원

CMS Tier-3는 ALICE Tier-3인 KiAF 클러스터, 유전체사용자와클러스터 통합 환경으로 구성되어 있습니다. CMS Tier-3의 최저보장 논리CPU코어의 쿼터는  총 2,288개입니다. (물리코어는 절반인 1,144개)

KIAF의 1696, BIO의 1160개와 합쳐 총 5,144개의 작업 슬롯을 가지고 있습니다.

CPU 코어들은 HTCondor 작업 관리자를 이용하여 자원을 배치하거나 회수합니다. 따라서, 작업을 제출하기 위해서는 HTCondor 사용방법을 알아야 합니다.

또한, 다른 그룹 사용자의 작업이 없을 경우에는 2,288개를 넘게 사용할 수 있습니다. 하지만, KIAF 사용자가 작업을 제출할 경우 KIAF 몫인 1696개의 코어를 자동으로 반납하며 반납된 작업 슬롯에서 실행되던 작업은 취소되어 대기열에 들어갑니다.

자세한 사용방법은 **5. HTCondor를 이용한 배치 작업 수행** 장 참고해주시기 바랍니다.

#### GPU 자원

KISTI-GSDC CMS Tier-3에는 머신러닝, GPGPU 기능 제공을 위해 GPU 서버를 1대 구비하고 있습니다.  이를 사용하시려면 해당 UI 서버로 직접 접속하셔서 사용을 권장드립니다.

해당 서버로 접속하기 위해 아래 명령어를 사용하십시오.

```bash
## 외부에서는 접근이 안됩니다. (ui 서버에서 접속하세요.)
ssh [CMS GPU Server]
```

##### 1. /cms (사용자 홈디렉토리 및 스크래치 공간)

/cms 공간은 사용자들의 홈디렉토리 및 임시 작업물 보관을 위한 스크래치 용으로 제공됩니다. 총 저장공간은 20TB로 NAS 형식으로 마운트되어 있습니다. 홈디렉토리와 스크래치 공간은 서로 같은 공간을 공유하여 사용합니다.

스크래치를 너무 많이 사용하면 홈디렉토리 사용에도 지장을 주기 때문에 결과물의 크기를 미리 예측하여 문제가 발생하지 않도록 주의하여 사용하시기 바랍니다.

> **❗ 중요**
>
> /cms 디렉토리는 모든 사용자가 2.5TB만 사용하실 수 있습니다. 추가 용량이 필요하시다면 아래 설명된 /cms\_scratch를 활용하시거나 스토리지로 바로 전송하도록 구성하여야 합니다.

##### 2. /cms\_scratch (배치 스크래치 공간)

공간 부족에 의하 추가로 10TB를 할당받은 공간입니다.

> **❗ 중요**
>
> 해당 공간은 UI 및 WN에서 접근이 가능하지만 **파일 생성 후 7일**이 지나면 모든 파일이 삭제됩니다.

##### 3./xrootd (장기 보관 스토리지)

CMS Tier-3는  KCMS 연구자들에게 독점적인 1004TB 자원을 제공하고 있습니다. 이 중, 604TB는 SAN 스토리지이며, 400TB는 NAS 스토리지를 통하여 제공되고 있습니다.

스토리지로 제공되는 서버는 총 5대이며 각각 10Gbps NIC로 연결되어 있습니다. 따라서,이론적인  최대 대역폭은 50Gbps입니다.

각 스토리지 서버는 XRootD 프로그램을 통하여 통합되어 있습니다. 따라서, xrootd 명령어를 통해 파일을 복사하거나 삭제할 때 성능이 가장 좋습니다.

GSIFTP 기능도 제공하기 때문에 PhEDEx 및 gfal util을 이용한 SRM 프로토콜 사용도 가능합니다. 하지만, xrootd와 gsiftp를 사용할 때 URL이 다르므로 주의하여야 합니다.

| Protocol  |                    GSIFTP                    |              XROOT for Public             |             XROOT for Internal             |
| --------- | :------------------------------------------: | :---------------------------------------: | :----------------------------------------: |
| Prefix    |                   gsiftp://                  |                  root://                  |                   root://                  |
| Server    |               cms-se.sdfarm.kr               |             cms-xrdr.sdfarm.kr            |             cms-xrdr.private.lo            |
| Directory |                //xrootd/store                |                //xrd/store                |                 //xrd/store                |
| Port      |                     2811                     |                    1094                   |                    2094                    |
| Client    |        gfal util (gfal-copy, gfal-ls)        |                xrdfs, xrdcp               |                xrdfs, xrdcp                |
| Full URL  | gsiftp://cms-se.sdfarm.kr:2811//xrootd/store | root://cms-xrdr.sdfarm.kr:1094//xrd/store | root://cms-xrdr.private.lo:2094//xrd/store |

---

## 4. 배치 작업 사용하기 (HTCondor 기초)

*테스트를 위한 작업이 아니라 대용량 데이터를 처리하기 위한 배치 작업의 사용 방법을 배웁니다.*

### HTCondor에 관하여

HTCondor는 우리 CMS Tier-3에서 제공하는 배치 시스템입니다. 해당 프로그램을 이용하여 작업 제출, 작업 결과 확인, 장비 상태 확인 등의 작업을 처리할 수 있습니다.

#### HTCondor 사용시 알아둬야하는 부분

* /xrootd 및 /xrootd\_user 디렉토리는 WN에서 접근이 제한되므로 xrootd 프로토콜로 직접 접속을 하시기 바랍니다.
* KISTI 내부에서는 root://cms-xrdr.private.lo:2094 를 접두어로 사용하면 됩니다.
* 외부에서 KISTI 저장소에 접근을 하려면 root://cms-xrdr.sdfarm.kr:1094 를 사용하여야 합니다.
  * 이 경로는 사용자 프록시 인증서가 꼭 필요합니다.  (voms-proxy-init 명령어 참고)
  * 실행 스크립트에서 인증서를 /tmp로 보내거나 작업디렉토리($\_CONDOR\_SCRATCH\_DIR)를 인증서 보관 디렉토리로 지정하여야 합니다.

### HTCondor 명령어 익혀보기

#### condor\_status

condor\_status 명령어는 머신들의 상태를 확인하는 명령어입니다. 일반적으로 옵션 없이 사용되는 명령어를 자주 사용합니다.

```
[geonmo@ui20 geonmo]$ condor_status                                                                                                                         
Name                             OpSys      Arch   State     Activity LoadAv Mem     ActvtyTime                                                             
                                                                                                                                                            
slot1@cms-gpu01.sdfarm.kr        LINUX      X86_64 Unclaimed Idle      0.000 386684  2+21:24:51                                                             
slot1@cms-t3-wn3001.sdfarm.kr    LINUX      X86_64 Unclaimed Idle      0.000 193349  0+16:29:10                                                             
slot2@cms-t3-wn3001.sdfarm.kr    LINUX      X86_64 Unclaimed Idle      0.000  87842  2+18:49:40                                                             
slot2_1@cms-t3-wn3001.sdfarm.kr  LINUX      X86_64 Claimed   Busy      0.010   2944  0+14:22:49                                                             
slot2_9@cms-t3-wn3001.sdfarm.kr  LINUX      X86_64 Claimed   Busy      1.000   2944  2+19:09:24                                                             
slot2_10@cms-t3-wn3001.sdfarm.kr LINUX      X86_64 Claimed   Busy      1.000   2944  2+19:06:09       
```

> **💡 참고**
>
> 작업이 실행되는 작업 슬롯은 요청된 작업의 특성에 따라 유동적으로 변하게 됩니다. KISTI CMS Tier-3에서는 각 머신당 3개의 유동 슬롯을 준비하였으며 각각의 슬롯은 2:1:1로 CPU 자원을 할당하였습니다.

> **❗ 중요**
>
> CMS 작업은 아무런 옵션이 없이 제출될 경우 \
> CPU 1개, 메모리 2933MB를 사용하도록 설정됩니다. 이 자원 요구량기보다 더 크게 요청할 경우 자원 확보가 용이하지 않아 작업이 실행되지 않을 수 있으니 참고 바랍니다.

### condor\_q

condor\_q는 작업에 대한 정보를 확인할 수 있습니다. 제출된 작업의 자세한 정보를 위해서는 -l( -long) 옵션을 사용하여 확인이 가능합니다.

```bash
[geonmo@ui20 geonmo]$ condor_q

-- Schedd: ui10.sdfarm.kr : <134.75.124.121:9618?... @ 05/21/20 10:59:50
OWNER  BATCH_NAME                                                   SUBMITTED   DONE   RUN    IDLE   HOLD  TOTAL JOB_IDS
jhchoi RequestTest_VBFHToWWToLNuQQ_M350__2016__10000               5/16 15:46      _      1      _      _      1 3671768.0
jhchoi RequestTest_VBFHToWWToLNuQQ_M450__2016__10000               5/16 15:46      _      1      _      _      1 3671777.0
jhchoi RequestTest_VBFHToWWToLNuQQ_M175__2016__10000               5/16 15:46      _      1      _      _      1 3671780.0

[geonmo@ui20 geonmo]$ condor_q -l 3671768.0 | grep Request
RequestCpus = 1
RequestDisk = DiskUsage
RequestMemory = 2930
Requirements = ((HasSingularity == true)) && (TARGET.Arch == "X86_64") && (TARGET.OpSys == "LINUX") && (TARGET.Disk >= RequestDisk) && (TARGET.Memory >= RequestMemory) && ((TARGET.FileSystemDomain == MY.FileSystemDomain) || (TARGET.HasFileTransfer))

```

condor\_q  -better-analyze 명령어를 통해 작업이 요구 조건 확인할 수 있습니다. 작업이 정지 상태라면 정지 원인도 확인 가능합니다.

```bash
[geonmo@ui20 geonmo]$ condor_q -better-analyze 3671768.0 

-- Schedd: ui10.sdfarm.kr : <134.75.124.121:9618?...
The Requirements expression for job 3671768.000 is

    ((HasSingularity == true)) && (TARGET.Arch == "X86_64") && (TARGET.OpSys == "LINUX") && (TARGET.Disk >= RequestDisk) &&
    (TARGET.Memory >= RequestMemory) && ((TARGET.FileSystemDomain == MY.FileSystemDomain) || (TARGET.HasFileTransfer))

Job 3671768.000 defines the following attributes:

    DiskUsage = 2
    FileSystemDomain = "sdfarm.kr"
    RequestDisk = DiskUsage
    RequestMemory = 2930

The Requirements expression for job 3671768.000 reduces to these conditions:

         Slots
Step    Matched  Condition
-----  --------  ---------
[0]         211  HasSingularity == true
[1]         211  TARGET.Arch == "X86_64"
[3]         211  TARGET.OpSys == "LINUX"
[5]         211  TARGET.Disk >= RequestDisk
[7]         211  TARGET.Memory >= RequestMemory
[9]         211  TARGET.FileSystemDomain == MY.FileSystemDomain

3671768.000:  Job is running.

Last successful match: Wed May 20 21:41:45 2020

3671768.000:  Run analysis summary ignoring user priority.  Of 88 machines,
      0 are rejected by your job's requirements
      0 reject your job because of their own requirements
      0 match and are already running your jobs
      0 match but are serving other users
     88 are able to run your job

-- Schedd: ui20.sdfarm.kr : <134.75.124.127:9618?...

```

### condor\_history

종료된 작업에 관한 정보를 확인하기 위한 명령입니다. 사용방법은 condor\_q 명령어와 동일합니다.

### condor\_submit

준비된 작업 제출 명세 파일(Job Description Submit File, .sub 혹은 .jds)을 이용하여 작업을 제출합니다. 기본적인 작업 제출 명세파일의 양식은 다음과 같습니다.

**📄 template.jds**
```bash
#### 작업목록에서 확인할이름을 지정합니다. 
JobBatchName = condorTestJobs 
#### executable은 실행할 프로그램을 입력합니다. 
# 바이너리 실행 파일을 입력하기도 하지만, 
# 일반적으로 실행파일의 환경설정을 위해 Bash 스크립트 파일을 넣습니다.
executable = test.sh
#### universe는 일반적인 경우는 vanilla를 설정합니다.
# 컨테이너를 실행하기 위한 container 유니버스, docker를 위한 docker 유니버스 등도 존재합니다.
# 각각의 유니버스들은 해당 프로그램 실행을 위한 환경설정을 지원합니다.
universe   = vanilla
#### arguments는 프로그램의 인자를 입력합니다.
# 여기서 사용된 $(Process)는 제출된 작업의 Process Id입니다.
# eg) test.sh 13
# FYI) JobId = $(Cluster).$(Process),  
arguments  = $(Process)

### 사용자의 환경변수를 WN에 전달하기 위한 옵션
# 하지만, 100% 전달되는 것은 아니기 때문에 확인이 꼭 필요.
getenv     = True

### 파일의 송수신 기능 활성화
# 이 기능이 꺼졌다면 executable 파일을 WN로 전송하지 않습니다.
# 또한, 실행 결과 파일도 가져오지 않습니다. 
# 작업 결과 파일을 가져오지 않고 공유 디렉토리에서 실행할 경우에는 NO로 설정합니다.
should_transfer_files = YES
### 위 키워드와 같이 사용합니다.
when_to_transfer_output = ON_EXIT

### 요구조건 설정
# 여기서는 Hostname과 동일한 Machine을 사용하도록 설정하였습니다.
# 특별한 조건이 필요한 머신만 특정지어 사용할 때 사용됩니다.
requirements = (Machine =?= "$(Hostname)")

### CMS에서만 사용되는 별도의 태그들 (선택사항)
# Tag는 프로그램의 이름, JobType은 MC, Analysis 중 설정합니다.
+Tag = "condor_check v1.22"
+JobType = "Analysis"

### 실행시 표준 출력 및 에러 저장 파일
output = job_$(Hostname).out
error = job_$(Hostname).err
### 작업 제출 때의 로그, submit 머신의 로그라고 볼 수 있습니다.
log = job.log

### 송신할 입력 파일 및 수신할 결과 파일 이름 지정
transfer_input_files = input_sandbox.tar.gz 
transfer_output_files = result.root
### 결과파일의 이름이 작업마다 겹칠 경우 다른 이름으로 저장하도록 지정하여야 합니다.
# 여기서는 result.root 파일들을 Hostname 변수를 추가하여 저장합니다.
transfer_output_remaps = "result.root = result_$(Hostname).root"

### 자원 요구량 설정
#request_Cpus=1
#request_GPUs =0 
#request_memory=2933
#request_disk = 1

### 이메일 알람 설정
#notification = Error
#notify_user = cmst3-support@kisti.re.kr

### Group Account 정보

#queue 13

queue 1 Hostname from test.txt
```

JDS 파일을 이용하여 작업을 제출할 때는 아래 명령어로 실행합니다.

```bash
condor_submit template.jds
```

현재 KISTI GSDC Tier-3에는 condor\_submit 관련하여 일반적인 환경과 다른 사항이 있습니다. 통합팜 클러스터 활용을 위해 CMS 사용자들은 accounting\_group="group\_cms" 옵션이 반드시 포함된 JDS를 작성하여야 합니다. 기본 bash 환경에서는 아래와 같이 예약어로 설정된 condor\_submit 명령어를 사용하기 때문에 위 내용을 생략할 수 있지만, bash 스크립트 등에서 제출할 경우에는 JDS에 해당 내용을 꼭 추가해주시기 바랍니다.

```bash
alias condor_submit='condor_submit -append accounting_group="group_cms"'
```

#### 작업 실행 OS 지정 (container universe)

UI 서버는 AlmaLinux 9(EL9) 환경입니다. 작업이 UI에서 테스트한 것과 같은 OS 환경에서 실행되도록, JDS에 container universe와 OS 이미지를 지정하여 제출하는 것을 권장합니다. EL9용 CMSSW(el9\_amd64\_\*)는 osgvo-el9 이미지를 사용합니다.

예전 UI(CentOS7)에서 사용하던 condor\_submit6, condor\_submit7 명령어는 더 이상 사용하지 않습니다. SLC6/CentOS7용 구버전 CMSSW(slc6\_\*, slc7\_\*)로 작업해야 하는 경우에는 이미지만 osgvo-el6 / osgvo-el7 로 바꾸어 제출합니다.

```bash
universe        = container
## 필요한 OS에 맞게 이미지를 선택합니다. (osgvo-el6 / osgvo-el7 / osgvo-el8 / osgvo-el9)
container_image = /cvmfs/singularity.opensciencegrid.org/opensciencegrid/osgvo-el7:latest
+SingularityBind = "/cvmfs,/etc/condor,/cms,/cms_scratch,/var/lib/condor,/run/user"
accounting_group = group_cms
```

전체 예시는 **5. HTCondor를 이용한 배치 작업 수행** 장의 submit.jds를 참고하시기 바랍니다. 같은 방식으로 이미지를 바꾸면 ubuntu 등 다른 OS 환경으로도 작업 제출이 가능합니다.

---

## 5. HTCondor를 이용한 배치 작업 수행

*이 페이지에서는 HTCondor를 이용하여 작업을 제출하고 관리하는 방법을 알려드립니다.*

### CMSSW 작업환경 구축

이 예제에서는 ZZ 샘플을 이용하여 분석 작업을 실행할 예정입니다. 해당 데이터 샘플을 살펴보고 어떠한 작업 환경을 구축해야할지 확인을 해야 합니다.

#### 데이터 파일 확인하기

ZZ샘플 데이터 파일 한개를 사용하여 데이터셋 정보를 확인합니다. 해당 파일의 위치는 다음과 같습니다.

> /xrootd/store/mc/RunIIFall17NanoAODv4/ZZ\_TuneCP5\_13TeV-pythia8/NANOAODSIM/PU2017\_12Apr2018\_Nano14Dec2018\_102X\_mc2017\_realistic\_v6-v1/30000/E13A1753-58AD-2E4A-9D47-4BA07BB3FD64.root

CMS에서 사용되는 LFN(Logical File Name) 주소는 반드시 "**/store**"로 시작됩니다. 위에서는 /xrootd만 빼면 됩니다. 해당 LFN은 어느 사이트에서도 동일합니다.

위 파일의 DataSet 및 Release 정보를 확인해봅시다.

```bash
### 임의의 CMSSW 작업 공간에서 cmsenv를 수행한 후,
dasgoclient --query "dataset file=/store/mc/RunIIFall17NanoAODv4/ZZ_TuneCP5_13TeV-pythia8/NANOAODSIM/PU2017_12Apr2018_Nano14Dec2018_102X_mc2017_realistic_v6-v1/30000/E13A1753-58AD-2E4A-9D47-4BA07BB3FD64.root"
```

> /ZZ\_TuneCP5\_13TeV-pythia8/RunIIFall17NanoAODv4-PU2017\_12Apr2018\_Nano14Dec2018\_102X\_mc2017\_realistic\_v6-v1/NANOAODSIM

```bash
 dasgoclient --query "release file=/store/mc/RunIIFall17NanoAODv4/ZZ_TuneCP5_13TeV-pythia8/NANOAODSIM/PU2017_12Apr2018_Nano14Dec2018_102X_mc2017_realistic_v6-v1/30000/E13A1753-58AD-2E4A-9D47-4BA07BB3FD64.root"
```

> \["CMSSW\_10\_2\_9"]

해당 파일의 데이터셋과 CMSSW 버전을 확인하였습니다. CMSSW의 버전이 10\_2\_9 이므로 작업 공간은 이 버전과 비슷하거나 살짝 높아야 합니다. 여기서는 10\_2\_9 버전으로 작업을 진행하겠습니다.

```bash
cmsrel CMSSW_10_2_9
```

> Waiting for release information to be obtained via [https://cmssdt.cern.ch/SDT/releases.map?release=CMSSW\_10\_2\_9\&architecture=slc7\_amd64\_gcc700\&scram=V2\_2\_9\_pre04\&releasetop=/cvmfs/cms.cern.ch/slc7\_amd64\_gcc700/cms/cmssw/CMSSW\_10\_2\_9](https://cmssdt.cern.ch/SDT/releases.map?release=CMSSW_10_2_9\&architecture=slc7_amd64_gcc700\&scram=V2_2_9_pre04\&releasetop=/cvmfs/cms.cern.ch/slc7_amd64_gcc700/cms/cmssw/CMSSW_10_2_9) (timeout in 7s) WARNING: Developer's area is created for non-production architecture slc7\_amd64\_gcc700. **Production architecture for this release is slc6\_amd64\_gcc700.**

위 메시지를 참고하여 사용 중인 UI 서버(cms-ui.sdfarm.kr, AlmaLinux 9)에서 SLC6 컨테이너 환경으로 들어갑니다. 또한, SCRAM\_ARCH=slc6\_amd64\_gcc700 환경변수를 이용하여 작업공간을 생성하고 src 디렉토리로 이동합니다.

```bash
cmssw-el6
SCRAM_ARCH=slc6_amd64_gcc700 cmsrel CMSSW_10_2_9
cd CMSSW_10_2_9
cd src
cmsenv
```

### KISTIBatch 실습 프로그램 다운로드

HTCondor를 이용한 실습을 수행하기 위하여 간단한 CMSSW 분석 작업 예제를 다운로드합니다.

```bash
git clone https://github.com/geonmo/KISTIBatch.git
```

> Cloning into 'KISTIBatch'... \
> remote: Enumerating objects: 72, done. \
> remote: Counting objects: 100% (72/72), done. \
> remote: Compressing objects: 100% (58/58), done. \
> remote: Total 87 (delta 39), reused 37 (delta 14), pack-reused 15 Receiving objects: 100% (87/87), 21.25 KiB | 109.00 KiB/s, done. Resolving deltas: 100% (41/41), done. \
> \
> (base) \[geonmo@ui20 src]$ ls KISTIBatch/ \
> LFNTool.py README.md filedialog.py filelist.txt getZMass.py gui.py run.sh setup.sh submit.jds test\_LFNTool.py

각 파일들의 세부 설명은 GitHub 페이지를 참고해주시기 바랍니다.

filstlist.txt 파일은 dasgoclient를 이용하여 생성할 수 있습니다. 다음 명령어를 참고해주시기 바랍니다.

```
## 파일 리스트 추출
dasgoclient --query="file dataset=/ZZ_TuneCP5_13TeV-pythia8/RunIIFall17NanoAODv4-PU2017_12Apr2018_Nano14Dec2018_102X_mc2017_realistic_v6-v1/NANOAODSIM" --limit 0 > filelist.txt
## Prefix 추가. 데이터 파일의 위치를 알고 있다면 prefix에 해당 센터의 xrootd 경로를 넣으시면 됩니다.
sed -i 's/^/root:\/\/cmsxrootd.fnal.gov\//g' filelist.txt
### T3_KR_KISTI의 경우,
## sed -i 's/^/root:\/\/cms-xrdr.private.lo:2094\/\/xrd/g' filelist.txt
```

#### 실습 코드에 관한 설명

해당 실습 코드는 getZMass.py 라는 분석 코드를 각 WN 머신에서 수행하게 됩니다. 이 때, 해당 분석코드는 첫번째 인자로 데이터 파일을 입력 받습니다.

```
./getZMass.py root://cms-xrdr.private.lo:2094//xrd/store/mc/RunIIFall17NanoAODv4/ZZ_TuneCP5_13TeV-pythia8/NANOAODSIM/PU2017_12Apr2018_Nano14Dec2018_102X_mc2017_realistic_v6-v1/30000/8DC8188D-246C-6D41-A24A-78C0CBBC6F17.root
```

> (base) \[geonmo@ui10 KISTIBatch]$ ls -l \*.root \
> -rw-r--r--. 1 geonmo geonmo 3780 Sep 2 14:05 zcandmass.root

#### HTCondor 작업 제출 명세 작성

HTCondor에 작업을 동시(concurrent)에 실행시키려면 HTCondor 문법에 맞게 작업 제출 파일을 작성하여야 합니다. 해당 제출 명세의 예제로 submit.jds 파일을 동봉하였습니다.

> batch\_name = kisti batch test \
> executable = run.sh \
> universe = container
>
> container\_image =  /cvmfs/singularity.opensciencegrid.org/opensciencegrid/osgvo-el9:latest\
> getenv = True\
> arguments = $(DATAFile)
>
> transfer\_input\_files = getZMass.py , /tmp/x509up\_u556800422, LFNTool.py\
> should\_transfer\_files = YES \
> when\_to\_transfer\_output = ON\_EXIT
>
> output = jo&#x62;_$(Process).out_ \
> _error = job_$(Process).err \
> log = condor.log
>
> transfer\__output\_files = zcandmass.root_ \
> _transfer\_output\_remaps = "zcandmass.root = zcandmass\__$(Process).root"\
> accounting\_group=group\_cms\
> +SingularityBind = "/cvmfs,/etc/condor,/cms,/cms\_scratch,/var/lib/condor,/run/user"\
> queue DATAFile from filelist.txt

위 작업은 filelist.txt을 읽어 한줄씩 $(DATAFile) 변수에 대입하며 하나씩 실행됩니다. 총 입력 파일이 3개이므로 총 3개의 작업이 제출됩니다.

> **💡 참고**
>
> 해당 작업이 제출될 때마다 $(Process)가 1씩 증가하게 됩니다.

WN에서 run.sh 스크립트를 통해 실행이 되며 인자로 $(DATAFile)이 입력됩니다. 이 때, run.sh는 CMSSW 실행을 위한 환경 설정과 ./getZMass.py을 실행합니다.

accounting\_group은 통합팜에서 작업을 실행하기 위해 필요합니다.

container universe는 apptainer 컨테이너 런타임 활용을 위해서 사용됩니다. 사용되는 이미지는 .sif 포맷이나 일반 디렉토리(혹은  /cvmfs)를 지정할 수 있습니다.

### 참고문헌

1. KISTIBatch GitHub Page : [https://github.com/geonmo/KISTIBatch](https://github.com/geonmo/KISTIBatch)
2.

---

## 6. create-batch를 이용한 배치 작업 수행

*create-batch는 경희대 고정환 교수님이 개발 및 유지보수를 하고 계신 작업 제출 프로그램입니다. 이 프로그램은 기존 HTCondor 자체를 사용한 작업 제출보다 편리하게 작업을 수행할 수 있습니다.*

### 1. 프로그램 획득

create-batch는 github를 통해 다운로드 받으실 수 있습니다.

```bash
git clone https://github.com/cms-kr/hep-tools.git
```

다운로드된 스크립트 중 create-batch를 이용하시면 됩니다. $HOME/bin 디렉토리로 복사해두시면 매우 편리하게 사용이 가능합니다.

```bash
cp hep-tools/create-batch $HOME/bin
```

### 2. 사용방법 안내

create-batch는 최신 트렌드에 맞쳐 **cmsRun 작업**과 **일반 bash를 통한 스크립트 작업** 모두 **대응 가능**합니다. 다음 도움말을 참고하여 cmsRun인지 일반 스크립트인지에 따라 적절하게 사용하시기 바랍니다.

```bash
(base) [geonmo@ui10 hep-tools]$ ./create-batch  
./create-batch  : create pbs/condor jobs 
Usage: create-batch cmsRun --cfg CONFIG_cfg.py --jobName JOBNAME --fileList fileList.txt --maxFiles MAXFILE        

       create-batch bash --cfg YOUR_BASH_script.sh --jobName JOBNAME --nJobs NJOBS --transferFiles YOUR_OUTPUT_FILES  
Mandatory options :   
       --jobName  NAME                         Name of job   
       --fileList DATA_FILES            File list text file   
       --maxFiles N                     Maximum number of files per job   
       --nJobs    N                     Number of Job sections   
       --cfg      CONFIG_FILE_cfg.py    Configuration file  
Optional :   
       --queue QUEUE_NAME               Set the batch queue name   
       --secondFileList DATA_FILES      Secondary file list text file   
       -n                               Do not submit jobs to batch   
       --transferDest OUTPUT_LOCATION   OUTPUT DIRECTORY (/store will be assumed to SE)   
       -G                               Disable Grid certificate   
       --maxEvent N                     Maximum number of events per job (-1 by default)   
       --transferFiles                  Additional files to transfer   
       -T                               Automatically transfer new files by archiveing them   
       --customise CUSTOMISE_cfg.py     Configuration file for customization   
       --args                           general arguments   
       --firstRun N                     For MC: run number   
       -B                               Rebuild whole package before job starts  
Optional, condor-specific :   
       --blacklist HOST1,HOST2,...      Remove specific hosts   
       --whitelist HOST1,HOST2,...      Use specific hosts
```

\--whitelist 혹은 --blacklist 옵션을 통하여 특정 머신에서만 작업이 돌도록 지정하거나 해당 머신을 제외하고 작업을 수행하도록 지정할 수 있습니다. (--blacklist cms-t3-wn3001.sdfarm.kr)

### 참고문헌

1. [https://github.com/cms-kr/hep-tools](https://github.com/cms-kr/hep-tools)

---

## 7. 데이터 전송 및 데이터셋 요청

*데이터 파일을 전송하는 방법과 CMS 데이터셋 전송을 요청하는 방법을 알려드립니다.*

**3. CMS Tier-3 컴퓨팅 환경** 장에 대한 설명 중 gsiftp와 xroot 프로토콜에 관한 설명을 드렸습니다. 연구자 분들은 언제 어느때이건 본인에게 필요한 파일을 Tier-3로 전송할 수 있습니다.

### 1. 데이터 전송

#### 데이터 파일의 위치를 정확히 알 때

데이터 파일의 위치를 정확하게 알고 있을 경우에는 데이터 전송을 매우 빠르고 쉽게 처리할 수 있습니다. 다음 명령어로 데이터를 전송할 수 있습니다.

xrdcp root://\[source of data file]//\[Path of source data file \[Dest]

```bash
xrdcp root://eoscms.cern.ch//store/mc/SAM/GenericTTbar/AODSIM/CMSSW_9_2_6_91X_mcRun1_realistic_v2-v1/00000/CE860B10-5D76-E711-BCA8-FA163EAA761A.root .

## xrdcp는 -r (하위 디렉토리 포함) 기능이 지원됩니다.
xrdcp root://eoscms.cern.ch//store/mc/SAM/GenericTTbar/AODSIM/CMSSW_9_2_6_91X_mcRun1_realistic_v2-v1/00000 .

```

> **⚠️ 주의**
>
> 수신과 송신처는 local 디렉토리 혹은 XRootD 서버여야 합니다.

#### 데이터 파일의 위치를 알지 못할 때

데이터 파일의 위치를 알지 못할 때에는 데이터의 위치를 찾아서 전송을 하거나 \
데이터 파일의 위치와 상관 없이 CMS AAA(Any Data, Anytime, Anywhere) 기능을 이용하여 데이터를 전송할 수 있습니다.

##### 데이터 파일 찾기

데이터 파일의 위치를 찾기 위해서는 dasgoclient 혹은 DAS 홈페이지를 사용하시는 것이 가장 편리합니다.

```bash
dasgoclient --query="site file=/store/mc/RunIIFall17NanoAODv5/QCD_HT1000to1500_TuneCP5_13TeV-madgraph-pythia8/NANOAODSIM/PU2017_12Apr2018_Nano1June2019_102X_mc2017_realistic_v7-v1/60000/04323B9F-4B66-2D44-89DE-644E8C48246D.root"

```

> T1\_RU\_JINR\_Disk \
> T1\_US\_FNAL\_Buffer \
> T1\_US\_FNAL\_MSS \
> T2\_KR\_KISTI \
> **T2\_US\_Purdue** \
> T2\_US\_Wisconsin \
> T3\_KR\_KISTI

저장된 사이트의 이름을 확인하였으며 해당 사이트의 prefix를 확인합니다.

```
cat /cvmfs/cms.cern.ch/SITECONF/T2_US_Purdue/PhEDEx/storage.xml | grep "root://"
```

> ```
>   <lfn-to-pfn protocol="xrootd" destination-match=".*" path-match="/+store/(.*)" result="root://cmsxrootd.fnal.gov//store/$1"/>
>   <lfn-to-pfn protocol="root" destination-match=".*" path-match="/+store/(.*)" result="root://xrootd.rcac.purdue.edu//store/$1"/>
>   <pfn-to-lfn protocol="root" destination-match=".*" path-match="root://xrootd.rcac.purdue.edu//store/(.*)" result="/$1"/>
>
> ```

위 경우 xrootd 프로토콜은

> [root://xrootd.rcac.purdue.edu//store/$1](root://xrootd.rcac.purdue.edu/store/$1)

인 것을 알 수 있습니다. 해당 prefix를 이용하여 파일을 전송해봅시다.

```
xrdcp root://xrootd.rcac.purdue.edu//store/mc/RunIIFall17NanoAODv5/QCD_HT1000to1500_TuneCP5_13TeV-madgraph-pythia8/NANOAODSIM/PU2017_12Apr2018_Nano1June2019_102X_mc2017_realistic_v7-v1/60000/04323B9F-4B66-2D44-89DE-644E8C48246D.root .
```

##### 데이터 자동 탐색 기능(CMS AAA) 사용하기

CMS AAA 기능을 사용하면 사이트의 탐색 없이 파일을 전송할 수 있습니다. 고정된 접두어(root://cmsxrootd.fnal.gov)를 사용하여 파일을 전송합니다.

```
xrdcp root://cmsxrootd.fnal.gov//store/mc/RunIIFall17NanoAODv5/QCD_HT1000to1500_TuneCP5_13TeV-madgraph-pythia8/NANOAODSIM/PU2017_12Apr2018_Nano1June2019_102X_mc2017_realistic_v7-v1/60000/04323B9F-4B66-2D44-89DE-644E8C48246D.root .
```

> **💡 참고**
>
> 현재 CMS AAA는 가장 빠른 사이트를 검색하는 등의 추가적인 기능이 제공되지 않고 있습니다. 따라서, 국내에 있는 다른 사이트에서 파일을 전송하고 싶어도 유럽이나 미국에서 파일을 전송할 가능성이 있습니다. 전송 받을 파일이 많아지면 데이터를 직접 탐색하여 접두어를 넣어 파일을 전송하도록 합시다.

### 2. 데이터셋 전송 요청

##### (1) Rucio (For Public Dataset)

현재 rucio 명령어를 통해 데이터를 전송하려면 T3\_KR\_KISTI에 대한 공간 쿼터를 할당 받아야 합니다. 혹시 쿼터가 필요한 분들은 다음 정보를 포함하여 이메일로 [요청](mailto:cmst3-support.kisti.re.kr)하시기 바랍니다.

1. 요청하는 사용자의 "CERN Computing ID"
2. 요청하는 쿼터 용량
3. 쿼터 요청하는 사이트 (T2 또는 T3)

* 다음 명령어로 rucio 명령어를 사용 가능하도록 설정합니다. (rucioenv 명령어로 대체 가)

```
###CMSSW 디렉토리 내에서 cmsenv를 하였을 경우 작동 안될 가능성 있음.
source /cvmfs/cms.cern.ch/cmsset_default.sh 
source /cvmfs/cms.cern.ch/rucio/setup.sh 
export RUCIO_ACCOUNT=<CERN ID>
voms-proxy-init --voms cms

### CERN과 KISTI ID가 같다면 다음 명령어로 바로 적용 가능합니다.
rucioenv 
```

* 아래 명령어로 데이터셋 전송 요청을 진행합니다.

```
### 쿼터가 있을 경우
rucio add-rule cms:<DATASET> 1 T3_KR_KISTI

## 쿼타가 없어서 데이터셋마다 확인이 필요할 경우,
rucio add-rule --ask-approval cms:<DATASET> 1 T3_KR_KISTI
```

* 아래 명령어로 데이터셋 전송 상태를 확인행합니다

```
rucio list-rules --account geonmo
```

* 출력형식
  * \[rucio ID as HexCode] \[User] cms:\[DataSet] \[Status/ OK, REPL,STUCK] \[Tier Center] \[COPIES]
    * OK : 전송완료
    * REPL : 전송 중
    * STUCK : 전송 실패 혹은 전송 작업 승인 대기

> 20e8a18cb7e04a75b8d8106baf4ece01 geonmo cms:/gluinoGMSB\_M3000\_ctau10000p0\_TuneCP2\_13TeV\_pythia8/RunIISummer16NanoAODv7-PUMoriond17\_Nano02Apr2020\_102X\_mcRun2\_asymptotic\_v8-v1/NANOAODSIM OK\[9/0/0] T3\_KR\_KISTI 1

* 총 9개의 파일 중 9개 전송 완료 \[OK:9, REPL: 0, STUCK:0]

##### (2) xrdDownload.py (for User Dataset)

개인 사용자가 만들어서 publish한 dataset은 아직 rucio를 통해 전송이 불가능합니다. 이에 따라, 각 파일들을 쉽게 전송할 수 있는 스크립트를 제공하고 있습니다. 아래 절차대로 데이터 전송 작업을 진행하시기 바랍니다.

* [hep-tools](https://github.com/cms-kr/hep-tools) Git 저장소로부터 dataset2filelist.sh와 xrdDownload.py 를 본인의 PATH 경로로 다운로드 받습니다.
* dataset2filelist.sh 를 이용하여 다운로드 받으려는 데이터셋들의 개별 파일 목록을 작성합니다.

**📄 input_dataset.txt**
```bash
/TTToSemiLeptonic_TuneCP5_13TeV-powheg-pythia8/swertz-TopNanoAODv6-1-1_2018-0d1d4920f08f56d048ece029b873a2cc/USER
/TTTo2L2Nu_TuneCP5_13TeV-powheg-pythia8/palencia-TopNanoAODv6-1-1_2018-0d1d4920f08f56d048ece029b873a2cc/USER
/TTToHadronic_TuneCP5_13TeV-powheg-pythia8/swertz-TopNanoAODv6-1-1_2018-0d1d4920f08f56d048ece029b873a2cc/USER
```

**▶ Bash**
```bash
[geonmo@ui20 migration]$ dataset2filelist.sh input_dataset.txt datalist.txt
```

dataset2filelist.sh \<from dataset list file> \<to data list result file>

**📄 datalist.txt**
```bash
/store/user/swertz/topNanoAOD/v6-1-1/2018/TTToSemiLeptonic_TuneCP5_13TeV-powheg-pythia8/TopNanoAODv6-1-1_2018/200610_101606/0000/tree_597.root   615580493  
/store/user/swertz/topNanoAOD/v6-1-1/2018/TTToSemiLeptonic_TuneCP5_13TeV-powheg-pythia8/TopNanoAODv6-1-1_2018/200610_101606/0000/tree_24.root   615219932  
/store/user/swertz/topNanoAOD/v6-1-1/2018/TTToSemiLeptonic_TuneCP5_13TeV-powheg-pythia8/TopNanoAODv6-1-1_2018/200610_101606/0000/tree_17.root   615423490  
/store/user/swertz/topNanoAOD/v6-1-1/2018/TTToSemiLeptonic_TuneCP5_13TeV-powheg-pythia8/TopNanoAODv6-1-1_2018/200610_101606/0000/tree_387.root   616238957  
/store/user/swertz/topNanoAOD/v6-1-1/2018/TTToSemiLeptonic_TuneCP5_13TeV-powheg-pythia8/TopNanoAODv6-1-1_2018/200610_101606/0000/tree_538.root   615412910  
/store/user/swertz/topNanoAOD/v6-1-1/2018/TTToSemiLeptonic_TuneCP5_13TeV-powheg-pythia8/TopNanoAODv6-1-1_2018/200610_101606/0000/tree_32.root   615054868  
/store/user/swertz/topNanoAOD/v6-1-1/2018/TTToSemiLeptonic_TuneCP5_13TeV-powheg-pythia8/TopNanoAODv6-1-1_2018/200610_101606/0000/tree_535.root   615491181  
/store/user/swertz/topNanoAOD/v6-1-1/2018/TTToSemiLeptonic_TuneCP5_13TeV-powheg-pythia8/TopNanoAODv6-1-1_2018/200610_101606/0000/tree_405.root   615219946  
/store/user/swertz/topNanoAOD/v6-1-1/2018/TTToSemiLeptonic_TuneCP5_13TeV-powheg-pythia8/TopNanoAODv6-1-1_2018/200610_101606/0000/tree_622.root   615852354  
/store/user/swertz/topNanoAOD/v6-1-1/2018/TTToSemiLeptonic_TuneCP5_13TeV-powheg-pythia8/TopNanoAODv6-1-1_2018/200610_101606/0000/tree_147.root   615512903 
```

* xrdDownload.py 스크립트에 해당 파일리스트를 이용하여 전송을 합니다.

**▶ Bash**
```bash
[geonmo@ui20 migration]$ xrdDownload.py -i datalist.txt -p 4
missing file                                                                                                                                                
missing file                                                                                                                                                
missing file
missing file
missing file
Source:  root://cmsxrootd.fnal.gov//store/group/lpccoffea/coffeabeans/NanoAODv6/nano_2016/MET/NanoTuples-2016_Run2016B-17Jul2018_ver2-v1/191210_034740/0001/nano_1444.root
Destination:  root://cms-xrdr.private.lo:2094//xrd//store/group/lpccoffea/coffeabeans/NanoAODv6/nano_2016/MET/NanoTuples-2016_Run2016B-17Jul2018_ver2-v1/191210_034740/0001/nano_1444.root
Source:  root://cmsxrootd.fnal.gov//store/group/lpccoffea/coffeabeans/NanoAODv6/nano_2016/MET/NanoTuples-2016_Run2016B-17Jul2018_ver2-v1/191210_034740/0002/nano_2098.root
Destination:  root://cms-xrdr.private.lo:2094//xrd//store/group/lpccoffea/coffeabeans/NanoAODv6/nano_2016/MET/NanoTuples-2016_Run2016B-17Jul2018_ver2-v1/191210_034740/0002/nano_2098.root
Source:  root://cmsxrootd.fnal.gov//store/group/lpccoffea/coffeabeans/NanoAODv6/nano_2016/MET/NanoTuples-2016_Run2016B-17Jul2018_ver2-v1/191210_034740/0000/nano_229.root
Source:  root://cmsxrootd.fnal.gov//store/group/lpccoffea/coffeabeans/NanoAODv6/nano_2016/MET/NanoTuples-2016_Run2016B-17Jul2018_ver2-v1/191210_034740/0000/nano_343.root
Destination:  root://cms-xrdr.private.lo:2094//xrd//store/group/lpccoffea/coffeabeans/NanoAODv6/nano_2016/MET/NanoTuples-2016_Run2016B-17Jul2018_ver2-v1/191210_034740/0000/nano_229.root
Destination:  root://cms-xrdr.private.lo:2094//xrd//store/group/lpccoffea/coffeabeans/NanoAODv6/nano_2016/MET/NanoTuples-2016_Run2016B-17Jul2018_ver2-v1/191210_034740/0000/nano_343.root

/store/group/lpccoffea/coffeabeans/NanoAODv6/nano_2016/MET/NanoTuples-2016_Run2016B-17Jul2018_ver2-v1/191210_034740/0000/nano_229.root is failed.        
Total: 1565 / Success: 1 / Fail: 0

/store/group/lpccoffea/coffeabeans/NanoAODv6/nano_2016/MET/NanoTuples-2016_Run2016B-17Jul2018_ver2-v1/191210_034740/0000/nano_343.root is failed.          
Source:  root://cmsxrootd.fnal.gov//store/group/lpccoffea/coffeabeans/NanoAODv6/nano_2016/MET/NanoTuples-2016_Run2016B-17Jul2018_ver2-v1/191210_034740/0001/nano_1009.root
Destination:  root://cms-xrdr.private.lo:2094//xrd//store/group/lpccoffea/coffeabeans/NanoAODv6/nano_2016/MET/NanoTuples-2016_Run2016B-17Jul2018_ver2-v1/191210_034740/0001/nano_1009.root
Total: 1565 / Success: 2 / Fail: 0

/store/group/lpccoffea/coffeabeans/NanoAODv6/nano_2016/MET/NanoTuples-2016_Run2016B-17Jul2018_ver2-v1/191210_034740/0001/nano_1444.root is failed.         
Source:  root://cmsxrootd.fnal.gov//store/group/lpccoffea/coffeabeans/NanoAODv6/nano_2016/MET/NanoTuples-2016_Run2016B-17Jul2018_ver2-v1/191210_034740/0000/nano_319.root
/store/group/lpccoffea/coffeabeans/NanoAODv6/nano_2016/MET/NanoTuples-2016_Run2016B-17Jul2018_ver2-v1/191210_034740/0002/nano_2098.root is failed.         
Destination:  root://cms-xrdr.private.lo:2094//xrd//store/group/lpccoffea/coffeabeans/NanoAODv6/nano_2016/MET/NanoTuples-2016_Run2016B-17Jul2018_ver2-v1/191210_034740/0000/nano_319.root
Total: 1565 / Success: 3 / Fail: 0

Source:  root://cmsxrootd.fnal.gov//store/group/lpccoffea/coffeabeans/NanoAODv6/nano_2016/MET/NanoTuples-2016_Run2016B-17Jul2018_ver2-v1/191210_034740/0000/nano_853.root
Destination:  root://cms-xrdr.private.lo:2094//xrd//store/group/lpccoffea/coffeabeans/NanoAODv6/nano_2016/MET/NanoTuples-2016_Run2016B-17Jul2018_ver2-v1/191210_034740/0000/nano_853.root
Total: 1565 / Success: 4 / Fail: 0

```

\-i \<Data list file> : 데이터 파일 리스트를 지정합니다. 복사 과정 중에 이미 전송된 파일들의 경우 자동으로 빠지기 때문에 전체 리스트를 넣으면 됩니다.

\-p \<num of parallel> : 동시에 전송할 파일 개수를 지정합니다. 4개가 적당하며 6개 이상부터는 후반에 에러가 발생할 확률이 높습니다. 네트워크 상태에 따라 선택하면 됩니다.

> **💡 참고**
>
> xrdDownload.py는 기존 파일의 크기를 이용하여 상태를 확인합니다. 따라서, 일부만 전송되었거나 전송되지 않은 파일들만 전송을 시도하기 때문에 도중에 작업을 취소한 후에 다시 시작할 수 있습니다.

### 3. 전송 요청한 데이터셋 삭제 (쿼터 회복)

rucio 로 전송한 데이터셋은  rucio delete-rule 명령어로 삭제가 가능합니다. 단, 다른 사용자가 같은 데이터셋을 신청한 상태라면 데이터셋 자체는 삭제되지 않습니다. 하지만, 쿼터는 복구되므로 필요한 쿼터를 확보할 수 있습니다.

```bash
[geonmo@ui20 ~]$ rucio list-rules --account geonmo
ID                                ACCOUNT    SCOPE:NAME
                                    STATE[OK/REPL/STUCK]    RSE_EXPRESSION      COPIES  EXPIRES (UTC)    CREATED (UTC)
--------------------------------  ---------  ------------------------------------------------------------------------------------------------------------------------------------------------
----------------------------------  ----------------------  ----------------  --------  ---------------  -------------------
2de8f9060ac94da38b898d9fb267d16f  geonmo     cms:/StealthSHH_2t4b_mStop-1200_mSo-100_TuneCP2_13TeV-madgraphMLM-pythia8/RunIIAutumn18NanoAODv7-Nano02Apr2020_102X_upgrade2018_realistic_v21-v1
/NANOAODSIM                         OK[8/0/0]               T3_KR_KISTI              1                   2020-08-12 01:30:18

[geonmo@ui20 ~]$ rucio delete-rule 2de8f9060ac94da38b898d9fb267d16f
```

### 4. rucio 사용량 및 쿼터 확인

rucio를 호라용하실 때 사용량 및 쿼터 용량 확인이 필요하신 경우가 있습니다. 아래와 같이 확인하시기 바랍니다.

#### (1) 쿼터 확인

```bash
[geonmo@ui20 ~]$ rucio list-account-limits geonmo                                                                                                        
/cvmfs/cms.cern.ch/rucio/x86_64/slc7/py2/current/lib/python2.7/site-packages/paramiko/transport.py:33: CryptographyDeprecationWarning: Python 2 is no longer
 supported by the Python core team. Support for it is now deprecated in cryptography, and will be removed in the next release.                              
  from cryptography.hazmat.backends import default_backend                                                                                                  
+-------------+-----------+                                                                                                                                  
| RSE         | LIMIT     |                                                                                                                                  
|-------------+-----------|                                                                                                                                  
| T2_KR_KISTI | 20.000 TB |
| T3_KR_KISTI | 20.000 TB |
+-------------+-----------+
+------------------+---------+
| RSE EXPRESSION   | LIMIT   |
|------------------+---------|
+------------------+---------+

```

#### (2) 사용량 확인

```bash
[geonmo@ui20 ~]$ rucio list-rse-usage --show-accounts T3_KR_KISTI
USAGE:
------
  used: 16.726 TB
  rse: T3_KR_KISTI
  updated_at: 2021-09-08 02:20:32
  source: expired
  rse_id: 1983977ffc0e47ffb2b1c4d496a8297f
------
  used: 4.157 TB
  rse: T3_KR_KISTI
  updated_at: 2021-09-08 02:20:07
  source: obsolete
  rse_id: 1983977ffc0e47ffb2b1c4d496a8297f
------
  files: 78660
  used: 109.455 TB
  rse: T3_KR_KISTI
  updated_at: 2021-09-02 06:46:12
  source: rucio
  per account:
  ------
    account: sync_t3_kr_kisti       used: 64.192 TB                 percentage: 58.65                                                                      
  ------
    account: jlee                   used: 12.457 TB                 percentage: 11.38                                                                      
  ------
    account: geonmo                 used: 7.077 TB                  percentage: 6.47                                                                       
  ------
    account: heewon                 used: 4.831 TB                  percentage: 4.41                                                                       
  ------
    account: yhoonlee               used: 3.691 TB                  percentage: 3.37                                                                       
  ------
    account: jichoi                 used: 2.658 TB                  percentage: 2.43                                                                       
  ------
    account: soohwan                used: 2.204 TB                  percentage: 2.01                                                                       
  ------
    account: mchoi                  used: 1.995 TB                  percentage: 1.82                                                                       
  ------
    account: yjeong                 used: 1.507 TB                  percentage: 1.38                                                                       
  ------
    account: haoh                   used: 128.632 GB                percentage: 0.12                                                                       
  ------
    account: gylee                  used: 50.821 GB                 percentage: 0.05                                                                       
  ------
    account: dakang                 used: 10.063 GB                 percentage: 0.01                                                                       
  ------
  rse_id: 1983977ffc0e47ffb2b1c4d496a8297f
------
  used: 15.442 GB
  rse: T3_KR_KISTI
  updated_at: 2021-09-02 15:00:13
  source: unavailable
  rse_id: 1983977ffc0e47ffb2b1c4d496a8297f
------

```

---

## 8. 스토리지 접근 방법

*/xrootd 스토리지에 저장된 데이터에 접근하는 방법에 대하여 설명드립니다.*

### 데이터 사용량 확인

Google Sheet를 이용하여 사용자들의 데이터 사용량 확인이 가능합니다. 아래 링크를 확인하시기 바랍니다.

[구글 시트 모니터링 페이지](https://docs.google.com/spreadsheets/d/1mk335AFIkW7X4Pod8pecfTw0m1-S_NTjNkxFzHEwAL0/edit#gid=1404507412)

### 데이터 쓰기 방법

UI서버들의 /xrootd 공간을 확인하시면 데이터 파일들의 경로와 위치 정보를 아실 수 있습니다. 일반적으로 사용자는 /xrootd공간 이후 어느 공간이든 사용이 가능하지만 다음과 같은 구조로 데이터들을 저장하고 있습니다.

##### xrdcp 명령어 사용하기

xrdcp는 xrootd 프로토콜을 이용한 데이터 전송 프로그램입니다. 사용방법은 scp와 매우 유사하지만 대상이 xrootd 프로토콜이어야 합니다. 테스트를 위해 CMS Tier-3 XRootD 디렉토리에 저장된 파일을 이용합니다. /xrootd 디렉토리는 각 UI 서버들에 마운트되어 있습니다.

```bash
[geonmo@ui20 geonmo]$ ls /xrootd/store/user/geonmo/dummy.dat 
/xrootd/store/user/geonmo/dummy.dat
[geonmo@ui20 geonmo]$ xrdcp -f root://cms-xrdr.private.lo:2094//xrd/store/user/geonmo/dummy.dat /dev/null
[100MB/100MB][100%][==================================================][100MB/s]  
```

##### xrdMigration.py 사용하기

xrdMigration.py는 /cms/lda&#x70;_&#x68;ome,_ /cms/scratch, /cms\_scratch에 저장된 데이터 파일들을 /xrootd/store/user/\<USERID>로 옮기기 위해 개발되었습니다. 해당 /cms 디렉토리들의 경로를 지정하면 동일한 경로명으로 /xrootd 디렉토리로 옮겨줍니다.

**▶ 옵션 설명**
```bash
[geonmo@ui20 ~]$ xrdMigration.py -d -c geonmo /cms/ldap_home/geonmo/migration_test/
Number of files is 1

xrdcp -p /cms/ldap_home/geonmo/migration_test/subdir root://cms-xrdr.private.lo:2094//xrd/store/user/geonmo/migration_test/subdir
xrdcp: /cms/ldap_home/geonmo/migration_test/subdir is a directory.
```

-d                     : 복사 후 checksum 값 비교후 정상이면 기존 파일 삭제\
-c \<CERN ID> : CERN ID를 기입하여 실행 속도 향상. -c 옵션이 없을 경우 CERN CRIC에 질의를 하여 사용자의 인증서 정보를 이용하여 CERN ID 정보를 가져오기 때문에 약 1분 가량의 지연시간이 발생합니다.

xrdMigration.py 스크립트를 사용하면 새로 생성된 디렉토리의 캐쉬 정보를 리셋을 합니다. 따라서, 디렉토리를 생성하였으나 실제로 없는 디렉토리라면서 접근이 안되는 현상이 사라집니다. 되도록 xrdMigration.py를 사용하길 추천드립니다.

> **❗ 중요**
>
> /xrootd 디렉토리에서는 절대로 mv 명령어를 사용하시면 안됩니다. 현재 시스템 상으로 mv를 하면 파일이 사라질 가능성이 존재합니다. \
> (최근 파일의 경우 더더욱 확률이 높습니다.)

##### 데이터 접근 방식

WN에서 스토리지 데이터를 접근하는 방법에 대해서 기술합니다.

##### (1) CMS T3\_KR\_KISTI 데이터 접근

**▶ ROOT**
```
## XRootD 프로토콜로 접근
root -l root://cms-xrdr.private.lo:2094//xrd/store/user/<CERNID>/~~.root
or
datafile = TFile::Open("root://cms-xrdr.private.lo:2094//xrd/store/user/~~");
```

**▶ PyROOT**
```
## In pyROOT,
datafile = TFile.Open("root://cms-xrdr.private.lo:2094//xrd/store/user/~~")
```

**▶ CMSSW**
```
## T3 데이터의 경우, LFN 및XRootD PFN 접근 둘다 가능합니다.
process.source = cms.Source("PoolSource",                            
            fileNames = cms.untracked.vstring(
            '/store/user/~~~.root',
            'root://cms-xrdr.private.lo:2094//xrd/store/user/~~.root',
    )
)
```

##### (2) CMS T2\_KR\_KISTI 데이터 접근

**▶ ROOT**
```
## XRootD 프로토콜로 접근
root -l root://cms-t2-se01.sdfarm.kr:1096//store/user/<CERNID>/~~.root
or
datafile = TFile::Open("root://cms-t2-se01.sdfarm.kr:1096//store/user/~~");
```

**▶ PyROOT**
```
## In pyROOT,
datafile = TFile.Open("root://cms-t2-se01.sdfarm.kr:1096//store/user/~~")
```

**▶ CMSSW**
```
## T2 데이터의 경우, XRootD PFN 접근 가능합니다.
process.source = cms.Source("PoolSource",                            
            fileNames = cms.untracked.vstring(
            'root://cms-t2-se01.sdfarm.kr:1096//store/user/~~.root',
    )
)
```

### 디렉토리 정보

##### /xrootd/store/mc

* 위 공간은 CMS DataSet 중 MC 샘플들이 저장되는 공간입니다.

##### /xrootd/store/data

* 위 공간은 CMS Dataset 중 Real Data가 저장되는 공간입니다.

##### /xrootd/store/himc

* 위 공간은 HeavyIon MC 샘플이 저장됩니다.

> **❗ 중요**
>
> 위 공간은 PhEDEx를 이용하여 데이터셋을 신청하여 파일을 전송하실 수 있습니다. 그외에 일반 유저가 해당 디렉토리에 파일을 쓰거나 삭제하지 않도록 주의해주시기 바랍니다.
>
> 다시 한번 말씀드리지만, XRootD 시스템의 기능의 한계로 인하여 일반 유저가 해당 공간에 대해 쓰기 권한을 가지고 있습니다. 해당 공간에 대해서 삭제 기능을 수행하지 않도록 주의를 기울여주시기 바랍니다.

##### /xrootd/store/user

* 위 공간은 개별 사용자들의 데이터 샘플을 장기 보관하는 공간입니다.
* 각 사용자들은 해당 공간에 데이터를 집어넣기 위해 gfal-copy, xrdcp 명령어를 사용하거나 UI 서버에서 /xrootd\_user/\[ID]/xrootd 디렉토리를 이용하여 데이터를 복사할 수 있습니다.

> **⚠️ 주의**
>
> /xrootd\_user 공간을 사용하실 때에는 mv 같은 암시적인 삭제 기능을 수행하지 않도록 주의해주시기 바랍니다. 해당 공간에서 mv를 실행하면 확률적으로 파일이 손실됩니다. 따라서, 꼭 cp로 복사한 후 복사파일들을 확인한 후 rm으로 지워주시기 바랍니다.

##### /xrootd/store/group

위 공간은 KCMS 연구자들이 생산한 공용 데이터들을 저장하고 있습니다.

---

## 9. CRAB3 사용 관련

*KISTI Tier3에 CRAB3를 사용할 때 주의사항에 관해 알려드립니다.*

### CRAB3 설정

CRAB3의 자세한 설정은 [CRAB3 페이지](https://twiki.cern.ch/twiki/bin/view/CMSPublic/WorkBookCRAB3Tutorial#Output\_dataset\_publication) 를 참고하시기 바랍니다. 여기서는 주요내용에 대해서만 다룹니다.

#### CRAB3 결과를 KISTI에 저장하기

Tier-3에 결과파일을 저장하기 위해서는 아래 옵션을 사용합니다. 저장된 데이터셋은 /xrootd/store/user/\<CERN ID> 밑에서 확인하실 수 있습니다.

**📄 crabConfig.py**
```python
config.General.transferOutputs = True
config.Site.storageSite = T3_KR_KISTI
# T2에 저장하려면
config.Site.storageSite = T2_KR_KISTI
```

> **❗ 중요**
>
> T3\__KR\_KISTI에  저장된 데이터는 더이상 CRAB 작업을 수행할 수 없습니다. CRAB 활용이 필요하다면 해당 데이터셋을 반드시 T2\_KR\_KISTI로 저장하세요._

#### 2차 작업이 필요한 경우

2차 작업이 필요할 경우 해당 데이터셋을 반드시 publish하여야 합니다. 그렇지 않으면 CRAB에서는 사용이 불가능합니다.

**📄 crabConfig.py**
```python
config.General.transferOutputs = True
config.Site.storageSite = T2_KR_KISTI
config.Data.publication = True
config.Data.outputDatasetTag = '<DATASET's NAME>'
```

생성된 데이터셋을 대상으로 작업을 돌릴 경우에는 DBS Instance를 "global"에서 "phys03"으로 변경해야 합니다.

**📄 crabConfig_2nd.py**
```python
config.Data.inputDataset = '<DATASET's NAME>/USER'
config.Data.inputDBS = 'phys03'
```

---

## 10. 빅데이터 분석 환경 구축하기

*최근들어 빅데이터 분석 시스템을 이용한 CMS 데이터 분석을 활용하시는 사용자분들이 늘어나고 있습니다. 이에 따라, 해당 환경 구축 방법에 대해 알려드립니다.*

### Apptainer를 이용한 PyTorch 사용하기

pytorch 사용을 위한 NGC 컨테이너 사용을 추천드립니다.

```
## 컨테이너 이미지 다운로드 (/tmp 20GB 이상 필요)
apptainer pull docker://nvcr.io/nvidia/pytorch:23.11-py3
apptainer shell --nv pytorch_23.11-py3.sif
```

### Apptainer를 이용한 TensorFlow 사용하기

현재 제공되는 GPU 서버에서는 tensorflow를 바로 실행하실 수 없습니다. Singularity 가상 환경을 추천드립니다.

```
## CMS GPU 서버에서 실행하세요.
apptainer pull docker://nvcr.io/nvidia/tensorflow:23.11-tf2-py3
apptainer shell --nv tensorflow_23.11-tf2-py3.sif
```

HTCondor를 이용한 작업 제출은 다음과 같습니다.

```
# Job description file for condor job cat_jpsi_ftl_full
jobbatchname = GPU test
executable = gpu_test.sh
universe   = container
container_image = tensorflow_23.11-tf2-py3.sif
arguments  = $(Process)
log = condor.log

getenv     = True
should_transfer_files = YES
when_to_transfer_output = ON_EXIT

output = job_$(Process).out
error = job_$(Process).err

#+SingularityBindCVMFS = True

request_cpus=1
request_GPUs = 1

queue 1
```

### 주피터 노트북 환경

주피터 노트북은 CMS GPU 서버에서 써보시는 것을 추천드립니다. (P100 그래픽카드 활용 가능)

본인 컴퓨터에서 8888 -> localhost:8889 로 지정하시고, UI 서버에서 8889번 포트를 GPU서버의 localhost:8888번으로 연결하신 후,  jupyter의 포트 번호를 8888로 여시면 됩니다.

MobaXterm 사용자는 다음 링크를 참조하여 본인 컴퓨터측 설정을 완료하시기 바랍니다. 비슷한 방식으로 putty에서 설정이 가능합니다.  ([https://developyo.tistory.com/84](https://developyo.tistory.com/84))

이후 아래 설정대로 진행하시면 됩니다.

```
ssh -Y cms-gpu01
### ssh local tunneling을 통해 간접 접속이 가능합니다.
# 본인의 데스크탑에서 접속을 하시려면 2중 포트 포워딩을 하세요.
# ### ui -> [CMS GPU 서버 FQDN]
# ex) ssh -4 -L 8889:localhost:8888 $USER@[CMS GPU 서버FQDN]
```

#### VirtualEnv 을 이용한 Python 환경 구축

VirtualEnv 를 통해 Python 설치 환경을 일반 사용자로서 설치가 가능하도록 합니다.

```
cd $HOME
### PyROOT 실행을 위해서는 python2 필요. ROOT 사용 안하면 python3 권장.
virtualenv -p python2 my_workspace
```

#### 작업 공간에서 Python 패키지 설치

대부분의 경우 해당 패키지들을 pip로 설치하게 됩니다.

```
cd my_workspace
source bin/activate
mkdir $HOME/pip_cache
TMPDIR=$PWD pip install --cache-dir=$HOME/pip_cache --build $PWD/build jupyter pandas seaborn torch sklearn  metakernel zmq
```

#### Jupyter 실행

```
### 나중에 다시 실행하기 위해서,
source my_workspace/bin/activate
jupyter notebook
## 혹은 포트 개방 후 본인 컴퓨터의 브라우저에서 지정한 포트로 연결하여 사용 가능
jupyter notebook --no-browser --ip 0.0.0.0 --port XXXX
```

#### local 컴퓨터에서 접속

제대로 접속이 되었다면 본인 컴퓨터의 브라우저에서 localhost:8888로 접속이 가능합니다.

![Token을 물어봅니다.](/wiz/api/portal.gsdcadmin.manual.image/download?community=CMS&path=bigdata-jupyter-token.png)

토큰 정보는 jupyter를 실행할 때 표시되므로 해당 내용을 복사&붙여넣기로 접속이 가능합니다.

---

## 11. FAQ 및 문제 해결

*유저들의 질의사항을 기록하여 비슷한 문제를 겪는 사용자들에게 답을 줄 수 있도록 안내해드리는 페이지입니다.*

#### 1. scram build시 컴파일 에

![컴파일 에러가 발생하는 경우가 있습니다.](/wiz/api/portal.gsdcadmin.manual.image/download?community=CMS&path=faq-scram-build-error.png)

위 에러의 경우는 CMSSW framework 디렉토리에서 CMSSW 환경과 sft.cern.ch에 있는 ROOT 환경이 충돌하여 발생하였습니다. 깨끗한 환경변수 상태에서 cmsenv만 진행 후 작업하면 이상이 없습니다.

#### 2. 작업이 끝나지 않아요!

끝날 시간이 되었으나 끝나지 않는 작업의 경우 몇가지를 확인하여야 합니다.

(1) 사용하는 데이터 파일이 외부에 있는 경우,

작업의 CPU 사용률을 확인하면 대략 알 수 있습니다.

```
[geonmo@ui20 manifests]$ condor_jobstatus runiyal
 ID         Owner   RemoteHost                    CPUsUsage            
9100463.0   runiyal slot1@bio-wn3006.sdfarm.kr    0.001307340657177903 
9100471.0   runiyal slot1@bio-wn3017.sdfarm.kr    0.002598957544583386 
9100537.0   runiyal slot1@bio-wn3015.sdfarm.kr    0.002616651650138061 
9100561.0   runiyal slot1@bio-wn3015.sdfarm.kr    0.002691392044219451 
9100664.0   runiyal slot3@bio-wn3016.sdfarm.kr    0.002509965002770551 
9100709.0   runiyal slot1@bio-wn3011.sdfarm.kr    0.004661354284485942 
9101560.0   runiyal slot2@bio-wn3011.sdfarm.kr    0.004759655887460485 
9101561.0   runiyal slot3@condor-wn1012.sdfarm.kr 0.02173146070995057  
9101575.0   runiyal slot2@condor-wn1008.sdfarm.kr 0.002791534221159765 

```

분석 데이터 파일이 KISTI가 아니라 외부의 있는 경우 뿐만 아니라 premixing 등에 사용되는 pileup event가 외부 사이트에 있을 경우에도 속도가 매우 떨어지게 됩니다. 해결방법은 해당 데이터들을 사이트 관리자의 도움을 받아 KISTI로 전송하는 것입니다.

(2)  머신 상태가 불량한 경우,

아래처럼 작업의 CPU 사용률이 0.0이면서 같은 머신에서 여러 작업들이 멈쳐 있는 경우라면 해당 머신에 문제가 생긴 것으로 판단할 수 있습니다.

```
[geonmo@ui20 manifests]$ condor_jobstatus geonmo
9100560.0   geonmo slot1@condor-wn1016.sdfarm.kr 0.0                  
9100560.1   geonmo slot1@condor-wn1016.sdfarm.kr 0.0                  
9100560.2   geonmo slot1@condor-wn1016.sdfarm.kr 0.0                  
```

#### 3. 패스워드를 리셋하고 싶어요!

패스워드를 리셋하려면 텔레그램에서 @GSDCAccountBot을 친구 추가해주세요.

* GSDCAccountBot 창에서 "/start" 명령어로 시작합니다.

![/start 명령어로 시작할 때,](/wiz/api/portal.gsdcadmin.manual.image/download?community=CMS&path=faq-pwreset-1-start.png)

* \[Reset password]를 선택하면 "커뮤니티" 이름을 물어봅니다. 이때, "CMS"를 입력합니다.

![커뮤니티 이름은 \[CMS\] 입니다.](/wiz/api/portal.gsdcadmin.manual.image/download?community=CMS&path=faq-pwreset-2-community.png)

* 아이디와 이메일 주소를 입력합니다.

![아이디와 이메일 주소는 가입할 때 정보와 일치해야 합니다.](/wiz/api/portal.gsdcadmin.manual.image/download?community=CMS&path=faq-pwreset-3-id-email.png)

* 이메일로 수신된 OTP 코드를 입력합니다.

![](/wiz/api/portal.gsdcadmin.manual.image/download?community=CMS&path=faq-pwreset-4-otp.png)

![](/wiz/api/portal.gsdcadmin.manual.image/download?community=CMS&path=faq-pwreset-5-done.png)

* 정상적으로 마무리되면 이메일 주소로 리셋된 패스워드가 전송됩니다. 해당 패스워드로 접속을 하면 패스워드를 바로 변경할 수 있습니다.
* 이후 다시 리셋을 할때에는 "/start" 명령을 클릭해야 합니다.

#### 4. /xrootd/store/user/\<ID> 디렉토리와 /xrootd\_user/\<ID> 디렉토리가 없어요!

/xrootd\_user 디렉토리는 스토리지의 쓰기 관련한 작업을UI 서버에서 처리 할 수있도록 지원하기 위해 생성된 디렉토리입니다. 이 디렉토리는 관리자가 생성해주는디렉토리입니다. 관리자에게 CENRN Account ID와 함께 요청을 해주세요.

/xrootd/store/user/\<ID> 디렉토리는 CRAB 작업 제출을 통해 생성이가능합니다. CRAB3 사용관련 페이지를 참고해주세요.

#### 5. UI 서버에서 작업을 하려고 하는데 타이핑도 늦어질 정도로 느려요!

현재까지 알려진 속도 지연현상의 원인은 아래와 같습니다. (1) CPU 과다 사용 이 경우는 사용자가 top 명령어를 통해 CPU 사용량을 확인하면 쉽게 알 수 있습니다. 해결방법으로 다른 UI 서버를 사용하시면 됩니다. 또한, 자원을 남용하는 사용자의 계정명을 알려주시면 조치를 취하겠습니다. (2) HOME 디렉토리 과다 사용 홈디렉토리에 과도한 쓰기 작업이 실행되면 모든 사용자들에 디스크 쓰기 필요 작업들에 영향을 주게 됩니다. 대개, 잠깐 기다리면 해결이 됩니다만, 치명적인 문제가 발생할 수도 있기 때문에 다음과 같은 내용 확인 후 관리자에게 전달을 부탁드립니다.

아래 명령 실행 후 shadow가 너무 많은 경우(200개 이상)에는 연락 바랍니다.

```
ps -ef | grep condor_shadow 
```

쓰기 작업의 특성상 지연 현상이 장기간 이어지는 경우는 많지 않습니다. 따라서, 조금만 기다려주시면 해당 이슈가 사라질 겁니다. 대량의 쓰기 작업을 필요로 하시는 사용자 분들께서는 되도록 /cms\_scratch 등 사용자 홈디렉토리와 연관이 적은 스토리지를 활용해주시기를 요청드립니다.
