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

## Invariants

### Main Invariant 1

```text
Auction collateral must not be sold below the buyer's max acceptable price.
```

### Main Invariant 2

```text
Collateral sent to the buyer must match the DAI paid.
```

### Main Invariant 3

```text
Auction debt and collateral balances must be updated consistently after purchase.
```

## Additional Invariants

### Additional Invariant 1

```text
Only active auctions can be taken.
```

### Additional Invariant 2

```text
Unsafe callback targets must not be allowed.
```

### Additional Invariant 3

```text
The auction must not allow invalid partial purchases.
```
