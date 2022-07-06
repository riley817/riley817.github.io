# docker를 사용하여 oracle 12c 세팅하기


> 기록쓰 기록쓰 👻👻👻👻

### Oracle 12c 공식 이미지 다운로드
- https://hub.docker.com/_/oracle-database-enterprise-edition?tab=resources
- 공식 이미지를 다운로드 후 `Preceed to Checkout` 버튼을 클릭하여 동의 정보를 입력한다.
![docker-oracle-1](/categories/images/docker-kubernetes/20210821172214.png)

대충 입력쓰...
![docker-oracle-1](/categories/images/docker-kubernetes/20210812172550.png)

### 도커 이미지 다운로드 후 실행
- 8080은 많이 사용하니까 각각 8282, 1522로 매핑하였다.
```bash
# docker image pull
docker pull store/oracle/database-enterprise:12.2.0.1

# docker run
docker run -d -p 8282:8080 -p 1522:1521 --name oracle12c store/oracle/database-enterprise:12.2.0.1
```

### 유저 생성 및 권한 부여
#### sysdba로 sqlplus 접속
```shell
docker exec -it oracle12c bash -c "source /home/oracle/.bashrc; sqlplus sys/Oradoc_db1@ORCLCDB as sysdba"
```

#### 사용할 유저 생성
- bluetape이라는 유저 생성후 "afternoon" 지정
```sql
CREATE USER bluetape IDENTIFIED BY afternoon;
```

#### ORA-65096: invalid common user or role name
- Oracle 12c에서는 사용자 생성 방식이 조금 변경되었다. 이전 생성 스크립트로 생성시 **ORA-65096: invalid common user or role name** 가 발생한다.
![docker-oracle-3](/categories/images/docker-kubernetes/20210812133521.png)
  
- 해결방안 1 : 계정앞에 `C##`키워드를 붙여 준다.
    ```sql
    CREATE USER c##bluetape IDENTIFIED BY afternoon;
    ```
- 해결방안 2 : 현재 세션에 설정 변경
    ```sql
    ALTER SESSION SET "_ORACLE_SCRIPT" = TRUE;
    ```





