# Deposit Break Think

## L1TokenBridge.bridgeERC20(...)

```text
INVARIANT
1. The intended recipient must be preserved.
CONSEQUENCES

1.  L1 tokens will be sent to Escrow and may become stuck or L1 tokens may be minted to the wrong recipient.
```

## L1TokenBridge.bridgeERC20To(...)

```text
INVARIANT
The custom recipient must be preserved.

CONSEQUENCES
 L1 tokens will be sent to Escrow and may become stuck or L1 tokens may be minted to the wrong recipient.

```

## L1TokenBridge._initiateBridgeERC20(...)

```text
INVARIANT
L1 escrowed amount must equal L2 minted amount.
The L1 token must map to the correct L2 token.
The deposit message must target the correct L2 bridge.

CONSEQUENCES
1. If this invariant breaks, L2 may mint more or less tokens than were actually locked in Escrow on L1.
2. The token on L1 does not match the token on L2.
3. Wrong finalize function can execute the wrong bridge message.
```

## L2TokenBridge.finalizeBridgeERC20(...)

```text
INVARIANT
1. Only an authentic L1 -> L2 message can mint L2 tokens.
2. Minted amount must equal the L1 escrowed amount.
3. Minted token must be the correct L2 token for the L1 token.

CONSEQUENCES
1. This may lead to ghost mint
2.The bridge may mint more or lesstokens than were actually locked in Escrow.
3.A user may receive the wrong token.

```
