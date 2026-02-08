# Lab - Connection 생성

## MySQL driver 다운로드

```bash
#Home directory로 이동
cd ~

# driver download
wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-j-8.4.0.tar.gz
```

## 압축 해제

```bash
tar xzf mysql-connector-j-8.4.0.tar.gz
```

## DSS stop

```bash
/dataiku/design/bin/dss stop
```

## Driver Jar 파일 복사 to Dataiku

```bash
cp ./mysql-connector-j-8.4.0/mysql-connector-j-8.4.0.jar /dataiku/design/lib/jdbc/
```

## DSS start

```bash
/dataiku/design/bin/dss start
```

## 옵션) DSS restart

```bash
/dataiku/design/bin/dss restart
```