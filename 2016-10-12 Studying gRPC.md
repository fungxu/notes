gRPC / etcd3 学习
=====
Fung.xu @ 2016.10

# gRPC 是什么，同级的是什么，有什么优势？


> 是什么?

gRPC是一个高性能、通用的开源RPC框架，其由Google主要面向移动应用开发并基于HTTP/2协议标准而设计，基于ProtoBuf(Protocol Buffers)序列化协议开发，且支持众多开发语言。

> ProtoBuf是一种数据序列化协议（类似于XML、JSON、hessian）

> 优势:

gRPC提供了一种简单的方法来精确地定义服务和为iOS、Android和后台支持服务自动生成可靠性很强的客户端功能库。客户端充分利用高级流和链接功能，从而有助于节省带宽、降低的TCP链接次数、节省CPU使用、和电池寿命。


# 怎么用? 以 node 为例


## 建好 protocal 文件

helloworld.proto
```

syntax = "proto3";

option java_multiple_files = true;
option java_package = "io.grpc.examples.helloworld";
option java_outer_classname = "HelloWorldProto";
option objc_class_prefix = "HLW";

package helloworld;

// 接口定义, 定义类
service Greeter {
  // 定义方法, 输入及输入出
  // Sends a greeting
  rpc SayHello (HelloRequest) returns (HelloReply) {}
}

// 定义数据结构
// The request message containing the user's name.
message HelloRequest {
  string name = 1;
}

// The response message containing the greetings
message HelloReply {
  string message = 1;
}


```

## 建服务器

```
var PROTO_PATH = __dirname + 'helloworld.proto';

var grpc = require('grpc');
var hello_proto = grpc.load(PROTO_PATH).helloworld;

/**
 * Implements the SayHello RPC method.
 */
function sayHello(call, callback) {
  callback(null, {message: 'Hello ' + call.request.name});
}

/**
 * Starts an RPC server that receives requests for the Greeter service at the
 * sample server port
 */
function main() {
  var server = new grpc.Server();
  
  // 注册服务
  server.addProtoService(hello_proto.Greeter.service, {sayHello: sayHello});

  server.bind('0.0.0.0:50051', grpc.ServerCredentials.createInsecure());
  server.start();
}

main();

```

## 建客户端 

```
var PROTO_PATH = __dirname + 'helloworld.proto';

var grpc = require('grpc');
var hello_proto = grpc.load(PROTO_PATH).helloworld;

function main() {
  var client = new hello_proto.Greeter('localhost:50051',grpc.credentials.createInsecure());
  var user;
  if (process.argv.length >= 3) {
    user = process.argv[2];
  } else {
    user = 'world';
  }
  client.sayHello({name: user}, function(err, response) {
    console.log('Greeting:', response.message);
  });
}

main();

```


# etcd3 

etcd3 分布式锁、选举和软件事务内存 ,基础服务接口使用gRPC代替了JSON以增加效率。原生的etcd3客户端使用gRPC协议进行通信。协议消息使用protobuf进行定义，它简化了PRC客户端存根代码的生成并且使协议易于管理。比较来看，即使etcd2客户端经过解析优化后与etcd3的消息处理性能仍然有2倍的差距。并且，gRPC在处理链接方面优势明显，因为gRPC使用单一链接的HTTP2多路复用流式RPC调用，而JSON客户端必须为每个请求建立一个链接。

服务发现
