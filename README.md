# RociFi: Under-Collateralized and Capital-Efficient Lending Protocol Example Datasets

This document provides specifications for the historical data events in the Roci.Fi protocol, focusing on two primary datasets:

1. **Liquidations** - Available at: [data/liquidations.json](data/liquidations.json)
2. **Repayments** - Available at: [data/repayments.json](data/repayments.json)

## Liquidation Events

Liquidation events occur when a loan is forcibly closed due to breaching protocol terms by maturity or price.

### Data Structure

| Field              | Type   | Description                                                 |
| ------------------ | ------ | ----------------------------------------------------------- |
| `id`               | String | Unique identifier for the liquidation event (composite key) |
| `loanId.id`        | String | Identifier for the liquidated loan                          |
| `borrower.id`      | String | Wallet address of the borrower                              |
| `borrower.nfcs.id` | String | NFT collateral identifier                                   |
| `collateralAmount` | String | Amount of collateral tokens in wei (18 decimals)            |
| `txHash`           | String | Transaction hash of the liquidation                         |
| `timestamp`        | String | Unix timestamp of the liquidation                           |
| `type`             | String | Liquidation trigger - "maturity" or "price"                 |

### Loan Data Substructure

| Field                     | Type   | Description                            |
| ------------------------- | ------ | -------------------------------------- |
| `data.loanStatus`         | Number | Status code of the loan (4 = CLOSED)   |
| `data.erc20Address`       | String | Address of collateral token            |
| `data.issueDate`          | String | Unix timestamp of loan creation        |
| `data.lt`                 | String | Liquidation threshold (18 decimals)    |
| `data.ltv`                | String | Loan-to-value ratio (18 decimals)      |
| `data.principal`          | String | Loan amount (6 decimals)               |
| `data.maturityDate`       | String | Unix timestamp of loan maturity        |
| `data.totalPaymentsValue` | String | Total amount repaid (6 decimals)       |
| `data.awaitingInterest`   | String | Outstanding interest (6 decimals)      |
| `data.awaitingCollection` | String | Funds awaiting collection (6 decimals) |
| `data.issueDateJS`        | String | Human-readable issue date              |
| `data.loanStatusTitle`    | String | Text representation of loan status     |

### Liquidation Types

- **Maturity**: Loan was liquidated after passing its maturity date without full repayment
- **Price**: Loan was liquidated because collateral value dropped below the LTV threshold

### Value Conversions

- Collateral amounts: Divide by 10^18 for token amount
- Monetary values: Divide by 10^6 for USD amount
- Percentages: Divide by 10^18 and multiply by 100 for percentage

## Repayment Events

Repayment events occur when borrowers make payments toward their loans.

### Data Structure

| Field                | Type   | Description                                    |
| -------------------- | ------ | ---------------------------------------------- |
| `id`                 | String | Unique identifier for the repayment event      |
| `amountRepaid`       | String | Amount repaid in this transaction (6 decimals) |
| `timestamp`          | String | Unix timestamp of the repayment                |
| `loanId.id`          | String | Identifier for the loan being repaid           |
| `loanId.loanStatus`  | Number | Status code of the loan after repayment        |
| `loanId.borrower.id` | String | Wallet address of the borrower                 |

### Value Conversions

- Repayment amount: Divide by 10^6 for USD amount

## Examples

### Liquidation Example

```json
{
  "id": "59921326030791161768114750123797638214491681772327044457514152415659706364463-0xec362ea53c2b5d74bb5b3e27b477b2585c4bfb0d5b92c014892242386a5c11ac",
  "loanId": {
    "id": "59921326030791161768114750123797638214491681772327044457514152415659706364463"
  },
  "borrower": {
    "id": "0xba8c56e07e1931203f2fdf6b429b7d05fbfa5483",
    "nfcs": {
      "id": "14402"
    }
  },
  "collateralAmount": "6706000000000000",
  "txHash": "0xec362ea53c2b5d74bb5b3e27b477b2585c4bfb0d5b92c014892242386a5c11ac",
  "timestamp": "1666872049",
  "data": {
    "id": "59921326030791161768114750123797638214491681772327044457514152415659706364463",
    "loanStatus": 4,
    "erc20Address": "0x2791bca1f2de4661ed88a30c99a7a9449aa84174",
    "issueDate": "1663847761",
    "borrower": {
      "id": "0xba8c56e07e1931203f2fdf6b429b7d05fbfa5483",
      "nfcs": {
        "id": "14402"
      }
    },
    "lt": "90000000000000000000",
    "ltv": "80000000000000000000",
    "principal": "10343159",
    "maturityDate": "1666439761",
    "totalPaymentsValue": "10397551",
    "awaitingInterest": "54392",
    "awaitingCollection": "0",
    "issueDateJS": "2022-09-22T11:56:01.000Z",
    "loanStatusTitle": "CLOSED"
  },
  "type": "maturity"
}
```

#### Analysis of Example Liquidation:

- Loan of 10.343159 USDC with 0.006706 tokens as collateral
- Liquidated due to maturity (maturity date: Oct 22, 2022)
- Loan was issued on Sept 22, 2022 (30-day term)
- Liquidation occurred on Oct 27, 2022 (5 days after maturity)
- Borrower repaid 10.397551 USDC (principal + partial interest)
- Outstanding interest at liquidation: 0.054392 USDC
- LTV was 80%, liquidation threshold was 90%

### Repayment Examples

```json
{
  "id": "7583130299395289065979796000078404567059143367832213056998704650073498960546-0x29a6f7e3786641a86e2c7d962eece838cef83d3f9ea734020aee41ca434fc38a",
  "amountRepaid": "4001491",
  "timestamp": "1657091671",
  "loanId": {
    "id": "7583130299395289065979796000078404567059143367832213056998704650073498960546",
    "loanStatus": 4,
    "borrower": {
      "id": "0x85c6a8fbaee3cb4c273bebe5916b0716a6baa721"
    }
  }
}
```

```json
{
  "id": "75857789280660338204844155585696308331708677211276633158563129609050840671254-0x002ad1e3a161ce1303b88233a8030bcb5fd461494d99a7183e874a3e1fbabcd6",
  "amountRepaid": "5856751",
  "timestamp": "1661801742",
  "loanId": {
    "id": "75857789280660338204844155585696308331708677211276633158563129609050840671254",
    "loanStatus": 4,
    "borrower": {
      "id": "0x8eef779818afa953b0652e45438423ebe089f55a"
    }
  }
}
```

#### Analysis of Example Repayments:

- First repayment: 4.001491 USDC on July 6, 2022
- Second repayment: 5.856751 USDC on August 29, 2022
- Both loans are marked as closed (status 4) after these repayments
