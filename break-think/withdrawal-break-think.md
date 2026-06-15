# Withdrawal Break Think

## L2TokenBridge.bridgeERC20(...)

```text
INVARIANT
The intended L1 recipient must be preserved.

CONSEQUENCES


```

## L2TokenBridge.bridgeERC20To(...)

```text
INVARIANT
The custom L1 recipient must be preserved.

CONSEQUENCES


```

## L2TokenBridge._initiateBridgeERC20(...)

```text
INVARIANT
L2 burned amount must equal L1 released amount.
The L2 token must map to the correct L1 token.
The withdrawal message must be created only after the burn step.

CONSEQUENCES


```

## L1TokenBridge.finalizeBridgeERC20(...)

```text
INVARIANT
Only an authentic L2 -> L1 message can release L1 tokens.
Released amount must equal the L2 burned amount.
Released token must be the correct L1 token for the L2 token.

CONSEQUENCES


```
