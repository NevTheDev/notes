# gRPC


## What is gRPC

gRPC stands for google Remove Procedure Call, it is a light weight webservice protocall

## Proto File

``` c#
syntax = "proto3";

option csharp_namespace = "GrpcDemo.Services";

import "protos/someother.proto";

service ReaderService {
    rpc SendMessage(ReaderMessage) returns (ReaderResponse);
    rpc ClientStreamSendMessage(stream ReaderMessage) returns (ReaderResponse);
    rpc ServerStreamSendMessage(ReaderMessage) returns (stream ReaderResponse);
    rpc BiStreamSendMessage(stream ReaderMessage) returns (stream ReaderResponse);
}

message ReaderMessage {
    string Message = 1;
}

message ReaderResponse {
    string Message = 1;
    reserved 3, 4
}

message ArrayMessage {
    repeated ReaderMessage messages = 1
}
```

## Setting up the server
``` c#


```