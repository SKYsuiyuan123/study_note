# 学习 MongoDB

## 非关系型数据库

- 图像数据库

- 列数据库(hbase)

- xml 数据库

- 键值对(k, v)数据库

- 文档数据库 (MongoDB) 最符合关系数据库

## MongoDB

- 最符合 关系数据库

- 便捷和丰富的查询手段

- 高可用: (主从备份) => 冗余

- 横向扩展。分库分表(sharding) => 分片

- 支持多个存储引擎 wiredtiger(3.0以后), mmap mongodb 提供了一个 ‘ 存储引擎api ’ 供第三方去扩展

`概念：`

- 数据库(database)： 数据库是一个仓库，在仓库中可以存放集合。

- 集合(collection)： 集合类似于数组，在集合中可以存放文档。

- 文档(document)： 文档数据库中的最小单位，我们存储和操作的内容都是文档。每个document 都是独立的。是无模式的。

- document 长度限制，document和json是非常相似的， 大小 最大 16M。无法添加相同的 key

- mongodb 天生就是分布式，可扩展，高性能。。。CAP

- 既然是分布式，存在两个问题： 内存【粒度太多】 和 网络带宽【多而碎】

**memcached 的value 是 1M

redis 的 value 是 512M【string】**

`上限集合 cappend collection 原理`

- 一个集合，它可以做到 size 的上限 和 document 个数的上限。

- bytes maxinum 指定 collection 的大小。

- document maxinum 指定 collection 中的 document 的个数

- 官网链接: [链接地址](https://docs.mongodb.com/manual/core/capped-collections/)
```javascript
    db.createCollection("log", {
        capped : true,
        size : 5242880,
        max : 5000
    })
```

【日志，疲劳度过滤】
千人千面的 crm 系统。【sms 短信， edm 邮件 的时候，三天内不要给这群人发送同样的信息。】 find 和 insert 操作。document为 1W,这样我们就不需要自己去写代码删除。。。队列的模式。

## Bson

1. Bson 能够支持哪些类型
大概能够支持20种数据类型
Double 'double', String 'string', Object 'object', Array 'array', Binary data 'binData', Undefined 'undefined', ObjectId 'objectId', Boolean 'bool', Date 'date', Null 'null', Regular Expression 'regex', DbPointer 'dbPointer', JavaScript 'javascript', Symbol 'symbol'


2. 通过 find() 操作,用 undefined 做为比较条件，会报错。

3. 通过 find() 操作,用 null 做为比较条件，会把 undefined 的值也筛选出来。

4. 使用 type 操作符 find({name: {$type: 6}}) 就可以找到 name等于undefined 的值。使用 type 操作符 find({name: {$type: 10}}) 就可以找到 name等于null 的值。

5. 使用 type 操作符 find({name: {$type: 'undefined'}}) 也可以找到 name等于undefined 的值。使用 type 操作符 find({name: {$type: 'null'}}) 也可以找到 name等于null的值。

6. type 操作符 [见文档](https://docs.mongodb.com/manual/reference/bson-types/)

## ObjectID
`无索引的情况下，我们的数据叫做 heap... 有了主键索引，那么就是一个 BTree...`

ObjectId('xxx') 就是默认的索引，通过下面的方式来将 objectID 做到全局唯一 [见文档](https://docs.mongodb.com/manual/reference/bson-types/)

- 前4个字节是 时间戳

- 再3个字节是 机器的唯一标示码，

- 再两个字节是 进程id

- 最后3个字节是 随机数

## shell

在 mongodb 中，我们有一个测试工具，就是一个 shell, 这个 shell 封装了一个 V8 引擎，可以运行我们的 js 代码

bin 目录下的一个 mongo程序就是一个对 mongodb 进行测试的一个程序

可以使用 外部 js文件的形式执行我们的命令...

1. 使用 load 方法。命令行里： load('/path.js')

2. help 操作。顾名思义就是帮助我们查询 命令： db.help()，如果在没有代码提示的环境下。

    1. 我们不知道 db 下面有哪些方法

    2. 不知道 collection 下面有哪些方法

    3. cusor(游标) 下面有哪些方法。

## 链接远程数据库

```javascript
// a.js
let mongo = new Mongo('192.168.161.136:27017');

let db = mongo.getDB('test');

let collection = db.getCollection('stus');

let list = collection.find();

printjson(list);

// shell
load('C://a.js')
```

`在MongoDB中，数据库和集合都不需要手动创建，当我们创建文档时，如果文档所在的集合或数据库不存在会自动创建数据库和集合。`

`MOngoDB的文档的属性值也可以是一个文档，当一个文档的属性值是一个文档时，我们称这个文档叫做 内嵌文档`

`MongoDB支持直接通过内嵌文档的属性进行查询，如果要查询内嵌文档则可以通过 . 的形式来匹配，如果要通过内嵌文档来对文档进行查询，此时属性名必须使用引号 ''。`

## 基本指令

- 显示当前所有数据库 show dbs 或者 show databases

- 进入指定的数据库 —— use 数据库名。如果数据库名不存在，则创建，如果数据库已存在，则进入。

- 查看当前所在的数据库 db

- 查看当前数据库里的 所有集合 show collections。不显示空数据库。

- 删除数据库 db.dropDatabase()

## 集合的命令

- 显示当前数据库中的所有集合 show collections

- 创建集合 db.集合名.insert({}) 通常，在创建数据时自动创建集合，不需要单独创建。

- 删除集合 db.集合名.drop()

## 数据库的 CRUD （增删改查）操作

`向数据库中插入文档`

- 当我们向集合中插入文档时，如果没有给文档指定 _id 属性，则数据库会自动为文档添加 _id 该属性用来作为文档的唯一标识, _id 我们可以自己指定，如果我们指定了，数据库就不会再添加了，如果自己指定 _id 也必须确保它的唯一性
    ```sql
        -- 向集合中插入一个 或 多个 文档 collection 集合名
        db.<collection>.insert(doc)

        --例如： 向 test 数据库中的 stus 集合中插入一个新的学生对象
        db.stus.insert({_id: 'hell',name: "sky2", age: 24, gender: "男"})
        -- 或
        db.stus.insert([
            {name: "JC", age: 35, gender: "男", isDel: false},
            {name: "blue", age: 40, gender: "男", isDel: false},
            {name: "sky", age: 24, gender: "男", isDel: false}
        ])
    ```

`查询当前集合中的所有文档`

- find() 用来查询集合中所有符合条件的文档，它 可以接收一个对象作为条件参数。不写或者 使用 {} 表示查询集合中所有的文档。 {属性：值} 查询属性是指定值的文档 多个条件表示并且 返回的是一个数组 是默认按照 _id 排序的

- find().count() 用来查询集合中所有符合条件的文档的 个数

- find().limit() 用来 设置显示数据的上限 表示 最多显示多少条数据

- find().skip() 用来 跳过指定数量的数据

- find().skip((页码 - 1) * 每页显示的条数).limit(每页显示的条数) 用来分页 即 分页公式

- find().limit(每页显示的条数).skip((页码 - 1) * 每页显示的条数) 跟上边一样，MongoDB会自动调整 skip 和 limit 的位置

- find({}).sort({属性名: 1/-1}) sort() 可以用来指定文档的排序的规则 1： 升序，-1： 降序，sort 里可以使用 多个 排序规则，写在前边的先比较。

- limit, skip, sort, 可以以任意的顺序调用，MongoDB会自动调整 会先掉用 sort 再掉 skip 最后 limit

- 在查询时，可以在第二个参数的位置来设置查询结果的 投影

- findOne() 用来查询集合中符合条件的第一个文档，返回的是一个 文档对象

- find().pretty() 将找到的数据以格式化的结果显示出来

- 模糊查询 语法： db.集合名.find({字段名: /值/})

- 使用查找操作符 查找指定的属性

    + $eq 查询 等于指定的值 db.集合名.find({字段名: {$eq: 值}})

    + $gt 查询 大于 指定的值 db.集合名.find({字段名: {$gt: 值}})

    + $gte 查询 大于等于 指定的值 db.集合名.find({字段名: {$gte: 值}})

    + $lt 查询 小于 指定的值 db.集合名.find({字段名: {$lt: 值}})

    + $lte 查询 小于等于 指定的值 db.集合名.find({字段名: {$lte: 值}})

    + $ne 查询 不等于 指定的值 db.集合名.find({字段名: {$ne: 值}})

    + $in 查询 等于A,B,C中的任一个 的值 db.集合名.find({字段名: {$in: [值1, 值2, 值3]}})

    + $nin 查询 不等于A,B,C中的任一个 的值 db.集合名.find({字段名: {$nin: [值1, 值2, 值3]}})

    + $size 查询 符合 数组的个数 的值 db.集合名.find({字段名: {$size: 数量}}) 比如：每个歌星都有代表作，并且代表作是数组，查询有 3 个代表作品的歌手：db.singer.find({works: {$size: 3}})

    + $exists 查询 该字段存在或不存在的 数据 db.集合名.find({字段名: {$exists: true | false}})

    + $or 或者 查询 符合 两个条件中的任一个的 数据 db.集合名.find({$or: [{字段名: 值}, {字段名: 值}]})

    + db.numbers.find({num: {$gt: 40, $lt: 50}}) 查询大于40，小于50的值。

    + 多条件查询，并且 db.集合名.find({字段名1: 值, 字段名2: 值})

```sql
    -- 查看文档中的所有数据
    db.<collection>.find()

    --例如： 查询 test 数据库中的 stus 集合中的 全部数据
    db.stus.find()

    --例如： 查询 test 数据库中的 stus 集合中的 全部数据的 name 字段，想显示哪个字段写上哪个字段就可以了
    db.stus.find({}, {name: 1}) -- 默认显示 _id
    db.stus.find({}, {name: 1, _id: 0}) -- 显示要求 不显示 _id

    --例如： 查询 test 数据库中的 stus 集合中的 前两条数据
    db.stus.find().limit(2)
    db.stus.find().skip(2).limit(1)

    --例如： 查询 test 数据库中的 stus 集合中的 没有被标记删除的用户
    db.stus.find({isDel: false})

    --例如： 查询 test 数据库中的 stus 集合中的 全部数据 的个数
    db.stus.find().count()

    -- 按照 _id 来查找数据
    db.stus.find({_id: 'hell'})

    -- 内嵌文档查找
    db.stus.find({'hobby.movies': 'hero'})
```

`练习`

```javascript

// 查询大于 6000 的值
db.numbers.find({num: {$gt: 6000}})

// 查询大于40，小于50的值。
db.numbers.find({num: {$gt: 40, $lt: 50}})

// 查询小于10，或者 大于50的值。
db.numbers.find({$or: [{num: {$lt: 10}}, {num: {$gt: 50}}]})

// 显示前十条数据 即 显示 第一页的数据
db.numbers.find().skip(0).limit(10)

// 显示 第二页的数据
let pageSize = 10;
let PageNum = 2
db.numbers.find().skip((PageNum - 1) * pageSize).limit(pageSize)

```

`修改`

- update(查询条件，新对象) 默认情况下会使用新对象替换旧的对象 默认只修改第一个，可以使用修改的配置选项，修改多个。

- 使用修改操作符 修改指定的属性

    + $set 可以用来修改文档中的指定属性，不存在该属性时则添加

    + $inc 可以用来修改文档中的指定属性，在原来值的基础上加上新的值

    + $unset 删除文档的指定属性

    + $inc 在原来的基础上加 1

    + $push 用于向数组中添加一个新的元素

    + $addToSet 向数组中添加一个新的元素，如果数组中已经存在了该元素，则不会添加。

- updateMany() 同时修改多个符合条件的文档

- updateOne() 修改一个符合条件的文档

- replaceOne() 替换一个文档

```sql
-- 替换
db.stus.update({name: 'sky'}, {age: 25})

-- $set 修改 age 字段的值
db.stus.update({name: 'sky'}, {$set: {age: 24}})

-- 修改 isDel 的值为 true 代表这个用户被删除了
db.stus.update({name: 'JC'}, {$set: {isDel: true}})

-- $push 在数组中添加一个元素，会一直 push
db.stus.update({name: 'JC'}, {$push: {'hobby.movies': 'Interstellar'}})

-- $addToSet 在数组中添加一个新的元素,如果数组中这个元素存在，则不添加
db.stus.update({name: 'blue'}, {$addToSet: {'hobby.movies': 'Interstellar'}})

-- 修改 age 的值，在原来的基础上 加 1
db.stus.update({name: 'JC'}, {$inc: {age: 1}})

-- $unset 删除 gender 属性
db.stus.update({name: 'sky'}, {$unset: {gender: 1}}) -- gender 的值可以随便写
```

`删除`

- remove() 可以根据条件来删除文档，参数传递的条件的方式和 find() 一样， 删除符合条件的所有文档。如果第二个参数是 true 则只删除一个。 remove() 必须传递参数，不然报错。删除集合中的所有文档 使用 remove({})

- deleteOne() 删除一个

- deleteMany() 删除多个

```sql
-- remove() 删除指定的数据
db.stus.remove({_id: 'hell'})
```

- db.集合名.drop() 删除集合，集合的名字 也被删除了。如果数据库中的最后一个集合也被删除掉了，那么数据库也会没有

- 一般数据库中的数据都不会删除，所以删除的方法很少调用。一般会在数据中添加一个字段({isDel: false})，来表示数据是否被删除。

`练习`

```javascript

// 向集合中添加 20000 条数据
let arr = [];
for(let i = 1; i <= 20000; i++) {
    arr.push({num: i});
}

db.numbers.insert(arr);

// 或者： （该方法不推荐，用时较长）
for(let i = 1; i <= 20000; i++) {
    db.numbers.insert({num: i});
}

// 查询大于 6000 的值
db.numbers.find({num: {$gt: 6000}})

// 查询大于40，小于50的值。
db.numbers.find({num: {$gt: 40, $lt: 50}})

// 查询小于10，或者 大于50的值。
db.numbers.find({$or: [{num: {$lt: 10}}, {num: {$gt: 50}}]})

// 显示前十条数据 即 显示 第一页的数据
db.numbers.find().skip(0).limit(10)

// 显示 第二页的数据
let pageSize = 10;
let PageNum = 2
db.numbers.find().skip((PageNum - 1) * pageSize).limit(pageSize)

```

## 文档间的关系

- 一对一 (one to one) 比如： 夫妻。 在 MongoDB 中，可以通过内嵌文档的形式来体现出一对一的关系。

- 一对多 (one to many) 比如： 用户 —— 订单，文章 —— 评论。 可以通过内嵌文档来映射一对多的关系。

- 多对一 (many to one)

- 多对多 (many to many) 比如： 分类 —— 商品，老师 —— 学生。

`练习`

```sql

-- 一对一
db.wifeAndHusband.insert([
    {
        name: '黄蓉',
        husband: {
            name: '郭靖'
        }
    },
    {
        name: '潘金莲',
        husband: {
            name: '武大'
        }
    }
])

-- 一对多
db.users.insert([
    {
        username: 'sky'
    },
    {
        username: 'blue'
    }
])

db.order.insert([
    {
        list: ['苹果', '香蕉', '大鸭梨'],
        user_id: ObjectId('5c3d999faaa582ef16756496')
    },
    {
        list: ['牛肉', '羊肉'],
        user_id: ObjectId('5c3d999faaa582ef16756497')
    }
])

-- 查找 sky 的订单
let id = db.users.findOne({username: 'sky'})._id
db.order.find({user_id: id})

-- 多对多
db.teachers.insert([
    {name: '洪七公'},
    {name: '黄药师'},
    {name: '龟仙人'}
])

db.students.insert([
    {
        name: '郭靖',
        teacher_ids: [
            ObjectId('5c3daa94aaa582ef1675649c'),
            ObjectId('5c3daa94aaa582ef1675649d')
        ]
    },
    {
        name: '孙悟空',
        teacher_ids: [
            ObjectId('5c3daa94aaa582ef1675649c'),
            ObjectId('5c3daa94aaa582ef1675649d'),
            ObjectId('5c3daa94aaa582ef1675649e')
        ]
    }
])

```

## mongoose

- Mongoose 是一个对象文档模型(ODM)库，它对Node原生的MongoDB模块进行了进一步的优化封装，并提供了更多的功能。

- 在大多数情况下，它被用来把结构化的模式应用到一个MongoDB集合，并提供了验证和类型转换等好处。

`新的对象`

- Schema (模式对象) Schema 对象定义约束了数据库中的文档结构

- Model 该对象作为集合中的所有文档的表示，相当于 MongoDB 数据库中的集合 collection

- Document 它表示集合中的具体文档，相当于集合中的一个具体的文档

`mongoose的好处`

- 可以为文档创建一个模式结构(Schema)

- 可以对模型中的对象/文档进行验证

- 数据可以通过类型转换 转换为对象模型

- 可以使用中间件来应用业务逻辑挂钩

- 比Node原生的MongoDB驱动更容易

`先创建 Schema 然后 Model 再然后 Document`

## 使用 mongoose

- 1、下载安装 mongoose
    ```shell
    npm i mongoose
    ```

- 2、在项目中引入 mongoose
    ```javascript
    const mongoose = require('mongoose');
    ```

- 3、链接数据库
    ```javascript
    // 如果 端口号是默认端口号(27017) 则可以省略不写
    mongoose.connect('mongodb://数据库的ip地址:端口号/数据库名', {
        useNewUrlParser: true
    })
    // 监听 MongoDB 数据库的链接状态
    // 在 mongoose 对象中，有一个属性叫做 connection，该对象表示的就是数据库链接。通过监听该对象的状态，可以来监听数据库的链接与断开。

    // 链接
    mongoose.connection.once('open',()=>{})

    // 断开
    mongoose.connection.once('close',()=>{})
    ```

- 4、断开数据库链接（一般不需要调用）。 MongoDB 数据库，一般情况下，只需要链接一次，链接一次以后，除非项目停止，服务器关闭，否则链接不会断开。
    ```javascript
    mongoose.disconnect()
    ```

`Boolean 一般不用`

`Document 和 集合中的文档 一一对应，Document 是 Model 的实例，通过 Model 查询到的结果 都是 Document`

`注意：` `如果 Schema 中没有定义 该字段属性及类型，那么该字段 是不能添加进数据库的。`