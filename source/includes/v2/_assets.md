# Assets

Sirius provides the set of crypto assets available to use.
You can get notifications about a deposit or initiate a withdrawal in any asset presented in this set.
The asset is a generic model which represents a native asset, fungible token or a non-fungible token.
It's possible that in the different blockchains assets with the same symbol and/or address are existed, so you have to rely only on the asset ID if you need to deterministically identify it.
If you've received a deposit of an asset which is not supported by Sirius yet, you can add this particular asset to your personal list by specifying all needed asset parameters.

## Search

Returns assets by search parameters.

```csharp
var request = new AssetSearchRequest
{
    Id = 10001,
    BlockchainId = "ethereum",
    Symbol = "USDC",
    TokenId = null,
    Address = "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
    Accuracy = 6,
    Pagination = new PaginationInt64
    {
        Cursor = null,
        Limit = 100,
        Order = PaginationOrder.Asc
    }
};

var response = client.AssetsV2.SearchAsync(request);
```

### Request

name | type | description | example
---- | ---- | ----------- | -------
`Id` | *optional*, *long* | ID of the asset to search | 100553
`BlockchainId` | *optional*, *string* | Exact blockchain id to search | bitcoin-private
`Symbol` | *optional*, *string* | Text to search in the asset symbol | BTC
`TokenId` | *optional*, *string* | Text to search in the asset symbol | BTC
`Address` | *optional*, *string* | Exact address to search | 0xC1701AbD559FC263829CA3917d03045F95b5224A
`Accuracy` | *optional*, *int* | Exact accuracy to search | 8
`Pagination` | *optional*, *[Pagination](#api-usage-pagination-request)* | |

### Response

*[Paginated](#api-usage-pagination-response)* array of the [assets](#assets-models-asset).

## Models

### Asset 

name | type | description | example
---- | ---- | ----------- | -------
`Id` | *long* | ID of the asset | 100003
`BlockchainId` | *string* | Exact blockchain id | bitcoin-private
`Symbol` | *string* | Asset symbol | BTC
`TokenId` | *string* | Token ID | 101
`Address` | *string* | Exact address | 0xC1701AbD559FC263829CA3917d03045F95b5224A
`Accuracy` | *int* | Exact accuracy | 8
`Name` | *string* | The name of token | Toy
`MetadataUrl` | *string* | The token metadata URL | ipfs://metadata.json
`Type` | *string* | The blockchain-specific type of token | ERC-20
`Brokerage` | *[BrokerageAssetAttributesInfo](#assets-models-brokerageassetattributesinfo)* | The brokerage attributes |
`Aml` | *[AmlAssetAttributesInfo](#assets-models-amlassetattributesinfo)* | The AML attributes |

### BrokerageAssetAttributesInfo

name | type | description | example
---- | ---- | ----------- | -------
`Id` | *long* | ID of the info | 300001
`MinDepositThreshold` | *decimal* | Minimum deposit threshold | 0.0001
`MinWithdrawalAmount` | *decimal* | Minimum withdrawal amount | 0.1
`CreatedAt` | *DateTime* | Create date time |
`UpdatedAt` | *DateTime* | Update date time |

### AmlAssetAttributesInfo

name | type | description | example
---- | ---- | ----------- | -------
`Id` | *long* | ID of the info | 300001
`Chainalysis` | *[AmlChainalysisAssetAttributesInfo](#assets-models-amlchainalysisassetattributesinfo)* | Chainalysis attributes |
`MerkleScience` | *[AmlMerkleScienceAssetAttributesInfo](#assets-models-amlmerklescienceassetattributesinfo)* | MerkleScience attributes |
`CreatedAt` | *DateTime* | Create date time |
`UpdatedAt` | *DateTime* | Update date time |

### AmlChainalysisAssetAttributesInfo

name | type | description | example
---- | ---- | ----------- | -------
`Symbol` | *string* | Chainalysis asset symbol | BTC
`IsCheckWithdrawalsEnabled` | *bool* | Indicates that withdrawals check enabled | true
`IsCheckDepositsEnabled` | *bool* | Indicates that deposits check enabled | true
`IsNotifyWithdrawalsEnabled` | *bool* | Indicates that notify withdrawals check enabled | true
`TransferFormat` | *[ChainalysisTransferFormat](#assets-models-chainalysistransferformat-enum)* | Update date time |

### ChainalysisTransferFormat (enum)

+ `HashAndIndex` - Hash and index
+ `HashAndAddress` - Hash and address
+ `Hash` - Hash

### AmlMerkleScienceAssetAttributesInfo

name | type | description | example
---- | ---- | ----------- | -------
`Symbol` | *string* | MerkleScience asset symbol | BTC
`Currency` | *int* | MerkleScience currency ID | 12
`IsDisabled` | *bool* | Indicates that asset check enabled | true
