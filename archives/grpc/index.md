# 

### gPRC 설정
```bash

go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
go install google.golang.org/protobuf/cmd/protoc-gen-go@v1.28
go install github.com/envoyproxy/protoc-gen-validate@v0.6.3

```

```bash
echo 'export GOPATH=$HOME/Go' >> $HOME/.zshrc
echo 'export PATH=$PATH:$GOPATH/bin' >> $HOME/.zshrc
```
