# preparedStatement 사용 시, bindParam과 bindValue 차이점 //왠만하면 bindParam사용해야하는 이유!!

[PDO(PHP Data Objects)](http://php.net/manual/en/book.pdo.php)에서 Prepared statements 사용시 값을 bind하기 위해 [PDOStatement::bindParam](http://php.net/manual/en/pdostatement.bindparam.php) 또는 [PDOStatement::bindValue](http://php.net/manual/en/pdostatement.bindvalue.php)를 주로 사용한다. 두개의 함수는 사용법이 거의 유사해서 어떤 차이가 있는지 알아둘 필요가 있다.

아래의 예시를 보면 정확한 차이를 바로 알 수 있다.

```
<?php
$sex = 'male';
$s = $dbh->prepare('SELECT name FROM students WHERE sex = :sex');
$s->bindParam(':sex', $sex); // use bindParam to bind the variable
$sex = 'female';
$s->execute(); // executed with WHERE sex = 'female'
```

```
<?php
$sex = 'male';
$s = $dbh->prepare('SELECT name FROM students WHERE sex = :sex');
$s->bindValue(':sex', $sex); // use bindValue to bind the variable's value
$sex = 'female';
$s->execute(); // executed with WHERE sex = 'male'
```

결론은 [PDOStatement::bindParam](http://php.net/manual/en/pdostatement.bindparam.php)는 변수의 레퍼런스로 바인딩 되므로 [PDOStatement::execute](http://php.net/manual/kr/pdostatement.execute.php)가 호출될 때 값이 반영된다.