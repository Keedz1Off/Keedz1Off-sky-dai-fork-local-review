# Function Review: Spot.poke(...)

## Function Code

```solidity
function poke(bytes32 ilk) external {
    (bytes32 val, bool has) = ilks[ilk].pip.peek();
    uint256 spot = has ? rdiv(rdiv(mul(uint(val), 10 ** 9), par), ilks[ilk].mat) : 0;
    vat.file(ilk, "spot", spot);
    emit Poke(ilk, val, spot);
}
```

## What This Function Does

`Spot.poke(...)` updates the collateral price used by `Vat`.

It reads from the oracle, applies system parameters, and writes the new `spot` value into `Vat`.

## Important Logic Notes

### Oracle Read

```solidity
(bytes32 val, bool has) = ilks[ilk].pip.peek();
```

This reads the price from the oracle.

`has` tells whether the oracle value is valid.

### Spot Calculation

```solidity
uint256 spot = has ? rdiv(rdiv(mul(uint(val), 10 ** 9), par), ilks[ilk].mat) : 0;
```

This calculates the risk-adjusted collateral price.

Simple meaning:

```text
oracle price -> adjusted by par and liquidation ratio -> Vat spot price
```

### Update Vat

```solidity
vat.file(ilk, "spot", spot);
```

This writes the new collateral price into `Vat`.
