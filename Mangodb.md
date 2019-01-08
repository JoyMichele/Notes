### 1. 数据库类型

​	Mongo是非关系型数据库 , 文件型数据库

### 2. 与mysql的对比

| SQL术语/概念 | MongoDB术语/概念 | 解释/说明                           |
| ------------ | ---------------- | ----------------------------------- |
| database     | database         | 数据库                              |
| table        | collection       | 数据库表/集合                       |
| row          | document         | 数据记录行/文档                     |
| column       | field            | 数据字段/域                         |
| index        | index            | 索引                                |
| table joins  |                  | 表连接,MongoDB不支持                |
| primary key  | primary key      | 主键,MongoDB自动将_id字段设置为主键 |

### 3. 常用语句

1. 基本语句

   ```sql
   use db // 使用数据库db
   show dbs // 查看当前服务器中鞋子磁盘上的数据库
   show tables // 查看数据库中的collection
   db // 查看当前使用的数据库
   ```

2. 增删改查

  增 : 

  ```sql
  db.collection.insert({ fileld :value }) 
  // 自动生成_id字段, _id字段的值是ObjectId('5b151f8536409809ab2e6b26')
  // '5b151f8536409809ab2e6b26'为十六进制数
  // "5b151f85" 指的是时间戳, 这条数据的产生时间
  // "364098" 代指某台机器的机器码, 存储这条数据时的机器编号
  // "09ab" 代指进程ID, 多进程存储数据的时候, 非常有用的
  // "2e6b26" 代指计数器, 这里要注意的是, 计数器的数字可能会出现重复, 不是唯一的
  
  -- insert 官方已经不推荐使用, 推荐使用insertOne和insertMany
  db.collection.insertOne({ fileld :value }) // 插入一条数据
  db.collection.insertMany([{ fileld :value },{ fileld1 :value1 }...]) //插入多条数据
  ```

  查 : 

```sql
db.collection.find({条件})
db.collection.findOne({条件})
```

  改 : 

```sql
db.collection.update({条件},{$修改器:{fileld :value}})
-- update 官方已经不推荐使用, 推荐使用updateOne和updateMany
db.collection.updateOne({条件},{$修改器:{fileld :value}}) // 更新一条
db.collection.updateMany({条件},{$修改器:{fileld :value}}) // 更新多条
```

  删 : 

```sql
db.collection.remove({条件})
-- 官方推荐下列写法
db.collection.deleteOne({条件}) // 删除一条数据
db.collection.deleteMany({条件}) // 删除所有符合条件的数据
```

  清除collection : 

```sql
db.collection.drop()
```

3. $修改器

```sql
$set // 简单粗暴直接强制修改 
	{name:value} dict["name"]=value 

$unset // 简单粗暴的删除字段 
	{$unset:{name:1}} del dict["name"]
	db.user_info.updateOne({age:200},{$unset:{age:1}})

$inc // 引用增加
	db.user_info.updateMany({},{$inc:{age:1}})

array 操作 :
	$push // 在array中追加一个新的元素 
		[].append(item)
		db.user_info.updateOne({name:"hello"},{$push:{hobby:10}})

	$pull // 在array中删除一个的元素 
		[].remove(item) [].pop(-1)
		db.user_info.updateOne({name:"hello"},{$pull:{hobby:0}})

	$pop // -1 :删除第一个  1 :删除最后一个
		db.user_info.updateOne({name:"hello"},{$pop:{hobby:1}})
```

4. $字符

```sql
   db.user_info.updateOne({hobby:6},{$set:{"hobby.$":"六"}})
   // 保存符合索引条件数据的下标
```

5. Object 字典操作

```sql
db.user_info.updateOne({name:"hello"},{$inc:{"info.age":-5}})
// db.user_info.updateOne({name:"hello"},{$set:{"info.age":12.5}})
```

6. array + Object

```sql
db.user_info.updateOne({"hobby.hight":150},{$set:{"hobby.$.age":14}})
```

7. limit 

```sql
db.user_info.find({}).limit(5)
// 选取数据从当前位置选择5个
```

8. skip 跳过

```sql
db.user_info.find({}).skip(2) 
// 从0开始跳过2条数据为当前位置
```

9. sort

```sql
db.user_info.find({}).sort({ id:-1 })
// 根据ID进行排序 -1倒叙 1正序
```

10. limit+skip+sort

```sql
db.user_info.find({}).limit(5).skip(10)
db.user_info.find({}).limit(c).skip((p-1)*c)
db.user_info.find({}).limit(5).skip(5).sort({ id:-1 })
优先级最高的是 sort 
其次优先为 skip
最低优先级 limit
```

