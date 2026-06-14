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

## Important Logic Notes

### DSR Accumulator Update

```solidity
tmp = _rmul(_rpow(dsr, now - rho, ONE), chi);
chi = tmp;
rho = now;
```

This updates the savings accumulator based on time and the DSR rate.

### Create Internal Dai for Savings Yield

```solidity
vat.suck(address(vow), address(this), _mul(Pie, chi_));
```

This creates the accounting effect for DSR yield.

### Join Savings

```solidity
pie[msg.sender] = _add(pie[msg.sender], wad);
Pie             = _add(Pie,             wad);
vat.move(msg.sender, address(this), _mul(chi, wad));
```

This deposits internal dai into the savings contract.

### Exit Savings

```solidity
pie[msg.sender] = _sub(pie[msg.sender], wad);
Pie             = _sub(Pie,             wad);
vat.move(address(this), msg.sender, _mul(chi, wad));
```

This withdraws internal dai from savings.
