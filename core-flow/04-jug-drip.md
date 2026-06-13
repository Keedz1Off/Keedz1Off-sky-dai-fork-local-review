# Function Review: Jug.drip(...)

## Function Code

```solidity
function drip(bytes32 ilk) external returns (uint rate) {
    require(now >= ilks[ilk].rho, "Jug/invalid-now");
    (, uint prev) = vat.ilks(ilk);
    rate = _rmul(_rpow(_add(base, ilks[ilk].duty), now - ilks[ilk].rho, ONE), prev);
    vat.fold(ilk, vow, _diff(rate, prev));
    ilks[ilk].rho = now;
}
```

## What This Function Does

`Jug.drip(...)` updates the accumulated stability fee rate for a collateral type.

It increases or updates debt accounting based on:

- base rate
- collateral-specific duty
- time passed since last update

## Invariants

### Main Invariant 1

```text
Rate accrual must be based on elapsed time since the last update.
```

### Main Invariant 2

```text
Vat debt accounting must be updated consistently with the new rate.
```

### Main Invariant 3

```text
The rate timestamp must move forward correctly.
```

## Additional Invariants

### Additional Invariant 1

```text
Drip must not use a timestamp older than the previous rho.
```

### Additional Invariant 2

```text
The fee delta must be sent to the correct vow address.
```
