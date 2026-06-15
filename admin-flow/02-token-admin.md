# Function Review: Token Admin Functions

## Function Code

```solidity
function registerToken(address l1Token, address l2Token) external auth {
    l1ToL2Token[l1Token] = l2Token;
    emit TokenSet(l1Token, l2Token);
}

function setMaxWithdraw(address l2Token, uint256 maxWithdraw) external auth {
    maxWithdraws[l2Token] = maxWithdraw;
    emit MaxWithdrawSet(l2Token, maxWithdraw);
}

function close() external auth {
    isOpen = 0;
    emit Closed();
}
```

Note:

```text
Original source:
makerdao/op-token-bridge/src/L1TokenBridge.sol
makerdao/op-token-bridge/src/L2TokenBridge.sol
```

## What These Functions Do

These are admin functions that control bridge configuration.

`registerToken(...)` sets the L1 token -> L2 token mapping.

`setMaxWithdraw(...)` sets the maximum withdrawal amount for an L2 token.

`close(...)` disables new outbound bridge messages.

## Important Logic Notes

### Token Mapping

```solidity
l1ToL2Token[l1Token] = l2Token;
```

This defines which L2 token corresponds to an L1 token.

### Withdrawal Limit

```solidity
maxWithdraws[l2Token] = maxWithdraw;
```

This limits withdrawal size for a token on L2.

### Close Bridge

```solidity
isOpen = 0;
```

This prevents new bridge messages from being initiated.

Old in-flight messages may still need to finish.

## Main Invariants

```text
1. Only authorized governance/admin can change bridge configuration.
```

## Additional Invariants / Checks

```text
Access control is the main check for these admin functions.
```
