# Function Review: Clip.take(...)

## Function Code

```solidity
function take(
    uint256 id,
    uint256 amt,
    uint256 max,
    address who,
    bytes calldata data
) external lock isStopped(3) {
    address usr = sales[id].usr;
    uint96  tic = sales[id].tic;

    require(usr != address(0), "Clipper/not-running-auction");

    uint256 price;
    {
        bool done;
        (done, price) = status(tic, sales[id].top);
        require(!done, "Clipper/needs-reset");
    }

    require(max >= price, "Clipper/too-expensive");

    uint256 lot = sales[id].lot;
    uint256 tab = sales[id].tab;
    uint256 owe;

    uint256 slice = min(lot, amt);
    owe = mul(slice, price);

    if (owe > tab) {
        owe = tab;
        slice = owe / price;
    }

    tab = tab - owe;
    lot = lot - slice;

    vat.flux(ilk, address(this), who, slice);

    if (data.length > 0 && who != address(vat) && who != address(dog)) {
        ClipperCallee(who).clipperCall(msg.sender, owe, slice, data);
    }

    vat.move(msg.sender, vow, owe);
    dog.digs(ilk, lot == 0 ? tab + owe : owe);
}
```

Note:

```text
This snippet is shortened from the original function to focus on the core purchase flow.
```

## What This Function Does

`Clip.take(...)` lets a buyer purchase collateral from a liquidation auction.

It checks price, calculates how much collateral is bought, transfers collateral to the buyer, collects DAI, and updates liquidation debt.

## Important Logic Notes

### Price Check

```solidity
require(max >= price, "Clipper/too-expensive");
```

The buyer defines the maximum acceptable price.

### Purchase Calculation

```solidity
uint256 slice = min(lot, amt);
owe = mul(slice, price);
```

This calculates how much collateral is bought and how much DAI is owed.

### Send Collateral

```solidity
vat.flux(ilk, address(this), who, slice);
```

This transfers internal collateral from the auction to the buyer.

### Collect DAI

```solidity
vat.move(msg.sender, vow, owe);
```

This transfers internal dai from the buyer to the system surplus/debt accounting contract.

### Optional Callback

```solidity
ClipperCallee(who).clipperCall(msg.sender, owe, slice, data);
```

If `data` is provided, the buyer contract can execute callback logic during the auction purchase.
