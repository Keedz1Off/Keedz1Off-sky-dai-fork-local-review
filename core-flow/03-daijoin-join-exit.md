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

## Important Logic Notes

### Join DAI

```solidity
vat.move(address(this), usr, mul(ONE, wad));
dai.burn(msg.sender, wad);
```

This burns external ERC20 DAI and credits internal Vat dai.

Simple meaning:

```text
external DAI decreases
internal Vat dai increases
```

### Exit DAI

```solidity
vat.move(msg.sender, address(this), mul(ONE, wad));
dai.mint(usr, wad);
```

This moves internal Vat dai into the adapter and mints external ERC20 DAI.

Simple meaning:

```text
internal Vat dai decreases
external DAI increases
```

### Wad / Rad Conversion

```solidity
mul(ONE, wad)
```

DSS uses higher precision internally. External DAI uses `wad`, internal Vat accounting uses `rad`.
