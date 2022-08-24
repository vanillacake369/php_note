# 문자열 비교시 !=가 아닌 !==을 써야하는 이유 [php]

![apple-touch-icon@2.png](%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A1%E1%84%8B%E1%85%A7%E1%86%AF%20%E1%84%87%E1%85%B5%E1%84%80%E1%85%AD%E1%84%89%E1%85%B5%20!=%E1%84%80%E1%85%A1%20%E1%84%8B%E1%85%A1%E1%84%82%E1%85%B5%E1%86%AB%20!==%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%8A%E1%85%A5%E1%84%8B%E1%85%A3%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AB%20%E1%84%8B%E1%85%B5%E1%84%8B%E1%85%B2%20%5B%20d0f44af226c744eaa76de433cce9eabb/apple-touch-icon2.png)

`==` and `!=` do not take into account the data type of the variables you compare. So these would all return true:

```
'0'   == 0
false == 0
NULL  == false

```

`===` and `!==` **do** take into account the data type. That means comparing a string to a boolean will **never** be true because they're of different types for example. These will all return false:

```
'0'   === 0
false === 0
NULL  === false

```

You should compare data types for functions that return values that could possibly be of ambiguous truthy/falsy value. A well-known example is `strpos()`:

```
// This returns 0 because F exists as the first character, but as my above example,
// 0 could mean false, so using == or != would return an incorrect result
var_dump(strpos('Foo', 'F') != false);  // bool(false)
var_dump(strpos('Foo', 'F') !== false); // bool(true), it exists so false isn't returned

```

!== should match the value and data type

!= just match the value ignoring the data type

```
$num = '1';
$num2 = 1;

$num == $num2; // returns true
$num === $num2; // returns false because $num is a string and $num2 is an integer

```