# Function Review: GemJoin.join(...) / GemJoin.exit(...)

## Function Code

```solidity
function join(address usr, uint wad) external {
    require(live == 1, "GemJoin/not-live");
    require(int(wad) >= 0, "GemJoin/overflow");
    vat.slip(ilk, usr, int(wad));
    require(gem.transferFrom(msg.sender, address(this), wad), "GemJoin/failed-transfer");
    emit Join(usr, wad);
}

function exit(address usr, uint wad) external {
    require(wad <= 2 ** 255, "GemJoin/overflow");
    vat.slip(ilk, msg.sender, -int(wad));
    require(gem.transfer(usr, wad), "GemJoin/failed-transfer");
    emit Exit(usr, wad);
}
```

## What This Function Does

`GemJoin` moves external collateral tokens into or out of the DSS internal accounting system.

`join(...)`:

```text
external collateral token -> GemJoin adapter
Vat collateral balance increases
```

`exit(...)`:

```text
Vat collateral balance decreases
GemJoin adapter -> external collateral token
```

## Invariants

### Main Invariant 1

```text
External collateral transfer must match Vat collateral accounting.
```

### Main Invariant 2

```text
Join must not work when the adapter is not live.
```

### Main Invariant 3

```text
Exit must not release more collateral than the user's internal Vat balance.
```

## Additional Invariants

### Additional Invariant 1

```text
Token transfer failure must revert the operation.
```

### Additional Invariant 2

```text
The collateral type must be the intended ilk.
```

### Additional Invariant 3

```text
The adapter must not credit collateral without receiving tokens.
```
