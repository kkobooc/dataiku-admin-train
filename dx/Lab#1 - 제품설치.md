# 관리자 교육 - 제품 설치

## 설치 파일 download ( 2.0GB, 약 2분)

```bash
wget https://cdn.downloads.dataiku.com/public/dss/13.4.4/dataiku-dss-13.4.4.tar.gz
```

## 압축 해제

```bash
tar xzf dataiku-dss-13.4.4.tar.gz
```

## Dependency 우선 설치

```bash
# 필수 - 최신 패키지 업데이트
sudo apt update && sudo apt upgrade -y

sudo -i "/home/dataiku/dataiku-dss-13.4.4/scripts/install/install-deps.sh"
```

## 설치 - design node

```bash
# Data 디레토리 생성
sudo mkdir -p /data/dataiku 

# 생성한 디렉토리의 owner를 dataiku 로 전환
sudo chown -R dataiku:dataiku /data/dataiku 

cd /data/dataiku

# design node 설정을 위한 디렉토리 생성
mkdir design

# 설치 디렉토리로 이동
cd ~/dataiku-dss-13.4.4/

# intall 파일 확인
ls installer.sh

# 설치 명령어 실행
./installer.sh -d /data/dataiku/design -p 10010
```

## 실행 - design node

```bash
/data/dataiku/design/bin/dss start

# linux 서버 실행 시 dataiku 가 함께 실행되도록 하기 위한 명령
sudo -i "/home/dataiku/dataiku-dss-13.4.4/scripts/install/install-boot.sh" "/data/dataiku/design" dataiku
```

## Design node 접속

- http://localhost:10010
- 혹시 방화벽이 있어 포트 접속이 안되면 아래 명령어 실행

```bash
sudo ufw disable  
```
- license 적용


## 설치 - automation node

```bash
mkdir /data/dataiku/automation
cd ~/dataiku-dss-13.4.4
./installer.sh -t automation -d /data/dataiku/automation -p 10020
```

## 실행 - automation node

```bash
/data/dataiku/automation/bin/dss start
sudo -i "/data/dataiku/dataiku-dss-13.4.4/scripts/install/install-boot.sh" "/data/dataiku/automation" dataiku
```

## Automation node 접속

- http://localhost:10020
- license 적용

## 설치 - api node

```bash
mkdir /data/dataiku/api
cd ~/dataiku-dss-13.4.4/
./installer.sh -t api -d /data/dataiku/api -p 10030 -l ./license.json
```

## 실행 - api node

```bash
/data/dataiku/api/bin/dss start
sudo -i "/data/dataiku/dataiku-dss-13.4.4/scripts/install/install-boot.sh" "/data/dataiku/api" dataiku
```

## 🟨 Infrastructure 연결 실행

1. Automation node 연결
    1. API Key 생성
        
        ```bash
        # api-key create
        /data/dataiku/automation/bin/dsscli api-key-create --label Key-for-infra --admin true
        
        ```
        
        key 값 복사
        
    2. Infrastructure 등록
        1. Design node > Local Deployer
        2. Deploying Projects > Infrastructures 탭 선택
        3. NEW INFRASTRUCTURE 버튼 클릭
        4. ID, Automation node URL, Admin Key 입력 
        5. 정상 연결 확인 - status가 초록색 마크 
2. API node 연결
    1. API Key 생성
        
        ```bash
        # api-key create
        /data/dataiku/api/bin/apinode-admin admin-key-create
        
        ```
        
        key 값 복사
        
    2. Infrastructure 등록
        1. Design node > Local Deployer
        2. Deploying API Services > Infrastructures 탭 선택
        3. NEW INFRASTRUCTURE 버튼 클릭
        4. Infrastructure ID 입력 후 “ADD” button click
        5. 좌측 메뉴에서 API Nodes 탭 선택 
        6. “ADD AN API NODE” 버튼 클릭
        7. URL 입력 , ex) http://api-node:10020
        8. Key 입력
