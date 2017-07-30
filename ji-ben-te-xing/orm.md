## ORM

目前 PhpBoot 提供最基本的 ORM 支持，包括：

## 1. 定义实体

实体对应数据库的表， 实体的属性名和数据库的列名一致。下面是一个典型的实体定义：

```
/**
 * 图书信息
 * @table books
 * @pk id
 */
class Book
{
    /**
     * @var int
     */
    public $id;
    /**
     * 书名
     * @var string
     */
    public $name='';

    /**
     * 简介
     * @var string
     */
    public $brief='';

    /**
     * 图片url
     * @var string[]
     */
    public $pictures=[];
}
```
其中:
* @table 指定表名
* @pk 指定表的主键
* @var 定义列的类型，如果类型为对象或者数组，则保存到数据库是将被序列化为 json

可以看到，ORM 中的实体和接口中的实体很类似，事实上，我们**鼓励在 ORM 和接口中复用实体类**，这样是面向对象编程鼓励的。

## 2. 操作数据库

PhpBoot 提供基本的 ORM 操作方法。

### 2.1. 添加

```
$book = new Book();
$book->name = ...
...

\PhpBoot\model($this->db, $book)->create();
echo $book->id; //获取自增主键的值
```
### 2.1. 修改
$book = new Book();
$book->id = ...
...

\PhpBoot\model($this->db, $book)->update();

### 2.3. 查找

```
$book = \PhpBoot\model($this->db, Book::class)->find($id);
```

或者

```
$books = \PhpBoot\model($this->db, Book::class)->where(['name'=>'PHP'])->get();
```

### 2.4. 删除

```
\PhpBoot\model($this->db, $book)->delete();
```


