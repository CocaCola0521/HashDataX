### Datax MongoDBJsonReader
#### 1 快速介绍

MongoDBJsonReader 插件利用 MongoDB 的java客户端MongoClient进行MongoDB的读操作。最新版本的Mongo已经将DB锁的粒度从DB级别降低到document级别，配合上MongoDB强大的索引功能，基本可以达到高性能的读取MongoDB的需求。

#### 2 实现原理

MongoDBJsonReader通过Datax框架从MongoDB并行的读取数据，通过主控的JOB程序按照指定的规则对MongoDB中的数据进行分片，并行读取，然后将MongoDB中的Document类型转换成字符串类型。

#### 3 功能说明
* 该示例从MongoDB读一份数据到GPDB。

	    {
	    "job": {
	        "setting": {
	            "speed": {
	                "channel": 2
	            }
	        },
	        "content": [
	            {
	                "reader": {
	                    "name": "mongodbjsonreader",
	                    "parameter": {
	                        "address": ["127.0.0.1:27017"],
	                        "userName": "",
	                        "userPassword": "",
	                        "dbName": "test",
	                        "collectionName": "restaurants",
	                        "column": []
	                    }
	                },
	                "writer": {
	                    "name": "gpdbjsonwriter",
	                    "parameter": {
	                        "column": ["c"],
	                        "accessId": "**************",
	                        "accessKey": "********************",
	                        "segment_reject_limit": 0,
	                        "preSql": [],
	                        "postSql": [],
	                        "connection": [
				             {
					            "jdbcUrl": "jdbc:postgresql://139.198.6.64:5432/hashdata",
					            "table": ["foo"]
				              }
			            ]
	                    }
	                }
	            }
	        ]
	    }
        }
#### 4 参数说明

* address： MongoDB的数据地址信息，因为MonogDB可能是个集群，则ip端口信息需要以Json数组的形式给出。【必填】
* userName：MongoDB的用户名。【选填】
* userPassword： MongoDB的密码。【选填】
* collectionName： MonogoDB的集合名。【必填】
* column：MongoDB的文档列名。【必填】
* name：Column的名字。【必填】
* type：Column的类型。【选填】
* splitter：因为MongoDB支持数组类型，但是Datax框架本身不支持数组类型，所以mongoDB读出来的数组类型要通过这个分隔符合并成字符串。【选填】

#### 5 类型转换

| DataX 内部类型| MongoDB 数据类型    |
| -------- | -----  |
| Long     | int, Long |
| Double   | double |
| String   | string, array |
| Date     | date  |
| Boolean  | boolean |
| Bytes    | bytes |


#### 6 性能报告
#### 7 测试报告