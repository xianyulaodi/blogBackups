---
title: mongoose的一些操作命令
date: 2017-05-06 11:50
categories:
tags:
     - mongoose
     - node
---

&ensp;&ensp;&ensp;&ensp;最近在用express+mongoose写一个网站，因为经常要用到mongoose的一些操作命令，经常去查还挺麻烦的，所以总结一篇mongoose的使用以及一些操作命令。mongoose里面有三个概念，schemal、model、entity。先来对其三者做个小小的总结概括。
`Schema` ： 一种以文件形式存储的数据库模型骨架，不具备数据库的操作能力
`Model` ： 由Schema发布生成的模型，具有抽象属性和行为的数据库操作对
`Entity` ： 由Model创建的实体，他的操作也会影响数据库
<!-- more -->
&ensp;&ensp;&ensp;&ensp;Schema、Model、Entity的关系请牢记，Schema生成Model，Model创造Entity，Model和Entity都可对数据库操作造成影响，但Model比Entity更具操作性。
&ensp;&ensp;&ensp;&ensp;你可以使用Model来创建Entity，Entity实体是一个特有Model具体对象，但是他并不具备Model的方法，只能用自己的方法。



 ## 1.Schema
schema是mongoose里会用到的一种数据模式，可以理解为表结构的定义；
每个schema会映射到mongodb中的一个collection，它不具备操作数据库的能力.
仅仅只是一段代码，无法通往数据库端, 仅仅只是数据库模型在程序片段中的一种表现

定义Schema
```javascript
var mongoose = require('./db.js'),

var UserSchema = new mongoose.Schema({          
    username : { type: String },                    
    userpwd: {type: String},                        
    userage: {type: Number},                       
    logindate : { type: Date}                      
});
```

** Schema Types的内置类型 **

* `String`
* `Number`
* `Boolean | Bool`
* `Array`
* `Buffer`
* `Date`
* `ObjectId | Oid`
* `Mixed`

## 2. Model模型

Model模型，是经过Schema构造来的，是Schema的编译版本。一个model的实例直接映射为数据库中的一个文档。基于这种关系， 以后的增删改查（CURD）都要通过这个Model实现。

下面我们来定义我们的model
```javascript
var mongoose = require('./db.js');
var ObjectId = mongoose.Schema.ObjectId;
var UserSchema = new mongoose.Schema({          
    name : { type: String },                                       
    age: {type: Number},   
    updated:{type:new Date},                  
    binary:{ type:Buffer },
    _id : { type:ObjectId },
    mixed : { type: Mixed },    
    isMerried :{ type : Boolean }                                      
});
module.exports = mongoose.model('User',UserSchema);
```
定义好model之后，就可以往里面进行一些增删查改的操作了。

## 3. model - 文档操作
** 增加数据 **
如果是Entity，使用save方法，如果是Model，使用create方法
`Model.create(data, callback))`
```javascript
User.create(data,function(err, doc) {
      if (err) return console.log(err);
      console.log(doc);
  });
```

** 查询 **
`model.find({}, callback)`; 参数1忽略,或为空对象则返回所有集合文档
`model.find({},field,callback)`;  过滤查询,参数2: {'name':1, 'age':0} 查询文档的返回结果包含name , field的值中,1为包括，0为不包括
`model.find({},null,{limit:20})`; 过滤查询,参数3: 游标操作 limit限制返回结果数量为20个,如不足20个则返回所有.
`model.findOne({}, callback)`; 查询找到的第一个文档
`model.findById('obj._i', callback)`; 查询找到的第一个文档,同上. 但是只接 `_id` 的值查询
```javascript
//find
User.find({},function(err, data){
    if (err) console.log(err);
    console.log(data);
})
//findOne
User.findOne({name: '张三'}, function(err, data){
    if (err) console.log(err);
    console.log(data);
})
//findByID 与 findOne 相同，但它接收文档的 _id 作为参数，返回单个文档。_id //可以是字符串或 ObjectId 对象。
User.findById(id, function(err, data){
    if (err) consoel.log(err);
    console.log(data);
});
```



** 更新 **
`Model.update(conditions, data, [options], [callback])`
conditions 更新条件
data 更新内容
option 更新选项
&ensp;&ensp;`safe (boolean)` 安全模式，默认选项，值为true
&ensp;&ensp;`upsert (boolean`) 条件不匹配时是否创建新文档，默认值为false
&ensp;&ensp;`multi (boolean)` 是否更新多个文件，默认值为false
&ensp;&ensp;`strict (boolean)` 严格模式，只更新一条数据
&ensp;&ensp;`overwrite (boolean)` 覆盖数据，默认为false
callback回调
```javascript
User.update({name: '张三'}, {age: '6'}, {multi : true}, function(err, numberAffected, raw){
    if (err) return console.log(err);
});
```

** 删除 **
`Model.remove(conditions,callback);`
参数1:查询条件
```javascript
User.remove({age: 6}, function(err){
    if (err) console.log(err);
})
```

## 4. Entity - 文档操作
由Model创建的实体，使用save方法保存数据，Model和Entity都有能影响数据库的操作，但Model比Entity更具操作性
创建
```javascript
//使用Entity来增加一条数据
var krouky = new PersonModel({name:'krouky'});
krouky.save(callback);
//对比使用Model来增加一条数据
var MDragon = {name:'MDragon'};
PersonModel.create(MDragon,callback);
```


## 5. 修改器和更新器

** 更新修改器 **
`$inc` 增减修改器,只对数字有效.下面的实例: 找到 age=22的文档,修改文档的age值自增1
```javascript
Model.update({'age':22}, {'$inc':{'age':1} }  ); // 执行后: age=23
```
`$set` 指定一个键的值,这个键不存在就创建它.可以是任何MondoDB支持的类型.
```javascript
Model.update({'age':22}, {'$set':{'age':'haha'} }  ); // 执行后: age='haha'
```
`$unset` 同上取反,删除一个键
```javascript
Model.update({'age':22}, {'$unset':{'age':'haha'} }  ); //执行后: age键不存在`
```

** 数组修改器: **
`$push`给一个键push一个数组成员,键不存在会创建
```javascript
Model.update({'age':22}, {'$push':{'array':10} } ); //执行后: 增加一个 array 键,类型为数组, 有一个成员 10`
```
`$addToSet` 向数组中添加一个元素,如果存在就不添加
```javascript
Model.update({'age':22}, {'$addToSet':{'array':10} } ); // 执行后: array中有10所以不会添加
```
`$each` 遍历数组, 和 $push 修改器配合可以插入多个值
```javascript
Model.update({'age':22}, {'$push':{'array':{'$each': [1,2,3,4,5]}} } ); //执行后: array : [10,1,2,3,4,5]
```
`$pop` 向数组中尾部删除一个元素
```javascript
Model.update({'age':22}, {'$pop':{'array':1} } ); //执行后: array : [10,1,2,3,4] tips: 将1改成-1可以删除数组首部元素
```
`$pull` 向数组中删除指定元素
```javascript
Model.update({'age':22}, {'$pull':{'array':10} } ); // 执行后: array : [1,2,3,4] 匹配到array中的10后将其删除
```

** 条件查询: **
`$lt `小于
`$lte` 小于等于
`$gt` 大于
`$gte` 大于等于
`$ne` 不等于
```javascript
Model.find({'age':{ '$get':18 , '$lte':30 } } ); //查询 age 大于等于18并小于等于30的文档
```

** 或查询 OR: **
`$in`  一个键对应多个值
`$nin` 同上取反, 一个键不对应指定值
`$or`  多个条件匹配, 可以嵌套 $in 使用
`$not` 同上取反, 查询与特定模式不匹配的文档
```javascript
Model.find({'age':{ '$in':[20,21,22.'haha']} } ); //查询 age等于20或21或21或'haha'的文档
Model.find({'$or' :  [ {'age':18} , {'name':'xueyou'} ] }); //查询 age等于18 或 name等于'xueyou' 的文档
```

** 类型查询: **
null 能匹配自身和不存在的值, 想要匹配键的值 为null, 就要通过  '$exists' 条件判定键值已经存在 "$exists" (表示是否存在的意思)
```javascript
Model.find('age' :  { '$in' : [null] , 'exists' : true  } ); // 查询 age值为null的文档
```
```javascript
Model.find({name:{$exists:true}},function(error,docs){
  //查询所有存在name属性的文档
});
Model.find({telephone:{$exists:false}},function(error,docs){
  //查询所有不存在telephone属性的文档
});
```


** 正则表达式: **
MongoDb 使用 Prel兼容的正则表达式库来匹配正则表达式
```javascript
find( {'name' : /joe/i } );  //查询name为 joe 的文档, 并忽略大小写
find( {'name' : /joe?/i } ); //查询匹配各种大小写组合
```

** 查询数组: **
`Model.find({'array':10} )`; 查询 array(数组类型)键中有10的文档, array : [1,2,3,4,5,10] 会匹配到
`Model.find({'array[5]':10} )`; 查询 array(数组类型)键中下标5对应的值是10, array : [1,2,3,4,5,10] 会匹配到
`$all` 匹配数组中多个元素
`Model.find({'array':[5,10]} )`; 查询 匹配array数组中 既有5又有10的文档
`$size` 匹配数组长度
`Model.find({'array':{"$size" : 3} } )`; 查询 匹配array数组长度为3 的文档
`$slice` 查询子集合返回
`Model.find({'array':{"$skice" : 10} } )`; 查询 匹配array数组的前10个元素
`Model.find({'array':{"$skice" : [5,10] } } )`; 查询 匹配array数组的第5个到第10个元素

** where **
用它可以执行任意javacript语句作为查询的一部分,如果回调函数返回 true 文档就作为结果的一部分返回
```javascript
//where
//查询数据类型是字符串时，可支持正则
User.where('age', '2').exec(function(err, data){
    if (err) console.log(err);
    console.log(data);
});

User
    .where('age').gte(1).lte(10)
    .where('name', '张三')
    .exec(function(err, data){
      if (err) console.log(err);
      console.log(data);
    });
```


** 游标: **
`limit(3)` 限制返回结果的数量,
`skip(3)` 跳过前3个文档,返回其余的
`sort( {'username':1 , 'age':-1 } )` 排序 键对应文档的键名, 值代表排序方向, 1 升序, -1降序

## 6.其他

** 数量查询 **
```javascript
// //返回数量
User.count({age: 2}, function(err, data){
    if (err) console.log(err);
    console.log(data);
})
```

** 分页查询 **
```javascript
var User = require("./user.js");
function getByPager(){
    var pageSize = 5;                   //一页多少条
    var currentPage = 1;                //当前第几页
    var sort = {'logindate':-1};        //排序（按登录时间倒序）
    var condition = {};                 //条件
    var skipnum = (currentPage - 1) * pageSize;   //跳过数
    User.find(condition).skip(skipnum).limit(pageSize).sort(sort).exec(function (err, res) {
        if (err) {
            console.log("Error:" + err);
        }
        else {
            console.log("Res:" + res);
        }
    })
}
getByPager();
```
