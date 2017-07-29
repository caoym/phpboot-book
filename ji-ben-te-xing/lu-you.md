# 路由

路由通过 @path 和 @route定义，形似如下：

```
/**
 * @path /books
 */
class Books
{
    /**
     * @route GET /{id}
     */
    public function getBook($id)
}
```

## @path 

**语法：** ```@path <prefix>```

标注在类的注释里，用于指定 Controller 类中所定义的全部接口的uri 的前缀。


## @route

**语法：** ```@path <method> <uri>```

标注在方法的注释里，用于指定接口的路由。method为指定的 http 方法，可以是 GET、HEAD、 POST、PUT、PATCH、DELETE、OPTION。uri 中可以带变量，用{}包围。






