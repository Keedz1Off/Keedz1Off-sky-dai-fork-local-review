# Deposit Flow: L1 -> L2

## Full Flow

```text
User on L1
-> L1TokenBridge.bridgeERC20(...) / bridgeERC20To(...)
-> L1TokenBridge._initiateBridgeERC20(...)
-> check bridge is open
-> check L1 token maps to correct L2 token
-> transferFrom(user, escrow, amount)
-> messenger.sendMessage(...)
-> L2 messenger relays message
-> L2TokenBridge.finalizeBridgeERC20(...)
-> onlyOtherBridge check
-> mint(to, amount) on L2
```

## Simple Meaning

```text
L1 tokens are locked in Escrow.
L2 tokens are minted to the recipient.
```

## Main Formula

```text
L1 escrowed amount = L2 minted amount
```

## Main Invariants

```text
1. L1 escrowed amount must equal L2 minted amount.
2. Only an authentic L1 -> L2 message can mint L2 tokens.
3. The L1 token must map to the correct L2 token.
```

## Additional Invariants / Checks

These are smaller architecture checks.

I keep them in the flow notes, but Break Think focuses mostly on the Main Invariants.

```text
The bridge must be open before creating a new message.
The recipient must be the intended recipient.
The message must target the correct L2 bridge.
The encoded amount must match the locked amount.
The token pair must be registered before deposit.
```

## Main Functions

```text
L1TokenBridge.bridgeERC20(...)
L1TokenBridge.bridgeERC20To(...)
L1TokenBridge._initiateBridgeERC20(...)
L2TokenBridge.finalizeBridgeERC20(...)
```

## Source Files

```text
src/L1TokenBridge.sol
src/L2TokenBridge.sol
src/Escrow.sol
```
