# Blockchains

Blockchain is not just Bitcoin or Ethereum, it's a particular network. For example Bitcoin MainNet, Ethereum Goerli are blockchains, whilst Bitcoin and Ethereum are protocols which are used by these blockchains. A network can be one of the types - `private`, `test`, `public`.

## Get blockchains

`swisschain.sirius.validator_api.blockchains.Blockchains/Get`

`Authorization required`

Returns a full list of the blockchains available for the validator.

### Request

```protobuf
swisschain.sirius.validator_api.blockchains.Blockchains/Get

> Requets: (application/grpc)

message GetBlockchainsRequest {
}
```

Empty

### Response

```protobuf
swisschain.sirius.validator_api.blockchains.Blockchains/Get

> Response: (application/grpc) - success response

message GetBlockchainsResponse {
    oneof body {
        GetBlockchainResponseBody response = 1;
        .swisschain.sirius.validator_api.common.Error error = 2;
    }
}

message GetBlockchainResponseBody {
    repeated Blockchain blockchains = 1;

    message Blockchain {
        string id = 1;
        string name = 2;
        NetworkType network_type = 3;
        google.protobuf.StringValue tenant_id = 4;
    };

    enum NetworkType {
        PRIVATE = 0;
        TEST = 1;
        PUBLIC = 2;
    };
}
```

#### GetBlockchainResponseBody

name | type | description
-----| ---- | -----------
`blockchains` | *[Blockchain](#blockchains-get-blockchains-response-blockchain)[]* | List of the blockchains

#### Blockchain

name | type | description | example
-----| ---- | ----------- | -------
`id` | *string* | Sirius ID of the blockchain | `ethereum`
`name` | *string* | Name of the blockchain | `Ethereum`
`network_type` | *[NetworkType](#blockchains-get-blockchains-response-networktype)* | Blockchain network type | `public`
`tenant_id` | *optional*, *string* | ID of a tenant (subscription) to which is blockchain is dedicated to on Sirius | `<null>`, `61a9d900-802b-4830-b741-d98c3204e0d8`

#### NetworkType

Type of blockchain network

+ `private` - Private blockchain.
+ `test` - Public testnet blockchain.
+ `public` - Public mainnet blockchain.