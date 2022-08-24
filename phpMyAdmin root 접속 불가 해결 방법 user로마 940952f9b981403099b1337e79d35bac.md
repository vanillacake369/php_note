# phpMyAdmin root 접속 불가 해결 방법 //user로만 접속가능함...

#1698 – Access denied for user ‘root’@’localhost’

mysqli_real_connect(): (HY000/1698) : Access denied for user ‘root’@’localhost’

MySQL 또는 MariaDB를 설치한 다음 데이터베이스 관리를 하기 위해 phpMyAdmin을 설치 후 웹에서 접속을 하려고 하면 위와 같은 메시지와 함께 로그인 불가 상태가 되는데요.

MySQL 5.7, MariaDB 10.1 이후 버전은 보안상 root 계정은 터미널에서만 접속할 수 있으며 root를 제외한 사용자 계정으로 phpMyAdmin에 접속할 수 있기 때문에 MySQL에서 새로 사용자를 만들고, 권한을 부여해서 root 계정처럼 사용할 수 있습니다.

![phpMyAdmin%20root%20%E1%84%8C%E1%85%A5%E1%86%B8%E1%84%89%E1%85%A9%E1%86%A8%20%E1%84%87%E1%85%AE%E1%86%AF%E1%84%80%E1%85%A1%20%E1%84%92%E1%85%A2%E1%84%80%E1%85%A7%E1%86%AF%20%E1%84%87%E1%85%A1%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8%20user%E1%84%85%E1%85%A9%E1%84%86%E1%85%A1%20940952f9b981403099b1337e79d35bac/phpMyAdmin_denied_root_01.png](phpMyAdmin%20root%20%E1%84%8C%E1%85%A5%E1%86%B8%E1%84%89%E1%85%A9%E1%86%A8%20%E1%84%87%E1%85%AE%E1%86%AF%E1%84%80%E1%85%A1%20%E1%84%92%E1%85%A2%E1%84%80%E1%85%A7%E1%86%AF%20%E1%84%87%E1%85%A1%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8%20user%E1%84%85%E1%85%A9%E1%84%86%E1%85%A1%20940952f9b981403099b1337e79d35bac/phpMyAdmin_denied_root_01.png)

## MySQL 사용자 계정 생성

![phpMyAdmin%20root%20%E1%84%8C%E1%85%A5%E1%86%B8%E1%84%89%E1%85%A9%E1%86%A8%20%E1%84%87%E1%85%AE%E1%86%AF%E1%84%80%E1%85%A1%20%E1%84%92%E1%85%A2%E1%84%80%E1%85%A7%E1%86%AF%20%E1%84%87%E1%85%A1%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8%20user%E1%84%85%E1%85%A9%E1%84%86%E1%85%A1%20940952f9b981403099b1337e79d35bac/phpMyAdmin_denied_root_02.png](phpMyAdmin%20root%20%E1%84%8C%E1%85%A5%E1%86%B8%E1%84%89%E1%85%A9%E1%86%A8%20%E1%84%87%E1%85%AE%E1%86%AF%E1%84%80%E1%85%A1%20%E1%84%92%E1%85%A2%E1%84%80%E1%85%A7%E1%86%AF%20%E1%84%87%E1%85%A1%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8%20user%E1%84%85%E1%85%A9%E1%84%86%E1%85%A1%20940952f9b981403099b1337e79d35bac/phpMyAdmin_denied_root_02.png)

터미널에 접속한 다음 ①`sudo su` 명령어로 슈퍼유저 권한을 획득합니다.

그리고 ②mysql -u root -p를 입력해 root 계정으로 로그인 한 후 다음 명령어를 입력해 새로운 MySQL 계정을 만들고 모든 권한을 부여해 root와 같은 계정을 만들어 줍니다.

```
create user '아이디'@'%' identified by '비밀번호';
grant all privileges on *.* to '아이디'@'%';
```

![phpMyAdmin%20root%20%E1%84%8C%E1%85%A5%E1%86%B8%E1%84%89%E1%85%A9%E1%86%A8%20%E1%84%87%E1%85%AE%E1%86%AF%E1%84%80%E1%85%A1%20%E1%84%92%E1%85%A2%E1%84%80%E1%85%A7%E1%86%AF%20%E1%84%87%E1%85%A1%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8%20user%E1%84%85%E1%85%A9%E1%84%86%E1%85%A1%20940952f9b981403099b1337e79d35bac/phpMyAdmin_denied_root_03.png](phpMyAdmin%20root%20%E1%84%8C%E1%85%A5%E1%86%B8%E1%84%89%E1%85%A9%E1%86%A8%20%E1%84%87%E1%85%AE%E1%86%AF%E1%84%80%E1%85%A1%20%E1%84%92%E1%85%A2%E1%84%80%E1%85%A7%E1%86%AF%20%E1%84%87%E1%85%A1%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8%20user%E1%84%85%E1%85%A9%E1%84%86%E1%85%A1%20940952f9b981403099b1337e79d35bac/phpMyAdmin_denied_root_03.png)

이제 웹 브라우저에서 phpMyAdmin로 이동한 다음 생성한 아이디와 비밀번호를 입력해 로그인할 수 있으며 root 보다는 한단계 낮은 권한으로 사용할 수 있는 것을 확인할 수 있습니다.

```
UPDATE mysql.user SET Grant_priv='Y', Super_Priv='Y' WHERE user='아이디';
FLUSH PRIVILEGES;
```

보안을 포기하고 root와 동등한 권한을 사용하고 싶다면 터미널에서 위 명령어를 추가로 입력하면 되겠습니다.