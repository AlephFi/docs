# Batch Processing

The system operates on a batched processing model where user requests are queued and settled periodically. All requests in a Batch use the same NAV pricing inputs.

#### Batch Structure and Rules

* Batch ID: A unique, incrementing `batchId` identifies each batch.
* Request Limits: Each allocator is limited to submitting one deposit and one redemption request per share class per batch.
* Settlement Order: Outstanding batches are always settled in strict chronological order, from oldest to newest. The system will not process the current batch until all previous batches have been settled.

### Batch Processing

The batch processing cycle consists of two distinct phases: the Request Phase and the Settlement Phase.

1. **Request Phase:** During this phase, users submit their deposit and redemption requests for the current batch. The following functions are used:
   * `requestDeposit(RequestDepositParams calldata _requestDepositParams)`
   * `requestRedeem(uint8 _classId, uint256 _shareUnits)`
2. **Settlement Phase:** This phase begins at the end of the batch window. The Oracle calls the settlement functions to finalize all outstanding requests.
   * `settleDeposit(SettlemenParams calldata _settlementParams)` \
     Shares are minted and allocated to the user.&#x20;
   * `settleRedeem(SettlemenParams calldata _settlementParams)` \
     Tokens are transferred to the user.

