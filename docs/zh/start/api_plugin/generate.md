# 自动生成 API 插件

Writing the API plugin yourself is boring. You can use code generator to generate all the code.

Let's say you want to add a `PublishTransactionalMessage` method to existing pubsub API. You write a new proto file `cmd/layotto_multiple_api/advanced_queue/advanced_queue.proto` :

```protobuf
// ......
/* @exclude extends pub_subs */
// AdvancedQueue is advanced pubsub API
service AdvancedQueue {

  rpc PublishTransactionalMessage(TransactionalMessageRequest) returns (TransactionalMessageResponse);

}

message TransactionalMessageRequest {
  string store_name = 1;

  string content = 2;
}

message TransactionalMessageResponse {
  string message_id = 1;
}

```

and run the generator:

```protobuf
protoc -I . \
          --go_out . --go_opt=paths=source_relative \
          --go-grpc_out=. \
          --go-grpc_opt=require_unimplemented_servers=false,paths=source_relative \
          --p6_out ./cmd/layotto_multiple_api/advanced_queue --p6_opt=paths=source_relative \
          cmd/layotto_multiple_api/advanced_queue/advanced_queue.proto
```

then you get the code:

<img src="https://user-images.githubusercontent.com/26001097/189822603-c4c9d0c6-86a1-4a66-bba8-3d01758808e7.png" width="30%" height="30%" />

Fix the path error and then you can register this API plugin in you `main`.

## Reference

[How to generate code and documentation from the .proto files](zh/api_reference/how_to_generate_api_doc)

[protoc-gen-p6](https://github.com/layotto/protoc-gen-p6)