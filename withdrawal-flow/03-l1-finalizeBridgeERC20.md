# Function Review: L1TokenBridge.finalizeBridgeERC20(...)

## Function Code

```solidity
function finalizeBridgeERC20(
    address _localToken,
    address _remoteToken,
    address _from,
    address _to,
    uint256 _amount,
    bytes calldata _extraData
)
    external
    onlyOtherBridge
{
    TokenLike(_localToken).transferFrom(escrow, _to, _amount);

    emit ERC20BridgeFinalized(_localToken, _remoteToken, _from, _to, _amount, _extraData);
}
```

Note:

```text
Original source: makerdao/op-token-bridge/src/L1TokenBridge.sol
```

## What This Function Does

This finalizes an L2 -> L1 withdrawal on L1.

Simple meaning:

```text
L1 bridge receives an authentic withdrawal message and releases tokens from Escrow.
```

## Important Logic Notes

### Auth Boundary

```solidity
onlyOtherBridge
```

This function should only run through the messenger path from the L2 bridge.

Users should not be able to call it directly.

### Release From Escrow

```solidity
TokenLike(_localToken).transferFrom(escrow, _to, _amount);
```

This transfers L1 tokens from the escrow to the withdrawal recipient.

Simple meaning:

```text
escrow -> user
```

## Main Invariants

```text
1. Only an authentic L2 -> L1 message can release L1 tokens.
2. Released amount must equal the L2 burned amount.
3. Released token must be the correct L1 token for the L2 token.
```

## Additional Invariants / Checks

```text
The recipient must be the intended recipient.
The finalize call must come through onlyOtherBridge.
Escrow must approve the bridge to transfer the token.
```
