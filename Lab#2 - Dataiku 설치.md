# 관리자 교육 - 제품 설치

## 전제사항
본 설치 과정은 RHEL/Rocky Linux에 dataiku 계정이 생성이 되어 있고, sudo 권한을 보유한 것으로 전제 합니다.

## 교육 자료 다운로드

```bash
# git 사용
git clone https://github.com/kkobooc/dataiku-admin-train.git
cd dataiku-admin-train

# 또는 zip 다운로드
wget https://github.com/kkobooc/dataiku-admin-train/archive/refs/heads/main.zip
unzip main.zip
cd dataiku-admin-train-main
```

## 설치 디렉토리 생성
```bash
sudo mkdir -p /opt/dataiku
cd /opt/dataiku
```

## 설치 파일 download ( 2.0GB, 약 2분)

```bash
sudo wget https://cdn.downloads.dataiku.com/public/dss/14.3.3/dataiku-dss-14.3.3.tar.gz
```

## 압축 해제

```bash
sudo tar xzf dataiku-dss-14.3.3.tar.gz
sudo chmod 755 -R /opt/dataiku
```

## Dependency 우선 설치

```bash
# 필수 - 최신 패키지 업데이트
sudo dnf update -y

# root 권한으로 종속성 설치
sudo -i "/opt/dataiku/dataiku-dss-14.3.3/scripts/install/install-deps.sh -yes"
```
## <img src="https://img.shields.io/badge/Design-Node-blue?style=flat&logo=architect">
## 설치 - design node

```bash
# Data 디렉토리 생성
sudo mkdir -p /dataiku

# 생성한 디렉토리의 owner를 dataiku 로 전환
sudo chown -R dataiku:dataiku /dataiku
sudo chmod 755 -R /dataiku
cd /dataiku

# design node 설정을 위한 디렉토리 생성
mkdir design

# 설치 디렉토리로 이동
cd /opt/dataiku/dataiku-dss-14.3.3/

# install 파일 확인
ls installer.sh

# 설치 명령어 실행 (~/dataiku-admin-train 에서 다운받은 경우)
./installer.sh -d /dataiku/design -p 10000 -l ~/dataiku-admin-train/license.json
```

## 실행 - design node

```bash
# Dataiku Design Node 시작
/dataiku/design/bin/dss start

# 서비스 상태 확인
/dataiku/design/bin/dss status

# Linux 서버 부팅 시 자동 실행 설정
sudo -i "/opt/dataiku/dataiku-dss-14.3.3/scripts/install/install-boot.sh" "/dataiku/design" dataiku
```

## Design node 접속

- http://<서버IP>:10000
- 혹시 방화벽이 있어 포트 접속이 안되면 아래 명령어 실행

```bash
sudo firewall-cmd --permanent --add-port=10000/tcp
sudo firewall-cmd --permanent --add-port=11000/tcp
sudo firewall-cmd --permanent --add-port=12000/tcp
sudo firewall-cmd --reload
```
- license 적용


## <img src="https://img.shields.io/badge/Automation-Node-orange?style=flat&logo=settings">
## 설치 - automation node

```bash
mkdir /dataiku/automation

cd /opt/dataiku/dataiku-dss-14.3.3
./installer.sh -t automation -d /dataiku/automation -p 11000 -l ~/dataiku-admin-train/license.json
```

## 실행 - automation node

```bash
# Dataiku Automation Node 시작
/dataiku/automation/bin/dss start

# 서비스 상태 확인
/dataiku/automation/bin/dss status

# Linux 서버 부팅 시 자동 실행 설정
sudo -i "/opt/dataiku/dataiku-dss-14.3.3/scripts/install/install-boot.sh" "/dataiku/automation" dataiku
```

## Automation node 접속

- http://<서버IP>:11000
- license 적용


## <img src="https://img.shields.io/badge/API-Node-green?style=flat&logo=lightning">
## 설치 - api node

```bash
mkdir /dataiku/api
cd /opt/dataiku/dataiku-dss-14.3.3/
./installer.sh -t api -d /dataiku/api -p 12000 -l ~/dataiku-admin-train/license.json
```

## 실행 - api node

```bash
# Dataiku API Node 시작
/dataiku/api/bin/dss start

# 서비스 상태 확인
/dataiku/api/bin/dss status

# Linux 서버 부팅 시 자동 실행 설정
sudo -i "/opt/dataiku/dataiku-dss-14.3.3/scripts/install/install-boot.sh" "/dataiku/api" dataiku
```


## <img src="https://img.shields.io/badge/Deployer-Node-6f42c1?style=flat&logo=rocket">
## Infrastructure 연결 실행

### 1. Automation node 연결

#### 1.1. API Key 생성

```bash
/dataiku/automation/bin/dsscli api-key-create --label Key-for-infra --admin true
```

생성된 key 값 복사

#### 1.2. Infrastructure 등록

1. Design node > Local Deployer
2. Deploying Projects > Infrastructures 탭 선택
3. NEW INFRASTRUCTURE 버튼 클릭
4. ID, Automation node URL, Admin Key 입력
5. 정상 연결 확인 - status가 초록색 마크

### 2. API node 연결

#### 2.1. API Key 생성

```bash
/dataiku/api/bin/apinode-admin admin-key-create
```

생성된 key 값 복사

#### 2.2. Infrastructure 등록

1. Design node > Local Deployer
2. Deploying API Services > Infrastructures 탭 선택
3. NEW INFRASTRUCTURE 버튼 클릭
4. Infrastructure ID 입력 후 "ADD" 버튼 클릭
5. 좌측 메뉴에서 API Nodes 탭 선택
6. "ADD AN API NODE" 버튼 클릭
7. URL 입력, ex) http://<서버IP>:12000
8. Key 입력
