# [Issue Note] H2 Database ~ not found, and IFEXISTS=true, so we cant auto-create it [90146-199]


## 이슈

+ 해당 정보를 입력하고 연결정보를 클릭시 아래와 같은 에러가 발생하였다.

![H2DB](/posts/images/dbms/20190625215123.png)

`Database "/Users/riley/test" not found, and IFEXISTS=true, so we cant auto-create it [90146-199] 90146/90146`

## 문제 해결 단계
아래 링크를 통하여 `test` 라는 이름의 새로운 데이터베이스를 생성해줌으로써 해결하였다.
+ [http://www.h2database.com/html/tutorial.html#creating_new_databases](http://www.h2database.com/html/tutorial.html#creating_new_databases)

```bash
# 설치경로로 이동
 cd /Users/riley/Utils/h2/bin
```

이동하면 h2-버전명 jar 파일이 있다.
![h2](/posts/images/dbms/20190625234751.png)

h2 쉘을 아래와 같이 실행해준다.
```bash
java -cp h2-1.4.199.jar org.h2.tools.Shell
```
![h2](/posts/images/dbms/20190625235307.png)

+ `URL` 은 `jdbc:h2:~/test` 로 지정하였다. 
+ `jdbc:h2:~/test` 로 접속 URL 을 설정하였지만 어플리케이션에서 접속시 URL 은 `jdbc:h2:tcp://localhost/~/test` 로 접속해야한다.
