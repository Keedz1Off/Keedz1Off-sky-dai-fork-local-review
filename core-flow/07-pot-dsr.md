# Function Review: Pot.drip(...) / Pot.join(...) / Pot.exit(...)

## Function Code

```solidity
function drip() external returns (uint tmp) {
    require(now >= rho, "Pot/invalid-now");
    tmp = _rmul(_rpow(dsr, now - rho, ONE), chi);
    uint chi_ = _sub(tmp, chi);
    chi = tmp;
    rho = now;
    vat.suck(address(vow), address(this), _mul(Pie, chi_));
}

function join(uint wad) external {
    require(now == rho, "Pot/rho-not-updated");
    pie[msg.sender] = _add(pie[msg.sender], wad);
    Pie             = _add(Pie,             wad);
    vat.move(msg.sender, address(this), _mul(chi, wad));
}

function exit(uint wad) external {
    pie[msg.sender] = _sub(pie[msg.sender], wad);
    Pie             = _sub(Pie,             wad);
    vat.move(address(this), msg.sender, _mul(chi, wad));
}
```

## What This Function Does

`Pot` manages the Dai Savings Rate.

`drip(...)` updates the savings accumulator.

`join(...)` deposits internal dai into savings.

`exit(...)` withdraws internal dai from savings.

## Invariants

### Main Invariant 1

```text
Savings rate accounting must be updated before users join.
```

### Main Invariant 2

```text
User pie balance and total Pie must update consistently.
```

### Main Invariant 3

```text
Vat dai movement must match the chi-adjusted savings amount.
```

## Additional Invariants

### Additional Invariant 1

```text
Drip must not use an invalid timestamp.
```

### Additional Invariant 2

```text
Exit must not withdraw more pie than the user has.
```
