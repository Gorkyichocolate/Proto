# Contract Repository Prompt

Create a contract-first Protocol Buffers repository for the AdvancedProgramming2 Go microservices project.

Repository A: `github.com/Gorkyichocolate/Proto`
Repository B: `github.com/Gorkyichocolate/ap2-generated`

This repository should contain only:
- `.proto` contract definitions
- GitHub Actions workflow for remote Go code generation
- documentation for the contract flow

## Required contract

Service: `PaymentService`
- rpc `ProcessPayment(PaymentRequest) returns (PaymentResponse)`

Service: `OrderService`
- rpc `SubscribeToOrderUpdates(OrderRequest) returns (stream OrderStatusUpdate)`

Messages:
- `PaymentRequest`:
  - `string order_id`
  - `int64 amount`
- `PaymentResponse`:
  - `string transaction_id`
  - `string status`
  - `google.protobuf.Timestamp processed_at`
- `OrderRequest`:
  - `string order_id`
- `OrderStatusUpdate`:
  - `string order_id`
  - `string status`
  - `string customer_id`
  - `string item_name`
  - `int64 amount`
  - `google.protobuf.Timestamp updated_at`

## Proto requirements

- use `syntax = "proto3"`
- set `package ap2.v1`
- use `google/protobuf/timestamp.proto`
- set `option go_package = "github.com/Gorkyichocolate/ap2-generated/ap2/v1;ap2v1"`

## Workflow requirements

- GitHub Actions workflow should run on push to `main` and on manual dispatch
- install `protoc`, `protoc-gen-go`, and `protoc-gen-go-grpc`
- clone Repository B using a `PAT_TOKEN` secret
- generate `.pb.go` files into the generated-code repository
- commit, tag, and push generated files to Repository B

## Usage guidance

Document how to consume generated code from Repository B in Go:

```bash
go get github.com/Gorkyichocolate/ap2-generated@vX.Y.Z
```

Also document the Contract-First flow:
- Repository A holds only `.proto`
- Repository B holds only generated Go code
- service implementation repos import generated code via `go get`
