

# Sample gRPC server
## Structure
```
bookshelf
├── README.md
├── app    # application
│   ├── adapter    # interface definition. files in app/service call external services based on the interface.
│   ├── cmd        # set-up functions and main function
│   ├── config
│   ├── external   # external service callers that implement app/adapter
│   ├── model      # the most important thing. models depend on nothing.
│   ├── pkg        # packages that do not depend on the project (this microservice itself)
│   ├── server     # responsible for validating requests and handling requests and events
│   └── service    # corresponds to use-case layer in the clearn architecture
├── client  # generated code based on proto
│   ├── api 
│   └── event
├── docker-compose.yaml # local env prep
└── proto
    ├── api
    │   └── bookshelf.proto
    └── event
```

## How to add a feature
1. mod proto/api or proto/event
2. specify api or event, substitute xxxx with the file name and exetute the command belown in the proto directory
```bash
$ protoc --go_out=../client --go_opt=paths=source_relative \
    --go-grpc_out=../client --go-grpc_opt=paths=source_relative \
    [api or event]/xxxx.proto
```
3. change files in the app directory

## How to run on your local
Copy app/config/.envrc and paste it to Termial. Then, in the root directory (bookshelf in this case), execute
```bash
go run app/cmd/main.go app/cmd/setup.go
```


## How to call RPCs using grpcul
Assuming that tools are installed on your computer.

#### Call ListBooks
``` bash
$ grpcurl -plaintext localhost:8080 BookshelfService/ListBooks
```

#### Call AddBook
```bash
$ grpcurl -plaintext -d '{"title": "sample", "category": 1, "author": "satoshi"}' localhost:8080 BookshelfService/AddBook
```
