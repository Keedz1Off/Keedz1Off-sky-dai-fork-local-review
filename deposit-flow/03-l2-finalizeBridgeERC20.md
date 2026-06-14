# Function Review: L2TokenBridge.finalizeBridgeERC20(...)

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
    TokenLike(_localToken).mint(_to, _amount);

    emit ERC20BridgeFinalized(_localToken, _remoteToken, _from, _to, _amount, _extraData);
}
```

Note:

```text
Original source: makerdao/op-token-bridge/src/L2TokenBridge.sol
```

## What This Function Does

This finalizes an L1 -> L2 deposit on L2.

Simple meaning:

```text
L2 bridge receives an authentic message from L1 bridge and mints L2 tokens.
```

## Important Logic Notes

### Auth Boundary

```solidity
onlyOtherBridge
```

This function should only run through the messenger path from the L1 bridge.

Users should not be able to call it directly.

### Mint

```solidity
TokenLike(_localToken).mint(_to, _amount);
```

This creates L2 tokens for the recipient.

Simple meaning:

```text
locked on L1 -> minted on L2
```

