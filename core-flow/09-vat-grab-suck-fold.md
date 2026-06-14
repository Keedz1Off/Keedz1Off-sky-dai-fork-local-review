# Function Review: Vat.grab(...) / Vat.suck(...) / Vat.fold(...)

## Function Code

```solidity
function grab(bytes32 i, address u, address v, address w, int dink, int dart) external auth {
    Urn storage urn = urns[i][u];
    Ilk storage ilk = ilks[i];

    urn.ink = _add(urn.ink, dink);
    urn.art = _add(urn.art, dart);
    ilk.Art = _add(ilk.Art, dart);

    int dtab = _mul(ilk.rate, dart);

    gem[i][v] = _sub(gem[i][v], dink);
    sin[w]    = _sub(sin[w],    dtab);
    vice      = _sub(vice,      dtab);
}

function suck(address u, address v, uint rad) external auth {
    sin[u] = _add(sin[u], rad);
    dai[v] = _add(dai[v], rad);
    vice   = _add(vice,   rad);
    debt   = _add(debt,   rad);
}

function fold(bytes32 i, address u, int rate) external auth {
    require(live == 1, "Vat/not-live");
    Ilk storage ilk = ilks[i];
    ilk.rate = _add(ilk.rate, rate);
    int rad  = _mul(ilk.Art, rate);
    dai[u]   = _add(dai[u], rad);
    debt     = _add(debt,   rad);
}
```

## What These Functions Do

These functions are high-impact accounting functions inside `Vat`.

`grab(...)` is used during liquidation to confiscate vault collateral and debt.

`suck(...)` creates unbacked internal dai and system debt.

`fold(...)` updates collateral rate accounting and system debt.

## Important Logic Notes

### Grab

```solidity
urn.ink = _add(urn.ink, dink);
urn.art = _add(urn.art, dart);
ilk.Art = _add(ilk.Art, dart);
```

This changes vault collateral and debt during liquidation.

### Suck

```solidity
sin[u] = _add(sin[u], rad);
dai[v] = _add(dai[v], rad);
vice   = _add(vice,   rad);
debt   = _add(debt,   rad);
```

This creates system debt and internal dai at the same time.

### Fold

```solidity
ilk.rate = _add(ilk.rate, rate);
int rad  = _mul(ilk.Art, rate);
dai[u]   = _add(dai[u], rad);
debt     = _add(debt,   rad);
```

This updates accumulated rate and adjusts system debt.
