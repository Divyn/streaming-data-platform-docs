---
sidebar_position: 4
---

# Token Holders API

The Token Holders API provides information on the holders of a specific token on a supported blockchain. This API enables developers to retrieve a list of addresses that hold a particular token, as well as additional information about each holder.

## Top 10 Token Holders
Here's an example query to retrieve the top 10 balance updates for a specific token contract on the Ethereum network:

```graphql
query MyQuery {
  EVM(dataset: combined, network: eth) {
    BalanceUpdates(
      orderBy: {descendingByField: "Balance"}
      limit: {count: 10}
      where: {Currency: {SmartContract: {is: "0x5Af0D9827E0c53E4799BB226655A1de152A425a5"}}, Block: {Date: {after: "2023-02-01"}}}
    ) {
      BalanceUpdate {
        Address
      }
      Balance: sum(of: BalanceUpdate_Amount, selectWhere: {gt: "0"})
    }
  }
}

```
In this query, you'll need to replace "0x5Af0D9827E0c53E4799BB226655A1de152A425a5" with the contract address of the token you'd like to retrieve balance updates for.

**Parameters**
-   `dataset: combined`: This parameter specifies that the dataset has both realtime and archive data
-   `network: eth`: This parameter specifies that the Ethereum network is being queried.
-   `orderBy: {descendingByField: "Balance"}`: This parameter orders the results of the query by the `Balance` field in descending order, meaning the highest balances will appear first.
-   `limit: {count: 10}`: This parameter limits the number of results returned to 10.
-   `where: {Currency: {SmartContract: {is: "0x5Af0D9827E0c53E4799BB226655A1de152A425a5"}}, Block: {Date: {after: "2023-02-01"}}}`: This parameter filters the results of the query based on the smart contract address "0x3ee2200efb3400fabb9aacf31297cbdd1d435d47" and the block date after "2023-02-01". The `Currency` field specifies the currency to filter by, and the `SmartContract` field specifies the smart contract address to filter by. The `Block` field specifies the block to filter by, and the `Date` field specifies the date to filter by.

**Returned Data**
-   `Balance: sum(of: BalanceUpdate_Amount)`: This field specifies the address and the balance amount in the results.
-   `Currency { Name }`: This field specifies the currency in which the balance is expressed. In this case, the `Name` of the currency is retrieved.

Here's a sample of the response:

```
 "BalanceUpdates": [
      {
        "Balance": "431",
        "BalanceUpdate": {
          "Address": "0x29469395eaf6f95920e59f858042f0e28d98a20b"
        }
      },
      {
        "Balance": "140",
        "BalanceUpdate": {
          "Address": "0x398d282487b44b6e53ce0aebca3cb60c3b6325e9"
        }
      },
```
You can find the graphql query [here](https://ide.bitquery.io/top-MILADY-MAKER-NFT-holders).

## Top Trending tokens based on Token holders
Here's an example query to fetch the top 10 trending tokens based on their holders on the Ethereum network:

```graphql
query MyQuery {
  EVM(network: eth, dataset: combined) {
    BalanceUpdates(
      where: { Block: {Date: {since: "2023-06-01"}}, BalanceUpdate: {Amount: {gt: "0"}}}
      orderBy: {descendingByField: "No_Holders"}
      limit: {count: 10}
    ) {
      No_Holders: count(distinct: BalanceUpdate_Address)
      Currency {
        Name
        SmartContract
      }
    }
  }
}

```

**Parameters**
-   `dataset: combined`: This parameter specifies that the dataset has both realtime and archive data.
-   `network: eth`: This parameter specifies that the Ethereum network is being queried.
-   `orderBy: {descendingByField: "No_Holders"}`: This parameter orders the results of the query by the `No_Holders` field in descending order, meaning the currency with the highest number of holders will appear first in the results.
-   `limit: {count: 10}`: This parameter limits the number of results returned to 10.
-   `where: { Block: {Date: {since: "2023-06-01"}}, BalanceUpdate: {Amount: {gt: "0"}}}`: The where parameter in the query filters the results based on mentioned conditions. The `Block` field specifies the block to filter by, and the `Date` field specifies the date to filter by. `BalanceUpdate: {Amount: {gt: "0"}}` filters the results to include only balance updates with amounts greater than zero.

**Returned Data**
-   `No_Holders: count(distinct: BalanceUpdate_Address)`: This field specifies the number of holders of token in the results.
-   `Currency { Name SmartContract }`: This field specifies the Currency details. In this case, the `Name` represents the name of the currency  and the `SmartContract` field, which contains the address of the currency's smart contract.

Here's a sample of the response:

```
"BalanceUpdates": [
        {
          "Currency": {
            "Name": "",
            "SmartContract": "0x"
          },
          "No_Holders": "6951766"
        },
        {
          "Currency": {
            "Name": "Wrapped Ether",
            "SmartContract": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2"
          },
          "No_Holders": "3307186"
        },
```
You can find the graphql query [here](https://ide.bitquery.io/Trending_Token_based_on_holders).

## Top NFT tokens based on holders
Here's an example query to fetch the Top 10 NFT tokens based on their holders on the Ethereum network:

```graphql
query MyQuery {
  EVM(network: eth, dataset: combined) {
    BalanceUpdates(
      where: {Currency: {Fungible: false}, Block: {Date: {since: "2023-06-01"}}, BalanceUpdate: {Amount: {gt: "0"}}}
      orderBy: {descendingByField: "No_Holders"}
      limit: {count: 10}
    ) {
      No_Holders: count(distinct: BalanceUpdate_Address)
      Currency {
        Name
        SmartContract
      }
    }
  }
}
```

**Parameters**
-   `dataset: combined`: This parameter specifies that the dataset has both realtime and archive data
-   `network: eth`: This parameter specifies that the Ethereum network is being queried.
-   `orderBy: {descendingByField: "No_Holders"}`: This parameter orders the results of the query by the `No_Holders` field in descending order, meaning the currency with the highest number of holders will appear first.
-   `limit: {count: 10}`: This parameter limits the number of results returned to 10.
-   `where: {Currency: {Fungible: false}, Block: {Date: {since: "2023-06-01"}}, BalanceUpdate: {Amount: {gt: "0"}}}`: It filters the results based on specific conditions. `Currency: { Fungible: false }` filters out fungible currencies, indicating only non-fungible tokens to be included. The `Block` field specifies the block to filter by, and the `Date` field specifies the date to filter by. `BalanceUpdate: {Amount: {gt: "0"}}` filters the results to include only balance updates with amounts greater than zero.

**Returned Data**
-   `No_Holders: count(distinct: BalanceUpdate_Address)`: This field specifies the number of holders of particular token in the results.
-   `Currency { Name SmartContract }`: This field specifies the currency. In this case, the `Name` represents the name of the currency  and the `SmartContract` field, represents the address of the currency's smart contract.

Here's a sample of the response:

```
"BalanceUpdates": [
        {
          "Currency": {
            "Name": "XTREME PIXELS",
            "SmartContract": "0x0c9663115b36fa95d18e71d59054117bcb0342ef"
          },
          "No_Holders": "65040"
        },
        {
          "Currency": {
            "Name": "ARGUS GENESIS ◢ ✦ ◣",
            "SmartContract": "0x9d7987d74c0b3ca8e8472f90c713c456dd656be8"
          },
          "No_Holders": "29118"
        },
```
You can find the graphql query [here](https://ide.bitquery.io/Top_NFTs_based_on_token_holder).

