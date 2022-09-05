# Blazed Blockchain Standard 1
- Author: Blazed Labs LLC
- Created: 9/4/2022

## Abstract
The following standard allows for the implementation of a standard API for NFTs within smart contracts. This standard provides basic functionality to track and transfer NFTs.

We considered use cases of NFTs being owned and transacted by individuals as well as consignment to third party brokers/wallets/auctioneers ("operators"). NFTs can represent ownership over digital or physical assets. We considered the following assets to be represented on the Blazed Blockchains, they are:
* L1: Blazed Cash (B$)
* L2: Blazed Land Token (BLT)
* L3: Blazed Bond Token (BBT)
  
## Motivation
The purpose for the Blazed Blockchains are to represent current assets/liabilities in the Expanded Blazed Universe (which includes all three blockchain layers). Furthermore, the digital currency will aid in the raising of capital and sale of private (and -- eventually -- public) equity in the Blazed Labs LLC, Concrete Games LLC, and Ruff Management Inc. companies.

## Specification
The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119.

Every BBS-1 complient contract must implement the following interface:

```js
    interface CONTRACT {
        String _name;
        String _symbol;
        function Transfer(Address _from, Address _to, double amount);
        function Approval(Address _owner, Address _approved, int tokenId);
        function balanceOf(Address _owner) returns double;
        function ownerOf(tokenId);
        function name() returns _name;
        function symbol() returns _symbol;
        function getTokenUri(tokenId) returns [string];
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
    `email`    VARCHAR(255) NOT NULL,    /* format: [user_id]@blz.one */
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
* The _email address is assigned to each user by id.
* Temporary accounts may use expiring keys.
* If credit or fixed-term loan, the "balance" represents current liabilities.
* Woodrow Financial's Bank Account Address (and main routing #) is: c4ca4238a0b923820dcc509a6f75849b

Every BBS-1 complient block will be stored in the following JSON schema for genesis blocks:

```
{
    "_id": "1",
    "CX": "BLZ",
    "HX": "6b86b273ff34fce19d6b804eff5a3f5747ada4eaa22f1d49c01e52ddb7875b4b",
    "PX": "G",
    "TX": {
        "1": {
            "_to": "c81e728d9d4c2f636f067f89cc14862c::c4ca4238a0b923820dcc509a6f75849b",
            "_from": "c81e728d9d4c2f636f067f89cc14862c::eccbc87e4b5ce2fe28308fd9f2a7baf3",
            "_amount": "17.23",
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
* **TX**: Transaction List [JSON]
  * _to: [to_routing::to_address]
  * _from: [from_routing::from_address]
  * _amount: [B$].[B¢]
  * _timestamp: [UNIX_TIMESTAMP]
  * _hash: SHA256([_amount]@[_timestamp])