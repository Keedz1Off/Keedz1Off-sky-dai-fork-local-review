# Function Review: DaiJoin.join(...) / DaiJoin.exit(...)

## Function Code

```solidity
function join(address usr, uint wad) external {
    vat.move(address(this), usr, mul(ONE, wad));
    dai.burn(msg.sender, wad);
    emit Join(usr, wad);
}

function exit(address usr, uint wad) external {
    require(live == 1, "DaiJoin/not-live");
    vat.move(msg.sender, address(this), mul(ONE, wad));
    dai.mint(usr, wad);
    emit Exit(usr, wad);
}
```

## What This Function Does

`DaiJoin` connects external ERC20 DAI with internal Vat dai accounting.

`join(...)`:

```text
burn ERC20 DAI
credit internal Vat dai
```

`exit(...)`:

```text
move internal Vat dai to adapter
mint ERC20 DAI
```

## Invariants

### Main Invariant 1

```text
ERC20 DAI burn/mint must match internal Vat dai movement.
```

### Main Invariant 2

```text
DaiJoin.exit(...) must not mint DAI without internal Vat dai backing.
```

### Main Invariant 3

```text
DaiJoin.join(...) must not credit internal dai without burning ERC20 DAI.
```

## Additional Invariants

### Additional Invariant 1

```text
Exit must not work when the adapter is not live.
```

### Additional Invariant 2

```text
Wad-to-rad conversion must be correct.
```

### Additional Invariant 3

```text
The user receiving minted DAI must be the intended recipient.
```
