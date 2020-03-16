# MySQL, MariaDB 처음 시작하기

환경 : MacOS 10.13.6 ( High Sierra ), MariaDB 10.4

## 설치

```bash
$ brew install mariadb
```

- 버전 정보 명시할 경우 `brew install mariadb@10.4` 등과 같이 입력하면 된다.

## 시작하기

```bash
$ mysql.server start
```

## 접속하기

```bash
$ mysql -u [username]
```

- 기본적으로 비밀번호는 입력되어 있지 않기 때문에 `mysql -uroot -p` 이후 `Enter Password:` 칸에서 바로 엔터를 입력하면 접속된다.

- root로 접속이 되지 않을 경우 1)사용자명으로 접속해보거나(나의 경우 `mysql -u leonardkim -p`) 2)`sudo`로 접속하는 방법, 3) `mysqld_safe --skip-grant-table &`을 이용해 우회하는 방법 등이 있다.

## 새로운 유저 생성하기, 권한 설정하기

```sql
create user 'username'@'host';
```

MariaDB에 접속 후 username, host를 특정하여 유저를 생성할 수 있다.

```sql
grant all privileges on *.* to 'username'@'host';
```

위 명령어로 이 계정에 대해 모든 권한을 줄 수 있다. 특정 테이블에 대한 권한을 주려면 `*.*`를 `[database].[table]`로 수정하면 되고, 특정 권한만을 주려면 `all privileges` 부분을 수정하면 된다.

