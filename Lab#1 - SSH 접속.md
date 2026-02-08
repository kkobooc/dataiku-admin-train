# 관리자 교육 - SSH 접속

## 서버 접속 정보

| 서버 | IP | 호스트명 |
|------|-----|----------|
| 서버 1 | 49.247.203.94 | kkobooc-300662 |
| 서버 2 | 49.247.203.245 | kkobooc-300661 |
| 서버 3 | 49.247.203.87 | kkobooc-300660 |
| 서버 4 | 49.247.202.177 | kkobooc-300659 |
| 서버 5 | 49.247.203.247 | kkobooc-300663 |

| 항목 | 값 |
|------|-----|
| 사용자 | dataiku |
| 비밀번호 | (교육 시 안내) |

## SSH 접속 방법

### Windows (PowerShell 또는 CMD)
```bash
ssh dataiku@<호스트IP>
```

### macOS / Linux
```bash
ssh dataiku@<호스트IP>
```

### ⚠️ 주의 사항
- 서버 OS: RHEL 또는 Rocky Linux
- dataiku 계정으로 접속하며, sudo 권한이 부여되어 있습니다.
