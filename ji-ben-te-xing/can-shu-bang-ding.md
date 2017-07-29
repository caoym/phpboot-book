# 参数绑定

实现接口时，通常需要从 http 请求中提取数据，作为方法的输入参数，并将方法的返回值转换成 http 的输出。参数绑定功能即可以帮你完成上述工作。

## 输入绑定

### 根据函数定义绑定


默认情况下，框架会从http请求中提取和方法的参数名同名的变量，作为函数的参数。比如：

```
/**
 * @route GET /books/
 */
public function getBooks($offsit, $limit)

```
上述代码，对应的 http 请求形式为 ```GET /books/?offsit=0&limit=10```。在此默认请求下：

* 如果路由 uri 中定义了变量，参数将优先选取 uri 变量。如：

 ```
 /**
  * @route GET /books/{id}
  */
 public function getBook($id)
 ```
 其中 $id 取自 uri。
 
* 对于没有 BODY 的 http 请求（GET、HEAD、OPTION、DELETE），参数来自 querystring 。

* 其他请求（POST、PUT、OPTION），参数先取 querystring，如果没有，再取 BODY。

### @param

如果在方法的注释中，标注了 @param，就会有用 @param 的绑定信息覆盖默认来自函数定义的绑定信息。@param 可以指定变量的类型，而原函数定义中只能在参数是数组或者对象时才能指定类型。@param 的语法为标准 PHP Document 的语法。

```
/**
 * @route GET /books/
 * @param int $offsit
 * @param int $limit
 */
public function getBooks($offsit, $limit)

```
以上代码，除了绑定变量外，还指定了变量类型，即如果输入值无法转换成 int，将返回 400 BadRequest 错误。未指定@param 时，参数的类型默认为 mixed。

### 参数默认值

如果想指定某个输入参数可选，只需给方法参数设置一个默认值。比如:

```
/**
 * @route GET /books/
 * @param int $offsit
 * @param int $limit
 */
public function getBooks($offsit=0, $limit=10)
```
**注意：php 方法的默认参数, 必须放在方法的最后**

## 输出绑定

## 绑定return

默认情况下，函数的返回值将 jsonencode 后，作为 body 输出。如

```
/**
 * @route GET /books/{id}
 */
public function getBook($id)
{
    return ['name'=>'PhpBook', 'desc'=>'PhpBook Document'];
}
```
curl 请求将得到以下结果

```
$ curl "http://localhost/books/1"
{
    "name": "PhpBook",
    "desc": "PhpBook Document"
}
```

**注意，这里为便于演示，直接在方法中返回了数组（其实这在其他语言里算对象），但你应该为这种返回定义一个类，首先，有很多改善代码质量的理由鼓励使用对象替代这类数组，其次在自动生成文档时，这类数组无发被结构化描述。**

## 绑定引用参数

如果函数的参数是引用类型，则这个参数将不会从请求中获取，而是作为输出。比如：

```
/**
 * @route GET /books/
 * @param int $offsit
 * @param int $limit
 * @return Books[] {@bind response.books}
 */
public function getBooks($offsit=0, $limit=10, &$total)
{
    $total = 1;
    return [new Books()];
}

```
curl 请求将得到以下结果

```
$ curl "http://localhost/books"
{
    "total": 1,
    "books": [
        {
            "name":null, 
            "desc":null
        }
    ]
}
```

可以看到，$total 输出到了 http body 中。 
 
## @bind

通过@bind，可以将输入输出绑定到默认位置外其他其他位置，如:

```
/**
 * @route GET /books/
 * @return Books[] {@bind response.books}
 */
public function getBooks($offsit=0, $limit=10, &$total)
```
表示将返回绑定到响应 body 的 books 变量（响应默认是 json）。


### 绑定输入

* **请求Body:** request.request
* **Query String:** request.query
* **Cookie：** request.cookies
* **请求Header：**request.headers
* **文件：**request.files

### 绑定输出

* **响应Body:** response.content
* **Cookie：** response.cookies
* **请求Header：**response.headers

