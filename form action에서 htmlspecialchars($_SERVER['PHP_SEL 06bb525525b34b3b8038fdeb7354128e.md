# form action에서 htmlspecialchars($_SERVER['PHP_SELF']) - 보다 ""을 써야하는 이유

![apple-touch-icon@2.png](form%20action%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5%20htmlspecialchars($_SERVER%5B'PHP_SEL%2006bb525525b34b3b8038fdeb7354128e/apple-touch-icon2.png)

`htmlspecialchars` replaces characters with special meaning in HTML with &-escaped entities. So, for example, `'` becomes `&#039;`. It doesn't turn `%22` into `&quot;`, however, because `%22` has no special meaning in HTML, so it's safe to display it without modification.

If you want a form to be handled by the same URL that is used to display it, always use `action=""` rather than `action=<?=$_SERVER['PHP_SELF']?>` or `action=<?=$_SERVER['REQUEST_URI']?>`.

As you've already figured out, there are serious risks of cross-site scripting (XSS) if you use either of the `$_SERVER` variables, because they contain user input and therefore cannot be trusted. So, unless you have a good reason that you need to tweak the URL somehow, just use `action=""`.