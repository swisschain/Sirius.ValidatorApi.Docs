# Data structures

## Order (enum)

Order of the sorting

+ `asc` - Ascending sorting order
+ `desc` - Descending sorting order

## NetworkType (enum)

Type of a blockchain network

+ `private` - Private blockchain.
+ `test` - Public test blockchain.
+ `public` - Public mainnet blockchain.

## BlockchainProtocol (object)

+ `code` (*string*) - Code of the protocol. (eg. "bitcoin").
+ `name` (*string*) - Name of the protocol. (eg. "Bitcoin").
+ `startBlockNumber` (*number*) - Number of the first block in the blockchain. Usually 0 or 1.
+ `requirements` (*object*) - Requirements of the protocol.
    + `publicKey` (*boolean*) - Indicates, if source address public key is required to build a transaction. (eg. `true`).
+ `capabilities` (*object*) - Capabilities of the protocol.
    + `destinationTag` (*optional*, *object*) - Capabilities of the protocol related to the destination tag. `null` if destination tag is not supported.
        + `number` (*optional*, *object*) - Capabilities of the protocol related to the `Number` [TagType](#TagType). `null` if `Number` tag is not suppoerted.
            + `min` (*number*) - Minimum number supported by the `Number` destination tag. (eg. 1).
            + `max` (*number*) - Maximum number supported by the `Number` destination tag. (eg. 9223372036854776000).
            + `name` (*string*) - Name of the `Number` tag field which is accepted in the given blockchain. (eg. "Memo ID").
        + `text` (*optional*, *object*) - Capabilities of the protocol related to the `Text` [TagType](#TagType). `null` if the `Text` tag is not supported.
            + `maxLength` (*number*) - Mximum length of the `Text` destination tag. (eg. 28).
            + `name` (*string*) - Name of the `Text` tag field which is accepted in the given blockchain. (eg. "Memo text").
        + `doubleSpendingProtectionType` (*[DoubleSpendingProtectionType](#DoubleSpendingProtectionType-enum)*) - Type of the protection agains funds double spending (eg. `coins`).

## TagType (enum)

Tag type of the account. Tag is an additional information associated with particular account, 
which can be specified in the blockchain transaction. This allows to use single blockchain 
address to handle many accounts on it. This feature is available only on some blockchains: 
(Ripple, Stellar, Eos...). In some blockchains different types of the tag are supported
and you have to specify exact type in this case.

+ `text` - text tag
+ `number` - numeric tag

## DoubleSpendingProtectionType (enum)

Each blockchain use some protection mechanism agains funds double-spending. Sirius distinguish next types:

+ `coins` - Coins are used to transfer funds. (eg. Bitcoin, Dash)
+ `nonce` - Nonce number (sequence) is used to distinguish outgoing transactions. (eg. Ethereum, Stellar)

## BrokerAccountState (enum)

State of the broker account

+ `creating` - The broker account is not fully created yet, you need to wait a bit, while Sirius complete its creation. You can create accounts within it, but not receive or send transfers.
+ `active` - The broker account is active and can be used as usual.
+ `blocked` - The broker account is blocked - processing of all deposits and withdrawals is on hold.

## AccountState (enum)

State of the account

+ `creating` - The account is not fully created yet, you need to wait a bit, while Sirius complete its creation. You can't receive or send transfers.
+ `active` - The account is active and can be used as usual.
+ `blocked` - The account is blocked - processing of all deposits and withdrawals is on hold.

## CustodyType (enum)

Type of the vault

+ `selfManaged` - can be run and managed either by **SwissChain** or by the **Customer** on infrastructure owned and controlled by the Customer. SwissChain provides Custody as a set of Docker images. It's fully secure and protected solution to keep your funds save and under your control. SwissChain, IBM admins, your admins or anyone else can't get unauthorized access to your funds. The solutions is based on **IBM HSM**, **Hyper Protect Virtual Servers**, **Hyper Protected DBaaS**, **Secure Image Build**, **Secure Image Deploy**, and **Operational Decision Manager**. It provides a flexible way to control you transaction signing policy. It includes **Multi Party Signature** mechanism which allows you to delegate transaction signing to any number of your employees and/or software components. The same time transactions can be signed automatically still. You can configure this via our transaction signing policy engine. It can be run either in the cloud or on-premise. IBM HSM is the only solution in the industry which is **FIPS 140-2 Level 4-certified**. [Contact](mailto:contact@swisschain.io) us to get help with deploying of the self-managed Custody.
+ `shared` - run and managed by SwissChain, shared between all the customers that use it.

## DepositState (enum)

State of the deposit

+ `detected` - deposit incoming transaction is detected
+ `confirmed` - deposit incoming transaction is confirmed (it's included in X blockchain blocks. X depends on particulaer blockchain)
+ `completed` - deposit consolidation transaction is confirmed
+ `failed` - deposit processing is failed
+ `cancelled` - the block with incoming transaction is removed from the blockchain due to reorganization of the network. It can happedn when deposit is detected
+ `provisioned` - token or tiny token deposit is provisioned with fee
+ `unknownAsset` - deposit of unknown asset is detected. Deposit processing will be continued once the asset is configured
+ `amlValidation` - AML validation of the deposit is in-progress
+ `amlFailed` - AML validation of the deposit is failed
+ `amlReviewed` - AML validation failure has been reviwed by the customer
+ `amlFailureAccepting` - AML validation failure is being accepted by the customer
+ `refunding` - deposit is being refunded
+ `refundingProvisioned` - token or tiny token deposit refund transaction is provisioned
+ `refunded` - deposit has been refunded

## DepositType (enum)

Type of the deposit

+ `tiny` - tiny deposit in native asset
+ `broker` - deposit on the broker account details
+ `regular` - deposit on account details in native token (ETH, BTC)
+ `token` - deposit on account details in custom token (ERC20)
+ `tinyToken` - deposit on account details in custom token (ERC20)

## BrokerAccountDetails (object)

Details of the broker account

+ `id` (*number*) - Unique ID.
+ `brokerAccountId` (*number*) - Broker account ID.
+ `createdAt` (*timestamp*) - Creation time of the details.
+ `address` (*string*) - Blockchain address of the details.
+ `blockchainId` (*string*) - Blockchain ID of the details.

## BrokerAccountDetailsBrief (object)

Brief broker account details

+ `id` (*number*) - Unique ID.
+ `address` (*string*) - Blockchain address of the details.

## AccountDetails (object)

Details of the account

+ `id` (*number*) - Unique ID.
+ `accountId` (*number*) - Account ID.
+ `createdAt` (*timestamp*) - Creation time of the details.
+ `address` (*string*) - Blockchain address of the details.
+ `blockchainId` (*string*) - Blockchain ID of the details.
+ `tag` (*optional*, *string*) - Tag of the details.
+ `tagType` (*optional*, *[TagType](#tagtype-enum)* - Type of the details tag.

## AccountDetailsBrief (object)

Brief details of the account

+ `id` (*number*) - Unique ID.
+ `address` (*string*) - Blockchain address of the details.
+ `tag` (*optional*, *string*) - Tag of the details.
+ `tagType` (*optional*, *[TagType](#tagtype-enum)* - Type of the details tag.

## BrokerAccountBalances (object)

Balances of the broker account in a particular asset. We distinguish 4 different balances
for each asset in each broker account.

+ `id` (*number*) - Unique ID.
+ `brokerAccountId` (*number*) - Broker account ID.
+ `assetId` (*number*) - Asset ID.
+ `ownedBalance` (*number*) - Balance owned by the broker.
+ `availableBalance` (*number*) - Balance available for a withdrawal.
+ `pendingBalance` (*number*) - Balance which is being deposited, but not completely transferred yet.
+ `reservedBalance` (*number*) - Balance which is being withdrawn, but not completely transferred yet.
+ `createdAt` (*timestamp*) - Creation time of the balances.
+ `updatedAt` (*timestamp*) - The update time of the balances.

## DestinationDetails (object)

Destination details for a transfer

+ `address` (*number*) - Details public address
+ `tag` (*optional*, *string*) - Tag which is related to address (applicable for Stellar)
+ `tagType` (*optional*, *[TagType](#tagtype-enum)*) - Tag type 

## WithdrawalDocument (object)

+ `version` (*string*) - Document version. Should be `1.0`
+ `brokerAccountId` (*number*) - ID of the broker account
+ `accountId` (*optional*, *number*) - ID of the account. Either `accountId` or `accountReferenceId` can be specified.
+ `accountReferenceId` (*optional*, *string*) - Reference ID of the account. Either `accountReferenceId` or `accountId` can be specified.
+ `withdrawalReferenceId` (*optional*, *string*) - Reference ID of the withdrawal. It can be internal ID of the withdrawal in your system.
+ `assetId` (*number*) - ID of the asset
+ `amount` (*decimal*) - Withdrawal amount
+ `destinationDetails` (*[DestinationDetails](#destinationdetails-object)*) - Destination details

## TransactionInfo (object)

TransactionInfo contains information about the transaction which happened on the blockchain.

+ `transactionId` (*string*) - Transaction ID (Hash).
+ `transaction_block ` (*number*) - Block number in which transaction was included in the blockchain.
+ `confirmationsCount` (*number*) - Count of blocks after the transaction block.
+ `requiredVonfirmationsCount` (*number*) - Required count of blocks for this transaction.
+ `date` (*timestamp*) - Date of the block.

## DepositSource (object)

DepositSource indicates from which address amount came form.

+ `address` (*string*) - Address of the sender.
+ `amount ` (*decimal*) - Amount in deposit asset.

## AmlChecks (object)

AmlChecks contains information about passed AML checks for deposit.

+ `overallResolution` (*[AmlOverallResolution](#amloverallresolution-enum)*) - Overall resolution of all AML checks.
+ `startedAt` (*timestamp*) - Time when AML checks was started.
+ `completedChecks` (*Array of [CompletedAmlCheck](#completedamlcheck-object)*) - Array of completed AML checks.
+ `requiredChecks` (*timestamp*) - Amount of running parallel checks for deposit/withdrawal.

## CompletedAmlCheck (object)

CompletedAmlCheck contains information about passed AML check for deposit.

+ `amlCheckId ` (*number*) - ID of this AML check in the system.
+ `amlConnectionId ` (*number*) - ID of the AML connection which was used for this check.
+ `resolution` (*[AmlResolution](#amlresolution-enum)*) - Resolution for specified AML check.
+ `finishedAt` (*timestamp*) - Time when AML check was completed.

## AmlOverallResolution (enum)

+ `pending` - Indicates that not all checks were completed
+ `failed` - Indicates that some checks are in HighRisk state. Manual check required
+ `succeeded` - Indicates that AML passed with lowRisk!

## AmlResolution (enum)

+ `highRisk` - Indicates that deposit/withdrawal has a high risk. Dirty money!
+ `normal` - AML has passed without issues.
+ `unknown` - AML provider can't asses risks.
+ `assetNotConfigured` - Asset is not configured for AML.

## WithdrawalState (enum)
+ `PROCESSING = 0;`
+ `EXECUTING = 1;`
+ `SENT = 2;`
+ `COMPLETED = 3;`
+ `FAILED = 4;`
+ `POLICY_VALIDATION = 5;`
+ `SIGNING = 6;`
+ `REJECTED = 7;`
+ `AML_VALIDATION = 8;`
+ `AML_FAILED = 9;`
+ `AML_REVIEWED = 10;`
+ `NOTIFYING_AML = 11;`
+ `REFUNDED = 12;`
+ `AML_FAILURE_ACCEPTING = 13;`


## WithdrawalError (enum)
+ `NOT_ENOUGH_BALANCE = 0;`
+ `INVALID_DESTINATION_ADDRESS = 1;`
+ `DESTINATION_TAG_REQUIRED = 2;`
+ `TECHNICAL_PROBLEM = 3;`
+ `VALIDATION_REJECTED = 4;`
+ `SIGNING_REJECTED = 5;`


## SmartContractDeploymentDocument (object)

+ `name` (*string*) - SmartContract internal name.
+ `blockchainId`  (*string*) - ID of blockchain used for deployment
+ `brokerAccountId`  (*string*) - ID of broker account which initiate invokation
+ `referenceId`  (*string*) - uniq identificatior of request
+ `arguments`  (*array*) - Array of repeatable object with `parameterName`  and `value` (can be empty)
    + `parameterName`  (*string*) - parameterName
    + `value`  (*object*) - Object wich contain `prototype` and *type* value
        + `prototype`  (*string*) - data type which used in argument # address
        + `*oneofDataItem*`  (*string*) - value depend on #DataItem
+ `payment`  (*object*) - Object which contatin payment details during deployment
+ `assetId`  (*int*) - Asset id for payment
+ `amount`  (*double*) - amount can be 0


## SmartContractInvokationDocument (object)

+ `methodName` (*string*) - Method name.
+ `methodAddress`  (*string*) - Method address
+ `brokerAccountId`  (*string*) - ID of broker account which initiate invokation
+ `referenceId`  (*string*) - uniq identificatior of request
+ `arguments`  (*array*) - Array of repeatable object with `parameterName`  and `value` (can be empty)
    + `parameterName`  (*string*) - parameterName
    + `value`  (*object*) - Object wich contain `prototype` and *type* value
        + `prototype`  (*string*) - data type which used in argument # address
        + `*oneofDataItem*`  (*string*) - value depend on #DataItem
+ `payment`  (*object*) - Object which contatin payment details during invokation
+ `assetId`  (*int*) - Asset id for payment
+ `amount`  (*double*) - amount can be 0

  

## DataItem 
+ `void` {}
+ `bool` bool value
+ `bytes` bytes value
+ `decimal` BigDecimal value
+ `int` BigInt value
+ `string` string value
+ `address` strint value
+ `timestamp` Timestamp value
+ `array` repeated #DataItem
+ `composite` repeated #Field (string name  & DataItem value) components
+ `either` - #Field (string name  & DataItem value) option
+ `map` repeated #MapItem (DataItem key / DataItem value)
+ `enum` string name

