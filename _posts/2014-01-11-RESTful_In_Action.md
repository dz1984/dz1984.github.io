---
layout: post
title: RESTful in Action
categories: articles
tags: [RESTful,NodeJS,MongoDB]
image:
  feature: texture-feature-01.jpg
date: 2014-01-11 12:20
---
## Summay

練習使用RESTful Web Servicet技術打造一個簡單書藉管理Web應用程式。
### REST & RESTFul Web Services
REST (Reprsentational State Transfer)是軟體構設計風格，並不是標準。RESTful Web Services是使用HTTP協定，並遵循REST原則的Web服務。

---

## 動手實作

### 修改package.json

這次實作會用到express、jade、mongoose等packages，另外，superagent和expect.js是用來測試時使用。

```js
{
  "name": "NodeJSTut",
  "version": "0.0.1",
  "description": "NodeJS Tutorial for RESTful",
  "keywords": [
    "OpenShift",
    "Node.js",
    "RESTful"
  ],
  "author": {
    "name": "DonaldIsFreak"
  },

  "engines": {
    "node": ">= 0.10.x",
    "npm": ">= 1.0.0"
  },

  "dependencies": {
        "express" : "3.3.8",
        "jade" : "*",
        "mongoose" : ">=0.0.1"
  },
  "devDependencies": {
        "superagent" : ">=0.15.0",
        "expect.js": ">=0.1.0"
  },
  "bundleDependencies": [],

  "private": true,
  "main": "server.js"
}
```

### MongoDB
先確定好MongoDB有安裝成功，建立Book Model，目前只有三個欄位，分別是title、isbn，以及description。 (_id是MongoDB自動產生)

```js
var mongoose = require("mongoose");

var bookSchema = new mongoose.Schema({
        title : String,
        isbn : String,
        description: String
});

module.exports = mongoose.model('Book',bookSchema);
```

### RESTful API規格

功能|     URL   |HTTP Method |Request   | Response
:---|:----------|:----------:|:--------:|:---------:
新增| /books    | POST       | Book JSON| --
刪除| /books/:id| DELETE     | --       | --
修改| /books/:id| PUT        | Book JSON| -- 
查詢| /books/:id| GET        | --       | Book JSON
列表| /books    | GET        | --       | Book JSON

### Express Server

+ server.js

```js
#!/bin/env node
var express = require('express'),
    routes = require('./routes'),
    apis = require('./routes/api'),
    http = require('http'),
    path = require('path'),
    mongoose = require('mongoose');

// connect MongoDB
mongoose.connect(process.env.OPENSHIFT_MONGODB_DB_URL+'nodejstut');

var app = express();

// all environments
app.set('port', process.env.OPENSHIFT_NODEJS_PORT || 8080);
app.set('ip',,process.env.OPENSHIFT_NODEJS_IP || '127.0.0.1');
app.set('views', __dirname + '/views');
app.set('view engine', 'jade');
app.use(express.favicon());
app.use(express.logger('dev'));
app.use(express.cookieParser());
app.use(express.bodyParser());
app.use(express.methodOverride());
app.use(app.router);
app.use(express.static(path.join(__dirname, 'public')));

// development only
if ('development' == app.get('env')) {
  app.use(express.errorHandler());
 }

http.createServer(app).listen(app.get('port'),app.get('ip'), function(){
  console.log('Express server listening on port ' + app.get('port'));
});
```

按照上面的RESTful API規格，主要負責MongoDB的CRUD。

```js
app.all('/*',function(req,res,next){
    res.header('Access-Control-Allow-Origin','*');
    res.header('Access-COntrol-Allow-Headers','X-Requested-With');
    next();
});

app.get('/',routes.index);

app.get('/books',apis.findAll);
app.get('/books/:id',apis.findByID);
app.post('/books',apis.post);
app.put('/books/:id',apis.updates);
app.del('/books/:id',apis.removeByID);
```
+ routes/api.js

  - ```GET /books```傳回全部Book JSON。

	```js
	exports.findAll = function(req,res,next){
	        Book.find(function(err,books){
	                res.send({book:books});
	        });
	};
	```
	
  - ```GET /books/:id```以id為查詢條件，傳回單筆Book JSON。
    
	```js
	exports.findByID = function(req,res,next){
	        Book.findOne({_id:req.params.id},function(err,book){
	                res.send({book:book});
	        });
	};
	```

  - ```POST /books```取得Book資料後寫入MongoDB。
    
	```js    
	exports.post = function(req,res){
	        var book = new Book({title:req.body.book.title,isbn:req.body.book.isbn,description:req.body.book.description});
	        book.save(function(err,book){
	                res.send({book:book});
	        });
	};
	```

  - ```PUT /books/:id```修改:id的Book資料 。
    
	```js
	exports.updates = function(req,res,next){
	        Book.update({_id:req.params.id},{$set:{description:req.body.book.description}},function(err,numberAffected,raw){
	                if (err)
	                        console.log(err);
	                res.send(200);
	        });
	};
	```

  - ```DELETE /books/:id```刪除:id的Book資料

	```js
	exports.removeByID = function(req,res,next){
	        Book.remove({_id:req.params.id},function(err){
	                if (err)
	                        next(err);
	        res.send(200);
	        });
	};
	```