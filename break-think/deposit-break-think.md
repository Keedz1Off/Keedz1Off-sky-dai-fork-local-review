# Deposit Break Think

## L1TokenBridge.bridgeERC20(...)

```text
INVARIANT
The intended recipient must be preserved.

CONSEQUENCES

1.  L1 tokens will be sent to Escrow or L1 tokens may be minted to the wrong recipient.

```

## L1TokenBridge.bridgeERC20To(...)

```text
INVARIANT
The custom recipient must be preserved.

CONSEQUENCES


```

## L1TokenBridge._initiateBridgeERC20(...)

```text
INVARIANT
L1 escrowed amount must equal L2 minted amount.
The L1 token must map to the correct L2 token.
The deposit message must target the correct L2 bridge.

CONSEQUENCES


```

## L2TokenBridge.finalizeBridgeERC20(...)

```text
INVARIANT
Only an authentic L1 -> L2 message can mint L2 tokens.
Minted amount must equal the L1 escrowed amount.
Minted token must be the correct L2 token for the L1 token.

CONSEQUENCES


```
