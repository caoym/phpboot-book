# Hook

Hook 即钩子，说明待补充... 以下演示如果定义和使用 Hook

## 1. 定义 Hook

演示如何通过 Hook 实现 Basic Authorization 登录校验
```
/**
 * 简单登录校验
 *
 * 实现了 Basic Authorization
 * @package App\Hooks
 */
class BasicAuth implements HookInterface
{
    /**
     * @param Request $request
     * @param callable $next
     * @return Response
     */
    public function handle(Request $request, callable $next)
    {
        $auth = $request->headers->get('Authorization');
        $auth or \PhpBoot\abort(new UnauthorizedHttpException('Basic realm="PhpBoot Example"', 'Please login...'));
        $auth = explode(' ', $auth);
        $auth[1] == md5("{$this->username}:{$this->password}") or fail(new UnauthorizedHttpException('Basic realm="PhpBoot Example", "Invalid username or password!"'));
        return $next($request);
    }

    /**
     * @var string
     */
    public $username;
    /**
     * @var string
     */
    public $password;
}
```
可以看到，Hook 只需要继承HookInterface，实现 handle 方法。

## 2. 使用 Hook

为需要的接口添加此 Hook

### 2.1. 通过 @hook 添加 Hook

```
/**
 * @route POST /books/
 * @param Book $book {@bind request.request}
 * @hook \App\Hooks\BasicAuth 指定此接口
 */
public function createBook(Book $bok)
```

一个接口可以指定多个 Hook，执行的顺序依照@hook 定义的顺序。

### 2.2. 添加路由时指定 Hook

Application::addRoute()、Application::loadRoutes*() 方法添加路由时，可以指定 Hook ，如：

```
$app->loadRoutesFromPath($path, [BaseAuth::class]);
```

