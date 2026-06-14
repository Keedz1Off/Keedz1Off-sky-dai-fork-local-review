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

## Important Logic Notes

### Time-Based Rate Update

```solidity
rate = _rmul(_rpow(_add(base, ilks[ilk].duty), now - ilks[ilk].rho, ONE), prev);
```

This calculates the new accumulated stability fee rate.

Simple meaning:

```text
old rate + time passed + fee settings = new rate
```

### Apply Fee to Vat

```solidity
vat.fold(ilk, vow, _diff(rate, prev));
```

This applies the rate change to Vat accounting and sends the fee effect to `vow`.

### Timestamp Update

```solidity
ilks[ilk].rho = now;
```

This records that the collateral type has been updated to the current time.
