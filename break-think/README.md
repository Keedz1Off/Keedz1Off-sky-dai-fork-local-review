# Break Think Analysis

This folder is for manual Break Think analysis.

This file only lists the functions that should be reviewed manually later.

## Files

```text
deposit-break-think.md
withdrawal-break-think.md
admin-break-think.md
```

## Break Think Focus

```text
Main Invariants are the main focus here.
Additional Invariants / Checks are kept in the flow notes and function explanations.
```

Simple meaning:

```text
Break Think = what happens if a core bridge invariant breaks.
```

Additional checks are still useful, but I do not treat every small check as a full Break Think item.

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

## Main Deposit Invariants

```text
L1 escrowed amount must equal L2 minted amount.
Only an authentic L1 -> L2 message can mint L2 tokens.
The L1 token must map to the correct L2 token.
```

## Main Withdrawal Invariants

```text
L2 burned amount must equal L1 released amount.
Only an authentic L2 -> L1 message can release L1 tokens.
The L2 token must map to the correct L1 token.
```

## Additional Checks

```text
Bridge must be open.
Recipient must be the intended recipient.
Withdrawal amount must not exceed maxWithdraw.
Escrow approval must be given only to the correct bridge.
Only authorized governance/admin can call admin functions.
```
