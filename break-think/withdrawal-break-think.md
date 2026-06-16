# Withdrawal Break Think

## L2TokenBridge.bridgeERC20(...)

```text
INVARIANT
The intended L1 recipient must be preserved.

CONSEQUENCES
The tokens will be burned on L2, but may be released to the wrong L1 recipent.   

```

## L2TokenBridge.bridgeERC20To(...)

```text
INVARIANT
The custom L1 recipient must be preserved.


CONSEQUENCES
The tokens will be burned on L2, but may be released to the wrong L1 recipent.
(as in L2TokenBridge.bridgeERC20() )

```

## L2TokenBridge._initiateBridgeERC20(...)

```text
INVARIANT
L2 burned amount must equal L1 released amount.

The L2 token must map to the correct L1 token.

The withdrawal message must be created only after the burn step.


CONSEQUENCES

The bridge may release more tokens than were actually burned.

The L2 token does not match the L1 token

This may lead to releasing on L1 without burning on L1







```

## L1TokenBridge.finalizeBridgeERC20(...)

```text
INVARIANT
Only an authentic L2 -> L1 message can release L1 tokens.

Released amount must equal the L2 burned amount.

Released token must be the correct L1 token for the L2 token.


CONSEQUENCES

This may lead to spoofed message execution therefore, the fake finalization may happen (becomes possible)

The bridge may release more tokens than actually burned.

A user receives the wrong token instead of inteded token.


```
