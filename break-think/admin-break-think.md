# Admin / Escrow Break Think

## Escrow.approve(...)

```text
INVARIANT
Only authorized governance/admin can approve token spending from Escrow.
Escrow approval must be given only to the correct bridge/spender.

CONSEQUENCES


```

## L1TokenBridge.registerToken(...)

```text
INVARIANT
Only authorized governance/admin can register token pairs.
The registered L1 token must map to the correct L2 token.

CONSEQUENCES


```

## L2TokenBridge.registerToken(...)

```text
INVARIANT
Only authorized governance/admin can register token pairs.
The registered L2 token must map to the correct L1 token.

CONSEQUENCES


```

## L2TokenBridge.setMaxWithdraw(...)

```text
INVARIANT
Only authorized governance/admin can set maxWithdraw.
Withdrawal amount must not exceed maxWithdraw.

CONSEQUENCES


```

## L1TokenBridge.close(...)

```text
INVARIANT
Only authorized governance/admin can close the L1 bridge.
Closing the bridge must stop new outbound L1 -> L2 messages.

CONSEQUENCES


```

## L2TokenBridge.close(...)

```text
INVARIANT
Only authorized governance/admin can close the L2 bridge.
Closing the bridge must stop new outbound L2 -> L1 messages.

CONSEQUENCES


```
