# Function Review: Vat.frob(...)

## Function Code

```solidity
function frob(bytes32 i, address u, address v, address w, int dink, int dart) external {
    require(live == 1, "Vat/not-live");

    Urn memory urn = urns[i][u];
    Ilk memory ilk = ilks[i];
    require(ilk.rate != 0, "Vat/ilk-not-init");

    urn.ink = _add(urn.ink, dink);
    urn.art = _add(urn.art, dart);
    ilk.Art = _add(ilk.Art, dart);

    int dtab = _mul(ilk.rate, dart);
    uint tab = _mul(ilk.rate, urn.art);
    debt     = _add(debt, dtab);

    require(either(dart <= 0, both(_mul(ilk.Art, ilk.rate) <= ilk.line, debt <= Line)), "Vat/ceiling-exceeded");
    require(either(both(dart <= 0, dink >= 0), tab <= _mul(urn.ink, ilk.spot)), "Vat/not-safe");

    require(either(both(dart <= 0, dink >= 0), wish(u, msg.sender)), "Vat/not-allowed-u");
    require(either(dink <= 0, wish(v, msg.sender)), "Vat/not-allowed-v");
    require(either(dart >= 0, wish(w, msg.sender)), "Vat/not-allowed-w");

    require(either(urn.art == 0, tab >= ilk.dust), "Vat/dust");

    gem[i][v] = _sub(gem[i][v], dink);
    dai[w]    = _add(dai[w],    dtab);

    urns[i][u] = urn;
    ilks[i]    = ilk;
}
```

## What This Function Does

`Vat.frob(...)` is the core vault modification function.

It changes:

- vault collateral
- vault debt
- total collateral debt
- global system debt
- internal collateral and dai balances

## Invariants

### Main Invariant 1

```text
Vault debt must stay safely collateralized after the operation.
```

### Main Invariant 2

```text
Debt ceilings must not be exceeded.
```

### Main Invariant 3

```text
Only authorized users may modify vault collateral or debt.
```

## Additional Invariants

### Additional Invariant 1

```text
The collateral type must be initialized.
```

### Additional Invariant 2

```text
Vault debt must be zero or above the dust limit.
```

### Additional Invariant 3

```text
Internal dai and gem accounting must match the vault modification.
```
