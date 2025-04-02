# grpc
gRPC Node.js Example
This project demonstrates how to create a simple gRPC service in Node.js using Protocol Buffers.

Requirements
Node.js
Protocol Buffers (protoc)
Installation
Clone this repository or create a new project folder:

mkdir grpc-tp && cd grpc-tp
Initialize a new Node.js project:

npm init -y
Install the required dependencies:

npm install @grpc/grpc-js @grpc/proto-loader
Define the gRPC Service
Create a hello.proto file with the following content:

syntax = "proto3";

package hello;

service Greeter {
  rpc SayHello (HelloRequest) returns (HelloReply) {}
}

message HelloRequest {
  string name = 1;
}

message HelloReply {
  string message = 1;
}
Implement the gRPC Server
Create a server.js file with the following code:

const grpc = require('@grpc/grpc-js');
const protoLoader = require('@grpc/proto-loader');
const path = require('path');

const PROTO_PATH = path.join(__dirname, 'hello.proto');
const packageDefinition = protoLoader.loadSync(PROTO_PATH, {
  keepCase: true,
  longs: String,
  enums: String,
  defaults: true,
  oneofs: true
});
const helloProto = grpc.loadPackageDefinition(packageDefinition).hello;

function sayHello(call, callback) {
  const { name } = call.request;
  callback(null, { message: `Bonjour, ${name} !` });
}

function main() {
  const server = new grpc.Server();
  server.addService(helloProto.Greeter.service, { SayHello: sayHello });
  const port = '0.0.0.0:50051';
  server.bindAsync(port, grpc.ServerCredentials.createInsecure(), () => {
    console.log(`Server running at ${port}`);
  });
}

main();
Running the gRPC Server
Start the server by running:

node server.js
You should see:

Server running at 0.0.0.0:50051
Testing with Postman
Open Postman
Create a new gRPC Request
Set the host as localhost:50051
Import or define the .proto file
Select Greeter.SayHello method
Send a request with the following JSON body:
{ "name": "TestUser" }
Expected response:
{ "message": "Bonjour, TestUser !" }


