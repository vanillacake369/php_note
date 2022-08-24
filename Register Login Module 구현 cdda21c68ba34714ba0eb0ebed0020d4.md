# Register / Login Module 구현

[PHP: HTML 폼 (GET, POST)](https://pronist.tistory.com/37)

**HTTP 요청 메서드**는 다양한 것이 있는데, **폼은 GET, POST 요청을 지원**한다. 최근 **Restful** 등 **자원 관점의 API(Application Programming Interface)** 설계가 늘어나면서, 더 많은 요청을 할 수 있게 되었지만, 폼에서 할 수 있는 요청은 가장 기초적인 것이다.

```php
<form action="/login.php" method="POST">
    <input type="text" name="id">
    <input type="password" name="password">
    <input type="submit">
</form>
```

**한 가지 주의해야 할 점은, 그 어떠한 데이터든 외부에서 오는 데이터는 믿으면 안 된다.**
 사용자가 프로그래머가 예상할 수 있는 데이터만을 넣을 것이라는 생각은 해킹을 당할 수 있는 지름길이다. 따라서 **폼을 포함한 HTTP 요청으로 받을 수 있는 데이터는 모두 임의의 규칙에 따라 검증과정을 거쳐야 한다.**

PHP의 **슈퍼 글로벌 변수** 중에는 **GET, POST** 를 담고있는 변수가 있다. 요청과 관련된 변수에는 `$_COOKIE, $_REQUEST` 등도 있다. `$_GET` 배열에는 **GET** 요청에 대한 파라매터가, `$_POST` 에는 **POST** 요청에 대한 파라매터가 담겨있는데, 즉 다음과 같이 온다고 볼 수 있다.

```php
$_POST = [
    'id' => '__USER_TYPED_DATA__',
    'password' => '__USER_TYPED_DATA__'
];

[ 'id' => $id, 'password' => $password ] = $_POST;

```

이렇게 날라온 데이터는 **Raw**, 말 그대로 날것이라 그대로 사용하면 보안에 위험하다. 레거시 프로젝트에는 **SQL Query** 에 날라온 데이터를 직접 대입하는 놀라운 코드도 많으므로 각별한 주의가 필요하다.

**모든 외부 데이타는 항상 필러링해야만 한다.** 이에 따라 유저데이터 필터링 과정을 거친다.

- 외부 데이터란?
    - 폼으로부터의 입력 데이타
    - Cookies
    - 웹서비스 데이타
    - 서버 변수
    - 데이타베이스 쿼리 결과

PHP filter 는 안전하지 않은 소스로부터의 데이타를 유효성 검사하여 필터링하는데 사용된다.

입력 또는 사용자 정의 데이타를 시험하고, 유효성 검사하고, 필터링하는 것은 웹프로그래밍의 중요한 부분이다.

PHP filter 확장은 데이타 필터링을 더 쉽고 빠르게 할 수 있도록 설계되었습니다.

- filter_var() - 지정된 필터로 **하나의 변수**를 필터링
- filter_var_array() - 동일한 또는 서로 다른 필터로 **여러 변수**를 필터링.
- filter_input() - **하나의 입력변수**를 가져와서 필터링
- filter_input_array() - **여러 입력변수**를 가져와서 동일한 또는 서로 다른 필터로 필터링

## [XSS(Cross-Site Scripting)](https://pronist.tistory.com/37#XSS-Cross-Site%--Scripting-)

**XSS** 공격은 간단히 말해 게시판이나 덧글과 같이 **사용자가 직접 입력할 수 있는 공간에 자바스크립트와 같은 코드를 주입하여 웹 사이트의 취약점을 공격하고 해킹을 시도하는 방법**이다. 보안에서 기본적으로 지켜줘야 할 사항이기도 하고, 아주 유명한 공격이기 때문에 이는 반드시 시켜야 한다.