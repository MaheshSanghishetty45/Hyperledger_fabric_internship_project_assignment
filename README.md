# Hyperledger Fabric Internship Assignment

**Project:** Asset management for a financial institution  
**Stack:** Hyperledger Fabric (chaincode in Go), Node.js REST API (Fabric Gateway), Dockerfile for REST API

## Features implemented
- Chaincode (Go) for asset management with fields:
  - DEALERID, MSISDN, MPIN, BALANCE, STATUS, TRANSAMOUNT, TRANSTYPE, REMARKS
- Functions:
  - CreateAsset, ReadAsset, UpdateAsset, DeleteAsset, GetAllAssets, GetAssetHistory, TransferAmount
- Node.js REST API to interact with deployed chaincode via Fabric Gateway
- Dockerfile to containerize REST API
- Instructions to deploy to Fabric test network (using Fabric test-network from fabric-samples)


## Folder structure
```
/chaincode
  /go
    go.mod
    asset_contract.go
/rest-api
  package.json
  server.js
  Dockerfile
.gitignore
README.md
```

## How to use

### 1) Setup Fabric test network
Use the Fabric test network from Hyperledger Fabric samples. Follow steps in fabric-samples/test-network:
- `git clone https://github.com/hyperledger/fabric-samples.git`
- `cd fabric-samples/test-network`
- `./network.sh up createChannel -ca`
- Install chaincode (see fabric-samples asset-transfer-basic doc). This project assumes chaincode packaged with name `asset_contract_go`.

### 2) Deploy chaincode
Place the chaincode folder `chaincode/go` under your chaincode packaging directory, then package and install per Fabric docs:
- `peer lifecycle chaincode package asset_contract_go.tar.gz --path ./chaincode/go --lang golang --label asset_contract_go_1`
- Follow the install/approve/commit lifecycle steps in Fabric docs.

### 3) Run REST API
- Copy connection profile and wallet credentials (connection-org1.json, identities) into `/rest-api/wallet` and `/rest-api/connection-org1.json` as required by the Gateway sample.
- Install dependencies: `npm install`
- Start server: `node server.js`
- Use HTTP endpoints (see README in /rest-api)

### 4) Docker build for REST API
- `docker build -t asset-rest-api:latest .`
- Run with appropriate mounted wallet and CCP or include the connection profile inside the image (not recommended for production).

## Endpoints (example)
- `POST /api/asset` - create asset
- `GET /api/asset/:id` - read asset
- `PUT /api/asset/:id` - update asset
- `DELETE /api/asset/:id` - delete asset
- `GET /api/assets` - get all assets
- `GET /api/asset/:id/history` - get asset history
- `POST /api/asset/:id/transfer` - transfer amount between accounts
