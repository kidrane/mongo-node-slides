!SLIDE

# MongoDB & Node.js #
## 程显峰 ##

!SLIDE bullets smaller transition=fade

# Who am I #

* [程显峰 Mars Cheng](mailto:chengxianfeng@admaster.com.cn)
* Evangelist at AdMaster
* Rubyist & Emacser
* [@程显峰-Mars](http://weibo.com/marscheng)

!SLIDE bullets incremental small transition=fade
# 做一个Web应用 #

* Python/Ruby/PHP/Ruby
* JavaScript
* MySQL/PostgreSQL
* RabbitMQ/Redis
* HTML/CSS

!SLIDE
# 一点也不好玩 #

* 太多东西要学习
* 构建一个实时应用依然不太容易


!SLIDE bullets smaller incremental
# MongoDB和Node结合的优势 #

* 同样的语法 JavaScript
* 同样的数据格式 JSON
* 门当户对
* PaaS ready

!SLIDE
# 典型场景

##  实时数据分析

!SLIDE

# Node的MongoDB驱动特色 #

### 非阻塞 ###
### DIRTY and FAST

!SLIDE code smaller
# Ruby例子 #

    @@@ ruby
    require 'mongo'
    db = Mongo::Connection.new.db("mydb")
    coll = db['test']
    doc = {"name" => "MongoDB",
           "type" => "database",
           "count" => 1,
           "info" => {"x" => 203, "y" => '102'}}
    coll.insert(doc)

!SLIDE code smaller


    @@@ javascript
    var client = new Db('test',
                        new Server("127.0.0.1", 27017, {}))
      , foo = function (err, collection) {
          collection.insert({a:2}, function(err, docs) {
            client.close();
          })
        };
    client.open(function(err, p_client) {
      client,collection('test', foo);
    });

!SLIDE
# WTF #
### too many callbacks ###
### Maybe Time to Try CoffeeScript ###

!SLIDE
# Mongoose
## ODM for Node

!SLIDE commandline incremental

    $ npm install mongoose
    mongoose@2.5.9 ./node_modules/mongoose
    ├── hooks@0.1.9
    └── mongodb@0.9.7-3-5

!SLIDE code smaller

    @@@ javascript
    var mongoose = require('mongoose');
    mongoose.connect('mongodb://localhost/my_database');

!SLIDE code smaller
# Schema

    @@@ javascript
    var Comments = new Schema({
        title     : String
      , body      : String
      , date      : Date
    });
    var Comment = mongoose.model('Comments', Comments);

!SLIDE code smaller
# set & get

    @@@ javascript
    Comments.path('title').set(function (v) {
      return capitalize(v);
    });

!SLIDE code smaller

    @@@ javascript
    function obfuscate (cc) {
      return '****-****-****-'
        + cc.slice(cc.length-4, cc.length);
    }
    var AccountSchema = new Schema({
      creditCardNumber: { type: String,
                          get: obfuscate }
    });
    var Account = mongoose.model('Account',
                                 AccountSchema);
    Account.findById( someId, function (err, found) {
      console.log(found.creditCardNumber);
      // '****-****-****-1234'
    });

!SLIDE code smaller
# validator

    @@@ javascript
    function validator (v) {
      return v.length > 5;
    };
    new Schema({
      name: { type: String,
              validate: [validator, 'my error type'] }
    })

!SLIDE code smaller
# 虚拟属性

    @@@ javascript
    PersonSchema
    .virtual('name.full')
    .get(function () {
      return this.name.first + ' ' + this.name.last;
    })
    .set(function (setFullNameTo) {
      var split = setFullNameTo.split(' ')
        , firstName = split[0]
        , lastName = split[1];

        this.set('name.first', firstName);
        this.set('name.last', lastName);
    });


!SLIDE code smaller
# Middleware

    @@@ javascript
    schema.pre('remove', function (next) {
      // ...
    })

!SLIDE
# QueryStreams

## The Node Way

[example](https://gist.github.com/1403797)


!SLIDE bullets incremental
# Plugins

* auth
* types
* joins
* dbref

!SLIDE
# 与Web集成

[express-mongoose](https://github.com/LearnBoost/express-mongoose)
[backbone.js](http://documentcloud.github.com/backbone/)

!SLIDE bullets incremental smaller
# 缺点 #

* 编程模型不一样 event
* 很多很多callback
* 调试困难

!SLIDE

# Q & A #
## Happy Hacking
