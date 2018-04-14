## mongodb
### 数据层级
数据库 -> collection -> document -> 字段

## 安装 mongodb


## mongoose
### 基本概念
- `Schema`： 一种以文件形式存储的数据库模型骨架，不具备数据库的操作能力
- `Model`： 由Schema编译而成的假想（fancy）构造器，具有抽象属性和行为。Model的每一个实例（instance）就是一个document。document可以保存到数据库和从数据库返回。
- `instance`： 由Model创建的实例

### 定义Schema
mongoose中任何任何事物都是从Schema开始的。每一个Schema对应MongoDB中的一个集合（collection）。Schema中定义了集合中文档（document）的样式

**基本类型**：
- String
- Number
- Date
- Buffer
- Boolean
- Mixed
- ObjectId
- Array

```js
var mongoose = require('mongoose');
var Schema = mongoose.Schema;
var blogSchema = new Schema({  
    title:  String,  
    author: String,  
    body:   String,  
    comments: [{ body: String, date: Date }],  
    date: { type: Date, default: Date.now },  
    hidden: Boolean, 
    meta: {    votes: Number,    favs:  Number  }
});
```
如果之后想要在Schema中添加键，可以使用Schema#add方法

### 创造一个model
为了使用schema定义，我们需要转换blogSchema为一个Model

```js
// mongoose.model(modelName, schema)
var BlogModel = mongoose.model('Blog', blogSchema);// 开始吧！
```

### 实例方法
Model的实例是document。实例有很多内置的方法，我们也可以给实例自定义方法
> `<schema>.methods.<method-name> = fn`

```js
var animalSchema = new Schema({ 
    name: String, type: String    
});
animalSchema.methods.findSimilarTypes = function (cb) {
    return this.model('Animal').find({ type: this.type }, cb);
}
```
现在所有的动物实例有findSimilarTypes方法

```js
var AnimalModel = mongoose.model('Animal', animalSechema);
var dog = new AnimalModel({ type: 'dog' });
dog.findSimilarTypes(function (err, dogs) { 
    console.log(dogs); // woof
})
```

### Statics方法
给Model添加一个静态方法
> `<schema>.statics.<method-name> = fn`

```js
animalSchema.statics.findByName = function (name, cb) {
    this.find({ name: new RegExp(name, 'i') }, cb);
}

var AnimalModel = mongoose.model('Animal', animalSchema);
AnimalModel.findByName('fido', function (err, animals) { 
    console.log(animals);
});
```

### 虚拟属性

```js
// 定义一个schema

var personSchema = new Schema({
    name: {  first: String,    last: String  }
});

// 创造实例
var Person = mongoose.model('Person', personSchema);

var xiaoming = new Person({ 
    name: { first: 'Walter', last: 'White' }
});


/* 虚拟属性 */
// getter
personSchema.virtual('name.full').get(function () { 
    return this.name.first + ' ' + this.name.last;
});
// setter
personSchema.virtual('name.full').set(function (name) {  
    var split = name.split(' ');  
    this.name.first = split[0];  
    this.name.last = split[1];
});

// set
mad.name.full = 'Breaking Bad';
console.log(mad.name.first); // Breaking
console.log(mad.name.last);  // Bad
```
### 索引
**参考**：[MongoDB入门之索引篇](https://segmentfault.com/a/1190000004294447)

MongoDB支持二级索引，定义索引有两种方式

```js
var animalSchema = new Schema({  
    name: String,  
    type: String,  
    tags: { type: [String], index: true } // field level
});

animalSchema.index({ name: 1, type: -1 }); // schema level, 1是正序，-1是倒序
```
如果要建立复合索引的话，在schema级别建立是必要的

索引或者复合索引能让搜索更加高效，默认索引就是主键索引ObjectId，属性名为`_id`

**注意**：
- mongodb限制每个集合最多有64个索引。通常，在一个特定的集合上，不应该拥有两个以上的索引

### Schema构造函数配置项options
```js
// 方式1
var schema = new Schema({…}, options)

// 方式2
var schema = new Schema({...});
schema.set(option, value);
```
**配置项**：
- autoIndex（默认true）
- capped
- collection
- id _id（默认true）
- read safe（默认true）
- shardKey strict（默认true）
- toJSON
- toObject
- versionKey
- typeKey
- validateBeforeSave
- skipVersioning
- timestamps
- useNestedStrict
- retainKeyOrder

#### safe
这个配置会在MongoDB所有的操作中起作用。如果设置成`true`就是在操作的时候要等待返回的MongoDB返回的结果，比如update，要返回影响的条数，才往后执行，如果设置为`false`，则表示不用等到结果就向后执行了。
默认设置为true能保证所有的错误能通过我们写的回调函数。我们也能设置其它的安全等级如

```js
# w为写入的大小范围，wtimeout设置写入时限
{ j: 1, w: 2, wtimeout: 10000 }
```
表示如果10秒内写操作没有完成，将会超时

#### toObject
用来表示在提取数据的时候, 把documents 内容转化为Object内容输出. 一般而言只需要设置getters为true即可
```js
schema.set('toObject', { getters: true });
var M = mongoose.model('Person', schema);
var m = new M({ name: 'Max Headroom' });
//实际打印出来的就是一个Object类型
console.log(m); // { _id: 504e0cd7dd992d9be2f20b6f, name: 'Max Headroom is my name' }
```

#### toJSON
该是和toObject一样的使用. 通常用来把 documents 转化为Object. 但是, 需要显示使用toJSON()方法


#### autoIndex–自动索引
应用开始的时候，Mongoose对每一个索引发送一个ensureIndex的命令。索引默认（_id）被Mongoose创建。

当我们不需要设置索引的时候，就可以通过设置这个选项
```js
var schema = new Schema({..}, { autoIndex: false });
var Clock = mongoose.model('Clock', schema);
Clock.ensureIndexes(callback);
```

#### bufferCommands

#### capped–上限设置
如果有数据库的批量操作，该属性能限制一次操作的量

```js
new Schema({...},{capped:1024}); //一次操作上线1024条数据

// 该参数也可是对象，包含size、max、autiIndexId属性
new Schema({...},{capped:{size:1024,max:100,autoIndexId:true}});
```

#### collection–集合名字
在MongDB中默认使用Model的名字作为集合的名字，如过需要自定义集合的名字，可以通过设置这个选项

```js
var schema = new Schema({...}, {collection: 'yourName'});
```

#### id
mongoose分配给每一个schema一个虚拟属性id，它是一个getter。返回的是_id转换为字符串后的值。如果不需要为schema添加这个getter，可以通过id配置修改

```js
// 默认行为
var pageSchema = new Schema({ name: String });
var pageModel = mongoose.model('Page', pageSchema);
var p = new pageModel({ name: 'mongodb.org' });
console.log(p.id); // '50341373e894ad16347efe01'

// 禁止id
var pageSchema = new Schema({ name: String }, { id: false } );
var pageModel = mongoose.model('Page', pageSchema);
var p = new pageModel({ name: 'mongodb.org' });
console.log(p.id); // undefined
```

#### _id
在一个schema中如果没有定义_id域（field），那么mongoose将会默认分配一个_id域（field）。类型是ObjectId。如果不需要使用这个默认的选择，可以通过设置这个选项

通过在schema中设置这个字段可以阻止生成mongoose获得_id。但是在插入的时候仍然会生成_id。设置这个字段之后，如果再使用Schema.set(’_id’, false)将无效

```js
// 默认行为
var pageSchema = new Schema({ name: String });
var pageModel = mongoose.model('Page', pageSchema);
var p = new pageModel({ name: 'mongodb.org' });
console.log(p); // { _id: '50341373e894ad16347efe01', name: 'mongodb.org' }

// 禁用 _id
var pageSchema = new Schema({ name: String }, { _id: false });
// schema构造器设置之后，不要再像下面这样设置
// var schema = new Schema({ name: String });
// schema.set('_id', false);

var PageModel = mongoose.model('Page', pageSchema);
var p = new pageModel({ name: 'mongodb.org' });
console.log(p); // { name: 'mongodb.org' }
// 当插入的时候，MongoDB将会创建_id
p.save(function (err) {  
    if (err) return handleError(err);  
    pageModel.findById(p, function (err, doc) { 
        if (err) return handleError(err);   
        console.log(doc); 
        // { name: 'mongodb.org', _id: '50341373e894ad16347efe12' }  
    })
})
```

#### read
允许在schema级别设置query#read，对于所有的查询，提供给我们一种方法应用默认的ReadPreferences

#### shardKey
需要mongodb做分布式，才会使用该属性

#### strict

### validation
#### required
表示必填字段

```js
new Schema({
 name: {
    type:String,
    required:[true,"name 是必须的"] //第二个参数是错误提示信息
 }
})
```

#### min, max
Number类型的数据限制范围

```js
var breakfastSchema = new Schema({
  eggs: {
    type: Number,
    min: [6, 'Too few eggs'],
    max: 12
  }
});
```

#### enum,match,maxlength,minlength
string类型的校验
- `enum`： 枚举，值的可选项
- `match`：匹配正则表达式
- `maxlength/minlength`：字符串的长度

```js
new Schema({
    drink: {
        type: String,
        enum: ['Coffee', 'Tea']
      },
     food:{
        type: String,
        match:/^a/,
        maxlength:12,
        minlength:6
    }
})
```

#### custom validation
```js
new Schema({
 phone: {
        type: String,
        validate: {
          validator: function(data) {
            return /\d{3}-\d{3}-\d{4}/.test(data);
          },
          message: '{VALUE} is not a valid phone number!' //VALUE代表phone存放的值
        },
        required: [true, 'User phone number required']
      }
})
```

#### 添加额外的验证

##### 步骤一：设置validation
```js
var toySchema = new Schema({
      color: String,
      name: String
    });

var validator = function (value) {
  return /blue|green|white|red|orange|periwinkle/i.test(value);
};
toySchema
.path('color')
.validate(validator, 'Color `{VALUE}` not valid', 'Invalid color');
```

##### 步骤二：调用
- 内置调用，当使用save方法时，会首先执行一次存储方法

```js
cat.save(function(error) {
//自动执行,validation
});
```

- 手动验证validate方法

```js
//上面已经设置好user的字段内容.
  user.validate(function(error) {
    //error 就是验证不通过返回的错误信息
     assert.equal(error.errors['phone'].message,
        '555.0123 is not a valid phone number!');
    });
});
```
### 子文档

```js
var childSchema = new Schema({ name: 'string' });

var parentSchema = new Schema({
  children: [childSchema]   //指明sub-doc的schema
});
//在创建中指明doc
var Parent = mongoose.model('Parent', parentSchema);
var parent = new Parent({ children: [{ name: 'Matt' }, { name: 'Sarah' }] })
parent.children[0].name = 'Matthew';
parent.save(callback);
```

### Model的方法
```js
User.find({age: {$gte: 21, $lte: 65}}, callback);
//等价于:
User.where('age').gte(21).lte(65).exec(callback);
```
**快捷函数**：
name | effect
---|---
select | 添加需要显示的字段,需要的字段在字段后加上`1`,不需要的加上`0`。`query.select({ a: 1, b: 0 })` 不能和`distinct`方法一起使用
distinct | 用来筛选不重复的值或者字段`distinct(field)`
$lt,$lte,$gt,$gte |	分别对应: `<`, `<=`, `>`, `>=`该字段是用在condition中的。如果,你想要链式调用,则需要使用`lt`,`lte`,`ge`,`gte`，`model.find({num:{$gt:12}},cb)` `model.where('num').gt(12).exec(cb)`
$in	| 查询包含键值的文档,`model.find({name:{$in:['jimmy','sam']}})`, 相当于匹配jimmy或者sam
$nin |	返回不匹配查询条件文档,都是指定数组类型`model.find({name:{$nin:['jimmy','sam']}})`
$ne	| 表示不包含指定值`model.find({name:{$ne:'sam'}})`
$or	| 表示或查询`model.find({$or:[{ color: 'red' }, { status: 'emergency' }]})`
$exits	| 表示键值是否存在`model.find({name:{$exits:true}})`
$all	| 通常用来匹配数组里面的键值,匹配多个值(同时具有)`$all:['apple','banana','peach']}`
$size	| 用来查询数组的长度值`model.find({name:{$size:3}})`, 匹配name的数组长度为3
$slice	| 用来获取数组字段的内容`query.slice('comments', 5)`

**游标集合的处理方法**： 

- **limit**：  
用来限定长度的内容
```js
query.limit(20) // 只返回前20个内容
```

- **skip**：  
返回指定下标后的数据
```js
query.skip(2)
```

- **sort**：  
根据指定字段排序，1为升序，-1为降序
```js
query.sort({name: 1, age: -1})
```

### population
**参考**：[mongoose 之 population 关联查询](https://segmentfault.com/a/1190000009690117)

- - - 
**参考**：
- [深入浅出mongoose](https://www.villainhr.com/page/2016/05/11/%E6%B7%B1%E5%85%A5%E6%B5%85%E5%87%BAmongoose)
- [Mongodb 与 Mongoose 的使用](https://github.com/alsotang/node-lessons/tree/master/lesson15)
- [mongoose学习笔记（超详细）](https://segmentfault.com/a/1190000010688972)