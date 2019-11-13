# SQL과 django ORM

## 기본 준비 사항

* https://bit.do/djangoorm에서 csv 파일 다운로드

* django app

  * `django_extensions` 설치

  * `users` app 생성

  * csv 파일에 맞춰 `models.py` 작성 및 migrate

    아래의 명령어를 통해서 실제 쿼리문 확인

    ```bash
    $ python manage.py sqlmigrate users 0001
    ```

* `db.sqlite3` 활용

  * `sqlite3`  실행

    ```bash
    $ ls
    db.sqlite3 manage.py ...
    $ sqlite3 db.sqlite3
    ```

  * csv 파일 data 로드

    ```sqlite
    sqlite > .tables
    auth_group                  django_admin_log
    auth_group_permissions      django_content_type
    auth_permission             django_migrations
    auth_user                   django_session
    auth_user_groups            auth_user_user_permissions  
    users_user
    sqlite > .mode csv
    sqlite > .import users.csv users_user
    sqlite > SELECT COUNT(*) FROM user_users;
    100
    ```

* 확인

  * sqlite3에서 스키마 확인

    ```sqlite
    sqlite > .schema users_user
    CREATE TABLE IF NOT EXISTS "users_user" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "first_name" varchar(10) NOT NULL, "last_name" varchar(10) NOT NULL, "age" integer NOT NULL, "country" varchar(10) NOT NULL, "phone" varchar(15) NOT NULL, "balance" integer NOT NULL);
    ```

    

## 문제

> 아래의 문제들을 sql문과 대응되는 orm을 작성 하세요.

### 기본 CRUD 로직

1. 모든 user 레코드 조회

   1. from users.models import User

   ```python
   # orm
   Users.objects.all()
   ```

      ```sql
   -- sql
   SELECT * FROM users_user;
      ```

2. user 레코드 생성

   ```python
   # orm
   Users.objects.create(first_name='', last_name='', age=, ... )
   ```

   ```sql
   -- sql
   -- .schema users_user 를 이용해서 column 이름 가져올 수 있음
   INSERT INTO users_user (c1,c2,c3,c4,c5,c6) VALUES ('name', 'famname','age','do','tel','num');
   INSERT INTO insert into users_user values ('완상','감',27,'경기도',010-0000-0000,600);
   ```

   * 하나의 레코드를 빼고 작성 후 `NOT NULL` constraint 오류를 orm과 sql에서 모두 확인 해보세요.

3. 해당 user 레코드 조회

   ```python
   # orm
   Users.objects.get(name='유저이름')
   ```

      ```sql
   -- sql
   SELECT * FROM users_user WHERE 'name'='해당유저이름';
      ```

4. 해당 user 레코드 수정

   ```python
   # orm
   Users.objects.
   ```

      ```sql
   -- sql
   UPDATE users_user SET '수정할 column'='수정할값' WHERE 'name' = '해당유저이름';
      ```

5. 해당 user 레코드 삭제

   ```python
   # orm
   ```

      ```sql
   -- sql
   DELETE FROM users_user WHERE 'name'='해당이름';
      ```

### 조건에 따른 쿼리문

1. 전체 인원 수 

   ```python
   # orm
   USer.objects.all()
   ```

      ```sql
   -- sql
   SELECT Count(*) FROM users_user;
      ```

2. 나이가 30인 사람의 이름

   ```python
   # orm
   Users.objects.filter(age=30).values('first_name')
   ```

      ```sql
   -- sql
   SELECT * FROM users_user WHERE 'age' = 30;
      ```

3. 나이가 30살 이상인 사람의 인원 수

   ```python
   # orm
   Users.objects.filter(age__gte=30).count()
   ```

      ```sql
   -- sql
   SELECT Count(*) FROM users_user WHERE 'age' >= 30;
      ```

4. 나이가 30이면서 성이 김씨인 사람의 인원 수

   ```python
   # orm
   Users.objects.filter(age=30, last_name=김).count()
   Users.objects.filter(age=30).filter(last_name=김).count()
   ```

      ```sql
   -- sql
   SELECT * FROM users_user WHERE age = 30, last_name='김';
      ```

5. 지역번호가 02인 사람의 인원 수

   ```python
   # orm
   Users.objects.filter(phone__startswith='02-').count()
   ```

      ```sql
   -- sql
   SELECT Count(*) FROM users_user WHERE phone LIKE '02-%';
      ```

6. 거주 지역이 강원도이면서 성이 황씨인 사람의 이름

   ```python
   # orm
   User.objects.filter(country='', last name='').values('first name')
   ```
```
   
      ```sql
   -- sql
   SELECT first_name FROM users_user WHERE country='강원도', last_name='황' ;
```



### 정렬 및 LIMIT, OFFSET

1. 나이가 많은 사람 10명

   ```python
   # orm
   User.objects.order_by('-age')[:10]
   ```

      ```sql
   -- sql
   SELECT * FROM users_user order by age desc limit 10;
      ```

2. 잔액이 적은 사람 10명

   ```python
   # orm
   User.objects.order_by('balance')[:10]
   ```

      ```sql
   -- sql
   SELECT * from users_user order by balance limit 10;
      ```

3. 성, 이름 내림차순 순으로 5번째 있는 사람

      ```python
   # orm
   User.objects.order_by('-last_name','-first_name')[4]
   ```
```
   
      ```sql
   -- sql
   select * from users_user order by last_name desc, first_name desc limit 5,1;
   select * from users_user order by last_name desc, first_name desc limit 1 offset 4;
```



### 표현식 (aggregate, annotate)

1. 전체 평균 나이

   ```python
   # orm
   from django.db. models import Avg
   User.objects.affrefate(Avg('age'))
   ```

      ```sql
   -- sql
   select AVG(age) from users_user;
      ```

2. 김씨의 평균 나이

   ```python
   # orm
   User.objects.filter(last_name='김').aggregate(Avg('age'))
   ```

      ```sql
   -- sql
   select AVG(age) from users_user where last_name = '김';
      ```

3. 계좌 잔액 중 가장 높은 값

   ```python
   # orm
   from django.db.models import max
   User.objects.aggregate(Max('balance'))
   ```

      ```sql
   -- sql
   select max(balance) from users_user;
      ```

4. 계좌 잔액 총액

      ```python
   # orm
   from django.db.models import SUM
user.objects.aggregate(Sum('balance'))
   ```
   
      ```sql
   -- sql
   select sum(balance) from users_user;
      ```





## 연습시작하기

1. venv 켜고 app시작하고 settings.py에 추가하고 model.py 에 모델 올리고 같은폴더레벨에 csv 파일 올리고

   ```python
   from django.db import models
   
   class User(models.Model):
       first_name = models.CharField(max_length=10)
       last_name = models.CharField(max_length=10)
       age = models.IntegerField()
       country = models.CharField(max_length=10)
       phone = models.CharField(max_length=15)
       balance = models.IntegerField()
   ```

   

2. sqlite3 db.sqlite3

3. .tables 에 users.user

4. csv파일 불러오기

   1. .mode csv
   2. .import users.csv users_user 
   3. SELECT COUNT(*) FROM users_user; 로 100줄 다 올라갔는지 확인하기

5. python manage.py shell_plus

6. 연습시작