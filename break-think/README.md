# Break Think Analysis

This folder is for manual Break Think analysis.

This file only lists the functions that should be reviewed manually later.

## Main Deposit Functions

```text
L1TokenBridge.bridgeERC20(...)
L1TokenBridge.bridgeERC20To(...)
L1TokenBridge._initiateBridgeERC20(...)
L2TokenBridge.finalizeBridgeERC20(...)
```

## Main Withdrawal Functions

```text
L2TokenBridge.bridgeERC20(...)
L2TokenBridge.bridgeERC20To(...)
L2TokenBridge._initiateBridgeERC20(...)
L1TokenBridge.finalizeBridgeERC20(...)
```

## Admin / Escrow Functions

```text
Escrow.approve(...)
L1TokenBridge.registerToken(...)
L2TokenBridge.registerToken(...)
L2TokenBridge.setMaxWithdraw(...)
L1TokenBridge.close(...)
L2TokenBridge.close(...)
```

## Format For Later

```text
INVARIANT

CONSEQUENCES
```
