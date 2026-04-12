# AdvancedProgramming2 Protobuf Contracts

This repository is Repository A: `github.com/Gorkyichocolate/Proto`.
It contains only contract-first Protocol Buffers definitions, a GitHub Actions workflow for remote Go code generation, and documentation for the contract flow.

## Contract repository ready

This repository now implements the Contract-First flow for the AdvancedProgramming2 project.

### What is included

- `ap2.proto` with:
  - `syntax = "proto3"`
  - `package ap2.v1`
  - `option go_package = "github.com/Gorkyichocolate/ap2-generated/ap2/v1;ap2v1"`
  - `PaymentService.ProcessPayment(PaymentRequest) returns (PaymentResponse)`
  - `OrderService.SubscribeToOrderUpdates(OrderRequest) returns (stream OrderStatusUpdate)`
  - all requested messages and `google.protobuf.Timestamp` fields
- `.github/workflows/generate.yml` with:
  - triggers on push to `main` and manual dispatch
  - installs `protoc`, `protoc-gen-go`, and `protoc-gen-go-grpc`
  - clones `github.com/Gorkyichocolate/ap2-generated` using `PAT_TOKEN`
  - generates `.pb.go` files into the generated-code repo
  - commits, tags, and pushes changes back to Repository B
- documentation describing the Repository A / Repository B roles and import flow

## Purpose

- Define service contracts for `PaymentService` and `OrderService`
- Keep only `.proto` definitions in Repository A
- Remotely generate Go code and push it into Repository B: `github.com/Gorkyichocolate/ap2-generated`

## Contract

The contract defines:

- `PaymentService.ProcessPayment(PaymentRequest) returns (PaymentResponse)`
- `OrderService.SubscribeToOrderUpdates(OrderRequest) returns (stream OrderStatusUpdate)`

The generated Go package path is configured as:

```proto
option go_package = "github.com/Gorkyichocolate/ap2-generated/ap2/v1;ap2v1";
```

## Usage

1. Create the generated code repository B: `github.com/Gorkyichocolate/ap2-generated`
2. Add a GitHub secret named `PAT_TOKEN` in repository A with a token that can push to repository B
3. Push to `main` or run the workflow manually from repository A

Service implementation repos consume generated code from repository B using:

```bash
go get github.com/Gorkyichocolate/ap2-generated@vX.Y.Z
```

Then import packages like:

```go
import "github.com/Gorkyichocolate/ap2-generated/ap2/v1"
```

## Contract-First flow

- Repository A (`github.com/Gorkyichocolate/Proto`) holds only `.proto` contracts
- Repository B (`github.com/Gorkyichocolate/ap2-generated`) holds only generated Go protobuf code
- Implementation repos (`order-service`, `payment-service`) import generated code via `go get`
- The GitHub Actions workflow in Repository A regenerates code and pushes updates to Repository B

## Notes

- No service implementation code is stored in this repository.
- The workflow generates `.pb.go` files from `.proto` and pushes them to the configured generated-code repository.
