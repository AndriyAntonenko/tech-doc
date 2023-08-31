# Service S public API spec

### Get or create new address

- **Path:** `/addresses`
- **Method:** `GET`
- **Authorization:** Required
- **Response:**

  ```json
  {
    "address": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
    "userId": "..."
  }
  ```

### Get user balances

- **Path:** `/balances`
- **Method:** `GET`
- **Authorization:** Required
- **Response:**
  ```json
  [
    {
      "asset": "0x0000000000000000000000000000000000000000",
      "value": "1000000000000000000",
      "formatted": "1.0",
      "decimals": 18
    },
    ...
  ]
  ```

### Estimate withdrawal fee

- **Path:** `/withdrawals/fee`
- **Method:** `GET`
- **Authorization:** Required
- **Query Params:**
  - `destination`: address to send `asset`
  - `asset`: asset identifier (`0x0000000000000000000000000000000000000000` for ETH)
  - `value`: value in wei
- **Response:**
  ```json
  {
    "destination": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
    "asset": "0x0000000000000000000000000000000000000000",
    "value": "1000000000000000000",
    "fee": "1000000000000000",
    "formattedFee": "0.001"
  }
  ```

### Create withdrawal

- **Path:** `/withdrawals`
- **Method:** `POST`
- **Authorization:** Required
- **Request:**
  ```json
  {
    "destination": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
    "asset": "0x0000000000000000000000000000000000000000",
    "value": "1000000000000000000",
    "fee": "1000000000000000"
  }
  ```
- **Response:**
  ```json
  {
    "destination": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
    "asset": "0x0000000000000000000000000000000000000000",
    "value": "1000000000000000000",
    "formattedValue": "1.0",
    "fee": "1000000000000000",
    "formattedFee": "0.001",
    "txHash": "0xb5c8bd9430b6cc87a0e2fe110ece6bf527fa4f170a4bc8cd032f768fc5219838"
  }
  ```

## Error Codes

Every API method could return some error codes:

- 400 - Invalid request or Business logic error
- 401 - Unauthorized
- 500 - Unexpected server error

If error is a business logic error, then it will also contains response body of such format:

```json
{
  "errorCode": -38001,
  "errorMessage": "Not enough balance"
}
```

You can find all possible error codes below:

| Error code | Error message           | Explanation                                                             |
| ---------- | ----------------------- | ----------------------------------------------------------------------- |
| -32001     | Not enough balance      | The `amount + fee` which user trying to withdraw is larger then balance |
| -32002     | Not enough liquidity    | Root address doesn't have enough on-chain balance to withdraw assets    |
| -32003     | Fee less then estimated | Fee provided to withdrawal method is less then new estimated fee        |
