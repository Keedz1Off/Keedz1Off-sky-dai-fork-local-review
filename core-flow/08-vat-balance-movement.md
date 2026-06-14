# Function Review: Vat.slip(...) / Vat.flux(...) / Vat.move(...)

## Function Code

```solidity
function slip(bytes32 ilk, address usr, int256 wad) external auth {
    gem[ilk][usr] = _add(gem[ilk][usr], wad);
}

function flux(bytes32 ilk, address src, address dst, uint256 wad) external {
    require(wish(src, msg.sender), "Vat/not-allowed");
    gem[ilk][src] = _sub(gem[ilk][src], wad);
    gem[ilk][dst] = _add(gem[ilk][dst], wad);
}

function move(address src, address dst, uint256 rad) external {
    require(wish(src, msg.sender), "Vat/not-allowed");
    dai[src] = _sub(dai[src], rad);
    dai[dst] = _add(dai[dst], rad);
}
```

## What These Functions Do

These are internal accounting movement functions inside `Vat`.

`slip(...)` changes internal collateral balance.

`flux(...)` moves internal collateral balance between users.

`move(...)` moves internal dai balance between users.

## Important Logic Notes

### Collateral Balance Update

```solidity
gem[ilk][usr] = _add(gem[ilk][usr], wad);
```

This changes the internal collateral balance for a user and collateral type.

### Collateral Transfer

```solidity
gem[ilk][src] = _sub(gem[ilk][src], wad);
gem[ilk][dst] = _add(gem[ilk][dst], wad);
```

This moves internal collateral from one Vat account to another.

### Internal Dai Transfer

```solidity
dai[src] = _sub(dai[src], rad);
dai[dst] = _add(dai[dst], rad);
```

This moves internal dai balance.

Simple meaning:

```text
Vat does not move ERC20 tokens here.
It moves internal accounting balances.
```
