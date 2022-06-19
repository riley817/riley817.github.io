# Doker로 MySQL 설치하기


### docker 다운로드 및 이미지 생성

- docker에서 내려 받을 수 있는 mysql 버전 확인 : [https://hub.docker.com/_/mysql/](https://hub.docker.com/_/mysql/)
- 버전을 명시 하지 않으면 가장 최신 버전을 다운로드하게 됨

```bash
# docker pull
sudo docker pull mysql:8

# docker 이미지 확인
sudo docker images
```

#### docker Mysql 컨테이너 생성 및 실행
```bash
# mysql 컨테이너 생성 및 실행
sudo docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=PASSWORD --name mysql8 -v /usr/riley/datadir:/var/lib/mysql mysql:8 --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci

# 실행된 컨테이너 확인
sudo docker ps -a
```

#### MySql 컨테이너 bash 쉘 접속하여 MySQL 에 접속하기
```bash
# 컨테이너 bash 쉘 접속
sudo docker exec -it mysql8 bash

# mysql 접속
mysql -u root -p
```
#### 데이터베이스의 유저를 생성하고 권한 부여
- `%`  :  모든 접속을 허용.

```sql
mysql> CREATE USER 'riley'@'%' IDENTIFIED BY '패스워드';
Query OK, 0 rows affected (0.01 sec)

mysql> GRANT ALL PRIVILEGES ON *.* TO 'riley'@'%';
Query OK, 0 rows affected (0.00 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)
```
