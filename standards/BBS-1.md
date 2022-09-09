# Blazed Blockchain Standard 1
- Author: Blazed Labs LLC
- Created: 9/4/2022

## Abstract
The following standard allows for the implementation of a standard API for NFTs within smart contracts. This standard provides basic functionality to track and transfer NFTs.

We considered use cases of NFTs being owned and transacted by individuals as well as consignment to third party brokers/wallets/auctioneers ("operators"). NFTs can represent ownership over digital or physical assets. We considered the following assets to be represented on the Blazed Blockchains, they are:
* Blazed Cash (B$)
* Blazed Land Token (BLT)
* Blazed Bond Token (BBT)
  
## Motivation
The purpose for the Blazed Blockchains are to represent current assets/liabilities in the Expanded Blazed Universe (which includes all three blockchain layers). Furthermore, the digital currency will aid in the raising of capital and sale of private (and -- eventually -- public) equity in the Blazed Labs LLC, Concrete Games LLC, and Ruff Management Inc. companies.

## Specification
The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119.

**How do gas fees work on the Blazed Blockchain?**

Contracts only impose "gas" fees (or just called "fees" by the Blazed National Government) if they issue a new NFT into circulation (production).
Certain banks in the Blazed Nation may choose to impose fees for wire transfers. These tranfers are not taxed by the government. Sales of goods, services, and/or securities, however,  impose taxation.

Every BBS-1 complient Blockchain MUST:
* deploy a contract to govern their NFT
* pay fees to their resident city
  * Yearly Fee (for operating an NFT on the blockchain): B$500,000
* be managed by a company with AT LEAST 10 employees (including bots)

Every BBS-1 complient contract must implement the following interface:

```js
    interface CONTRACT {
        /* unique hash created by MD5([_symbol])  */
        String _hash;

        /* ex: Blazed Cash */
        String _name;

        /* format: 2-4 characters (including symbols; case insensitive) ex: B$, BBT, BLT */
        String _symbol;

        /* format: [host].[domain].[tld]; ex: cash.blazed.city */
        String _uri; 

        /* total NFT in circulation; format: [N_$].[N_¢] */
        String totalNFT;

        /* format: [_timestamp] (0 if inherited) */
        int _start;

        /* format: [_timestamp] (0 if perpetual) */
        int _end;

        /* maximum number of transactions allowed per block (0 for infinite/manual block mint) */
        int _transaction_limit;

        /* total number of blocks in chain */
        int blocks;

        /*
            interface Contract
            @desc Construct a new Smart Contract object.
            @param config {
                name: "[_name]",
                symbol: "[_symbol]",
                uri: "[host].[domain].[tld]",
                start: 0,
                end: 0,
                transaction_limit: 2500,
                totalNFT: "$.¢",
                genesis: "https://github.com/blazed-labs/blazed-labs/blob/main/chains/L1/genesis.json"
            }
            @return [_hash]
        */
        function _constructor(config) returns [String];

        /*
            Transfer(_from, _to, amount)
            @desc Transfer funds from one account (_from) to another (_to).
            @param _from = Routing #/Address of sender.
            @param _to = Account number of recipient.
            @param amount = Amount of NFT to send.
            @return [String] = Transfer URI 
                format: ([token_symbol]://[block_id])
                ex: B$://238624daa853204c36315fbca81cbac5047a7158ad6d28ef400a363c8bed1d81/
        */
        function transfer(Address _from, Address _to, double amount) returns [String];

        /*
            Approval(uuid)
            @desc generate "approval" Proof of Work (PoW).
            @param uuid = format: B$://238624daa853204c36315fbca81cbac5047a7158ad6d28ef400a363c8bed1d81/eeb6dd511cd17f51500916a0e619aaef4999c3408fcbd9bed587f6b62cbde162
        */
        function approval(String uuid, );

        /*
            MintBlock(uuid)
            @desc mins a new block on the chain.
            @return [String]
        */
        function mintBlock() returns [_symbol]:\/\/[block_hash]/;

        /*
            name()
            @desc gets the contract's name
            @return [String]
        */
        function name() returns _name;

        /*
            symbol()
            @desc gets the contract's symbol
            @return [String]
        */
        function symbol() returns _symbol;

        /*
            getTotal()
            @desc gets the total number of NFT in circulation.
            @return [String]
        */
        function getTotal() returns [String];

        /*
            getStart()
            @desc gets the starting timestamp.
            @return [int]
        */
        function getStart() returns [_start];

        /*
            getEnd()
            @desc gets the ending timestamp.
            @return [int]
        */
        function getStart() returns [_start];

        /*
            getTerm()
            @desc gets the total term length (in microseconds).
            @return [microseconds]
        */
        function getTerm() returns int[_end - _start];

        /*
            getTransactionUri(blockSymbol, transactionId)
            @desc get full URI pointing to the transaction (if found).
            @param blockSymbol = select blockchain by symbol
            @param blockHash = select block by hash
            @param transactionId = select transaction on block
            @return 
                if transactionId is 0:
                    [_symbol]://[block_hash]/
                else:
                    [_symbol]://[block_hash]/[transaction_id]
        */
        function getTransactionUri(String blockSymbol, String blockHash, int transactionId);

        /*
            deploy(citySymbol, district, streetAddress, roomId)
            @desc deploy the contract to a room (_roomId), located on a street (_streetAddress) w/ plot addressing number, inside the district of (_district) and the city of (_citySymbol).
            @param citySymbol = Symbol of the city
            @param 
        */
        function deploy(String citySymbol, int district, String streetAddress, int roomId);

    }
```
* The above is required to begin issuing a token (NFT) on a Blazed Blockchain.

To hold, send, and lend (or borrow) NFT tokens, the server node(s) must also host a database of wallets (or banking accounts). The table schemas required to support the wallet (or banking account) functionality are as follows:
```sql
   CREATE TABLE bank_accounts(
    `id`       INT(11)      NOT NULL,
    `routing`  VARCHAR(32)  NOT NULL,    /* address of issuing bank's assigned routing # */
    `type`     INT(2)       NOT NULL,    /* see below for the possible account types */
    `address`  VARCHAR(64)  NOT NULL,    /* MD5 hash of [_id] */
    `public`   TEXT         NOT NULL,    /* format: PGP (RSA/2048) */
    `private`  TEXT         NOT NULL,    /* format: PGP (RSA/2048) */
    `owner`    INT(11)      NOT NULL,    /* unique _id of user or company that owns account */
    `email`    VARCHAR(255) NOT NULL,    /* format: MD5([user_id])@blz.one */
    `created`  TIMESTAMP    NOT NULL,    /* format: UNIX Timestamp */
    `balance`  FLOAT        NOT NULL,    /* format: [B$].[B¢] */
    `company`  VARCHAR(255) DEFAULT "",  /* format: [company_id]; or empty if personal */
   )

```
* The _address is an MD5 hash.
* The _type is one of the following:
  * Personal Accounts
    * **1**: Personal Checking
    * **2**: Personal Savings
    * **3**: Personal Credit
    * **4**: Personal Fixed-Term Loan
  * Business Accounts
    * **5**: Business Checking
    * **6**: Business Savings
    * **7**: Business Credit
    * **8**: Business Fixed-Term Loan
  * Other
    * **9**: Bond Note
    * **10**: Equity Share (Stock)
    * **11**: Exchange-Traded Fund (ETF/portfolio of stock)
    * **12**: Mutual Fund (pool of investors/portfolio of stock)
* The _public and _private are PGP keys (RSA/2048) [Passphrase: MD5 hash of UNIX timestamp at time of creation].
* The _name is the uuid of the user account
* The _email address is assigned to each user by id.
* Temporary accounts may use expiring keys.
* If credit or fixed-term loan, the "balance" represents current liabilities.
* Woodrow Financial's Bank Account Address (and main routing #) is: c4ca4238a0b923820dcc509a6f75849b

Every BBS-1 complient block will be stored in the following JSON schema for genesis blocks:

```
{
    "_id": "1",
    "CX": "B$",
    "HX": "22beb300ac9673368557debd75b5ea8cd7bc3a70058133b9732e8d640a5af8ab",
    "PX": "cb03b8fa2f174c0ab061798c78ba30418382af5a571a0d114bfc880f4bcaa87f"
}
```

where:
* **CX**: Chain symbol
* **HX**: Block hash [SHA256(_id)]
* **PX**: Previous block hash [SHA256(G/0***) for genesis block]


Every BBS-1 complient block will be stored in the following JSON schema for subsequent blocks:
```
{
    "_id": "2",
    "CX": "B$",
    "HX": "5bfc40149be02fa6695f93cbf16de8defbe739c4bb049cafa87172a7ed79acc7",
    "PX": "22beb300ac9673368557debd75b5ea8cd7bc3a70058133b9732e8d640a5af8ab",
    "NX": 1,
    "TX": {
        "1": {
            "_id": "1",
            "_to": "c81e728d9d4c2f636f067f89cc14862c::c4ca4238a0b923820dcc509a6f75849b",
            "_from": "c81e728d9d4c2f636f067f89cc14862c::eccbc87e4b5ce2fe28308fd9f2a7baf3",
            "_amount": "17.23",
            "_nonce": "975",
            "_timestamp": "1662282475",
            "_hash": "eeb6dd511cd17f51500916a0e619aaef4999c3408fcbd9bed587f6b62cbde162",
            "_validators": {
                    "1": "[validator_A]",
                    "2": "[validator_B]",
                    [...]
                }
        }
        [...]
    }
}
```

where:
* **CX**: Chain symbol
* **HX**: Block hash [SHA256(_id)]
* **PX**: Previous block hash [SHA256(previous_id)]
* **NX**: Total number of transactions on block
* **TX**: Transaction List [JSON]
  * _id: N (incriments from 1, counter is reset for each new block)
  * _to: [to_routing::to_address]
  * _from: [from_routing::from_address]
  * _amount: [B$].[B¢]
  * _nonce: RAND(0, 3096)
  * _timestamp: [UNIX_TIMESTAMP]
  * _hash: SHA256([_amount]@[_timestamp]?[_nonce]![_id])
  * _validators: [JSON]



