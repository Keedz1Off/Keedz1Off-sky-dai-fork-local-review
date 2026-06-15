# Withdrawal Flow: L2 -> L1

## Full Flow

```text
User on L2
-> L2TokenBridge.bridgeERC20(...) / bridgeERC20To(...)
-> L2TokenBridge._initiateBridgeERC20(...)
-> check bridge is open
-> check L2 token maps to correct L1 token
-> check amount <= maxWithdraw
-> burn(user, amount) on L2
-> messenger.sendMessage(...)
-> OP withdrawal proving / finalization delay
-> L1TokenBridge.finalizeBridgeERC20(...)
-> onlyOtherBridge check
-> transferFrom(escrow, to, amount)
```

## Simple Meaning

```text
L2 tokens are burned.
L1 tokens are released from Escrow.
```

## Main Formula

```text
L2 burned amount = L1 released amount
```

## Main Invariants

```text
1. L2 burned amount must equal L1 released amount.
2. Only an authentic L2 -> L1 message can release L1 tokens.
3. The L2 token must map to the correct L1 token.
```

## Additional Invariants / Checks

These are smaller architecture checks.

I keep them in the flow notes, but Break Think focuses mostly on the Main Invariants.

```text
The bridge must be open before creating a new message.
The withdrawal amount must not exceed maxWithdraw.
The recipient must be the intended recipient.
The message must target the correct L1 bridge.
The encoded amount must match the burned amount.
```

## Main Functions

```text
L2TokenBridge.bridgeERC20(...)
L2TokenBridge.bridgeERC20To(...)
L2TokenBridge._initiateBridgeERC20(...)
L1TokenBridge.finalizeBridgeERC20(...)
```

## Source Files

```text
src/L2TokenBridge.sol
src/L1TokenBridge.sol
src/Escrow.sol
```
