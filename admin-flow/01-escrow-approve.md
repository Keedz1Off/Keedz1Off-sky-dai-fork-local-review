# Function Review: Escrow.approve(...)

## Function Code

```solidity
function approve(address token, address spender, uint256 value) external auth {
    GemLike(token).approve(spender, value);
    emit Approve(token, spender, value);
}
```

Note:

```text
Original source: makerdao/op-token-bridge/src/Escrow.sol
```

## What This Function Does

`Escrow.approve(...)` gives a spender approval to move tokens from the escrow.

In this bridge, the L1 bridge releases tokens from escrow during withdrawal finalization.

Simple meaning:

```text
Escrow holds L1 tokens.
Approved bridge can transfer tokens out when a valid withdrawal is finalized.
```

## Important Logic Notes

### Auth

```solidity
external auth
```

Only an authorized address can approve token spending from escrow.

### Token Approval

```solidity
GemLike(token).approve(spender, value);
```

This sets ERC20 allowance from Escrow to the spender.

## Main Invariants

```text
1. Only authorized governance/admin can approve token spending from Escrow.
```
