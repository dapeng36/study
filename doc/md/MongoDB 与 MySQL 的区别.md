# MongoDB 与 MySQL 的区别



MongoDB 虽说是文档型数据库，但是在学习和使用其语法时发现又与 MySQL 有些相似之处，在此记录点滴日后复习。

## 二、概念区别

| 比较   | MYSQL       | MONGODB                                  |
| ---- | ----------- | ---------------------------------------- |
| 库    | database    | database                                 |
| 表    | table       | collection                               |
| 行    | row         | document                                 |
| 列    | column      | field                                    |
| 索引   | index       | index                                    |
| 表关联  | table joins | [$lookup](https://docs.mongodb.com/manual/reference/operator/aggregation/lookup/#pipe._S_lookup) |
| 主键   | primary key | primary key                              |
| 聚合   | aggregation | aggregation pipeline                     |

## 三、命令区别

| 比较   | MYSQL  | MONGODB |
| ---- | ------ | ------- |
| 服务端  | mysqld | mongod  |
| 客户端  | mysql  | mongo   |

## 四、关键字和函数区别

| MYSQL    | MONGODB                                  |
| -------- | ---------------------------------------- |
| where    | [$match](https://docs.mongodb.com/manual/reference/operator/aggregation/match/#pipe._S_match) |
| group by | [$group](https://docs.mongodb.com/manual/reference/operator/aggregation/group/#pipe._S_group) |
| having   | [$match](https://docs.mongodb.com/manual/reference/operator/aggregation/match/#pipe._S_match) |
| select   | [$project](https://docs.mongodb.com/manual/reference/operator/aggregation/project/#pipe._S_project) |
| order by | [$sort](https://docs.mongodb.com/manual/reference/operator/aggregation/sort/#pipe._S_sort) |
| limit    | [$limit](https://docs.mongodb.com/manual/reference/operator/aggregation/limit/#pipe._S_limit) |
| sum()    | [$sum](https://docs.mongodb.com/manual/reference/operator/aggregation/sum/#grp._S_sum) |
| count()  | [$sum](https://docs.mongodb.com/manual/reference/operator/aggregation/sum/#grp._S_sum) |
| join     | [$lookup](https://docs.mongodb.com/manual/reference/operator/aggregation/lookup/#pipe._S_lookup) |

## 五、语句区别

### 5.1 表结构

#### 5.1.1 创建表/集合

```
db.people.insertOne( {
    user_id: "abc123",
    age: 55,
    status: "A"
 } )
 
相当于 
 
CREATE TABLE people (
    id MEDIUMINT NOT NULL AUTO_INCREMENT,
    user_id Varchar(30),
    age Number,
    status char(1),
    PRIMARY KEY (id)
)

```

#### 5.1.2 新增字段

```
db.people.updateMany(
    { },
    { $set: { join_date: new Date() } }
)

相当于 ALTER TABLE people ADD join_date DATETIME

```

#### 5.1.3 删除字段

```
db.people.updateMany(
    { },
    { $unset: { "join_date": "" } }
)

相当于 ALTER TABLE people DROP COLUMN join_date

```

#### 5.1.4 创建索引

```
db.people.createIndex( { user_id: 1 } )

相当于 CREATE INDEX idx_user_id_asc ON people(user_id)

```

#### 5.1.5 删除表/集合

```
db.people.drop()

相当于 DROP TABLE people

```

### 5.2 新增记录/文档

```
db.people.insertOne(
   { user_id: "bcd001", age: 45, status: "A" }
)

相当于 INSERT INTO people(user_id,age,status) VALUES ("bcd001",45,"A")

```

### 5.3 查询记录/文档

#### 5.3.1 简单查询

```
db.people.find()

相当于 SELECT * FROM people

```

```
db.people.find(
    { },
    { user_id: 1, status: 1 }
)

相当于 SELECT id,user_id,status FROM people

```

```
db.people.find(
    { },
    { user_id: 1, status: 1, _id: 0 }
)

相当于 SELECT user_id, status FROM people

```

#### 5.3.2 条件查询

```
db.people.find(
    { status: "A" }
)

相当于 SELECT * FROM people WHERE status = "A"

```

```
db.people.find(
    { status: "A" },
    { user_id: 1, status: 1, _id: 0 }
)

相当于 SELECT user_id, status FROM people WHERE status = "A"

```

#### 5.3.3 非查询

```
db.people.find(
    { status: { $ne: "A" } }
)

相当于 SELECT * FROM people WHERE status != "A"

```

#### 5.3.4 且查询

```
db.people.find(
    { status: "A",
      age: 50 }
)

相当于 SELECT * FROM people WHERE status = "A" AND age = 50

```

#### 5.3.5 或查询

```
db.people.find(
    { $or: [ { status: "A" } ,
             { age: 50 } ] }
)

相当于 SELECT * FROM people WHERE status = "A" OR age = 50

```

#### 5.3.6 大于查询

```
db.people.find(
    { age: { $gt: 25 } }
)

相当于 SELECT * FROM people WHERE age > 25

```

#### 5.3.7 小于查询

```
db.people.find(
   { age: { $lt: 25 } }
)

相当于 SELECT * FROM people WHERE age < 25

```

#### 5.3.8 范围查询

```
db.people.find(
   { age: { $gt: 25, $lte: 50 } }
)

相当于 SELECT * FROM people WHERE age > 25 AND   age <= 50

```

#### 5.3.9 模糊查询

```
db.people.find( { user_id: /bc/ } ) 或 db.people.find( { user_id: { $regex: /bc/ } } )

相当于 SELECT * FROM people WHERE user_id like "%bc%"

```

```
db.people.find( { user_id: /^bc/ } ) 或 db.people.find( { user_id: { $regex: /^bc/ } } )

相当于 SELECT * FROM people WHERE user_id like "bc%"

```

#### 5.3.10 排序查询

```
db.people.find( { status: "A" } ).sort( { user_id: 1 } )

相当于 SELECT * FROM people WHERE status = "A" ORDER BY user_id ASC

```

```
db.people.find( { status: "A" } ).sort( { user_id: -1 } )

相当于 SELECT * FROM people WHERE status = "A" ORDER BY user_id DESC

```

#### 5.3.11 统计查询

```
db.people.count() 或 db.people.find().count()

相当于 SELECT COUNT(*) FROM people

```

```
db.people.count( { user_id: { $exists: true } } ) 或 db.people.find( { user_id: { $exists: true } } ).count()

相当于 SELECT COUNT(user_id) FROM people

```

```
db.people.count( { age: { $gt: 30 } } ) 或 db.people.find( { age: { $gt: 30 } } ).count()

相当于 SELECT COUNT(*) FROM people WHERE age > 30

```

#### 5.3.12 去重查询

```
db.people.distinct( "status" )

相当于 SELECT DISTINCT(status) FROM people

```

#### 5.3.13 分页查询

```
db.people.findOne() 或 db.people.find().limit(1)

相当于 SELECT * FROM people LIMIT 1

```

```
db.people.find().limit(5).skip(10)

相当于 SELECT * FROM people LIMIT 5 SKIP 10

```

#### 5.3.14 查询计划

```
db.people.find( { status: "A" } ).explain()

相当于 EXPLAIN SELECT * FROM people WHERE status = "A"

```

### 5.4 修改记录/文档

```
db.people.updateMany(
   { age: { $gt: 25 } },
   { $set: { status: "C" } }
);

相当于 UPDATE people SET status = "C" WHERE age > 25;

db.people.updateMany(
   { status: "A" } ,
   { $inc: { age: 3 } }
);

相当于 UPDATE people SET age = age + 3 WHERE status = "A";

```

### 5.5 删除记录/文档

```
db.people.deleteMany( { status: "D" } );

相当于 DELETE FROM people WHERE status = "D";

db.people.deleteMany({});

相当于 DELETE FROM people;

```

## 六、参考资料

- [https://docs.mongodb.com/manual/reference/sql-aggregation-comparison/](https://docs.mongodb.com/manual/reference/sql-aggregation-comparison/) 关键字和函数相关
- [https://docs.mongodb.com/manual/reference/sql-comparison/](https://docs.mongodb.com/manual/reference/sql-comparison/) 语句相关

