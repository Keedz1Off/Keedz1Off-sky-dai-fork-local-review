# Function Review: Vow.flog(...) / Vow.heal(...) / Vow.flop(...) / Vow.flap(...)

## Function Code

```solidity
function flog(uint era) external {
    require(add(era, wait) <= now, "Vow/wait-not-finished");
    Sin = sub(Sin, sin[era]);
    sin[era] = 0;
}

function heal(uint rad) external {
    require(rad <= vat.dai(address(this)), "Vow/insufficient-surplus");
    require(rad <= sub(sub(vat.sin(address(this)), Sin), Ash), "Vow/insufficient-debt");
    vat.heal(rad);
}

function flop() external returns (uint id) {
    require(sump <= sub(sub(vat.sin(address(this)), Sin), Ash), "Vow/insufficient-debt");
    require(vat.dai(address(this)) == 0, "Vow/surplus-not-zero");
    Ash = add(Ash, sump);
    id = flopper.kick(address(this), dump, sump);
}

function flap() external returns (uint id) {
    require(vat.dai(address(this)) >= add(add(vat.sin(address(this)), bump), hump), "Vow/insufficient-surplus");
    require(sub(sub(vat.sin(address(this)), Sin), Ash) == 0, "Vow/debt-not-zero");
    id = flapper.kick(bump, 0);
}
```

## What These Functions Do

`Vow` manages system surplus and system debt.

`flog(...)` releases queued debt after a delay.

`heal(...)` cancels surplus and debt against each other.

`flop(...)` starts a debt auction.

`flap(...)` starts a surplus auction.

## Important Logic Notes

### Flog

```solidity
require(add(era, wait) <= now, "Vow/wait-not-finished");
Sin = sub(Sin, sin[era]);
sin[era] = 0;
```

This moves queued debt into active debt after the waiting period.

### Heal

```solidity
vat.heal(rad);
```

This cancels system surplus and system debt.

### Flop

```solidity
id = flopper.kick(address(this), dump, sump);
```

This starts a debt auction when the system has unpaid debt.

### Flap

```solidity
id = flapper.kick(bump, 0);
```

This starts a surplus auction when the system has enough extra surplus.
