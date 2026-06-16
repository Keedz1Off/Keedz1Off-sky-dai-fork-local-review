# Admin / Escrow Break Think

## Governance / Admin Control

```text
INVARIANT
Only authorized governance/admin can change bridge configuration.

CONSEQUENCES
An unauthorized user may change token mapping, withdrawal limits, bridge status, or Escrow approvals.
```

## Escrow.approve(...)

```text
INVARIANT
Only authorized governance/admin can approve token spending from Escrow.
```

## L1TokenBridge.registerToken(...)

```text
INVARIANT
Only authorized governance/admin can register token pairs.
```

## L2TokenBridge.registerToken(...)

```text
INVARIANT
Only authorized governance/admin can register token pairs.
```

## L2TokenBridge.setMaxWithdraw(...)

```text
INVARIANT
Only authorized governance/admin can set maxWithdraw.
```

## L1TokenBridge.close(...)

```text
INVARIANT
Only authorized governance/admin can close the L1 bridge.
```

## L2TokenBridge.close(...)

```text
INVARIANT
Only authorized governance/admin can close the L2 bridge.
```
