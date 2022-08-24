# [PHP] mysqli_real_escape_string 사용법 / PHP 보안

![%5BPHP%5D%20mysqli_real_escape_string%20%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8%20PHP%20%E1%84%87%E1%85%A9%E1%84%8B%E1%85%A1%E1%86%AB%20f77b39cdd0d944a58e1617372bcad9b7/img.png](%5BPHP%5D%20mysqli_real_escape_string%20%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8%20PHP%20%E1%84%87%E1%85%A9%E1%84%8B%E1%85%A1%E1%86%AB%20f77b39cdd0d944a58e1617372bcad9b7/img.png)

### **mysqli_real_escape_string() 이란?**

php에서 제공하는 함수로 MYSQL과 커넥션을할때 String을 Escape한 상태로 만들어준다.

**사용법 :**

**mysqli_real_escape_string***(connection, escapestring);*

- MYSQL 과 연결하는 connection과 escape형태로 만들어줄 string을 입력한다.

**Escape string이란?**

우리가 string을 입력할때 Tom's cat 이란 입력을 하면 '는 sql문에 앞서 있던 ' 와 중첩이 될 수 있다.

이러한 문제를 막기위해 \n, \r \" 처럼 구별해주는 형태로 만들어주는 것을 *Escape string* 이라고 한다.

### **활용예시**

보안을 위해 사용할 수 있다.

만약 Escape하지 않은 소스로 그냥 input을 받는다고 생각해보자.

예를들면 사용자가 입력창에 *'**DROP TABLE** 테이블명'* 같은 위험성 있는 문장을 악의적으로 기입 할 수 있다. 그러면 테이블이 다 날라가버린다. (이를 injection 공격이라고 한다.)

mysqli_real_escape_string 을 통해 그러한 공격을 방지하며 입력받을 수가 있다.

![%5BPHP%5D%20mysqli_real_escape_string%20%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8%20PHP%20%E1%84%87%E1%85%A9%E1%84%8B%E1%85%A1%E1%86%AB%20f77b39cdd0d944a58e1617372bcad9b7/img%201.png](%5BPHP%5D%20mysqli_real_escape_string%20%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8%20PHP%20%E1%84%87%E1%85%A9%E1%84%8B%E1%85%A1%E1%86%AB%20f77b39cdd0d944a58e1617372bcad9b7/img%201.png)

위의 코드는 예시인데,

$_POST 로 넘어오는 인자값들을 직접 받지않고

mysqli_real_escape_string 을 통해 filtering 된 인자로 받아준다.

(참조 : [생활코딩 https://www.youtube.com/watch?v=GdRZhWjTDnE&t=420s )](https://www.youtube.com/watch?v=GdRZhWjTDnE&t=420s)