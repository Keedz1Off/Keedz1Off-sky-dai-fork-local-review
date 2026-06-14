# Function Review: Dog.bark(...)

## Function Code

```solidity
function bark(bytes32 ilk, address urn, address kpr) external returns (uint256 id) {
    require(live == 1, "Dog/not-live");

    (uint256 ink, uint256 art) = vat.urns(ilk, urn);
    Ilk memory milk = ilks[ilk];
    uint256 dart;
    uint256 rate;
    uint256 dust;
    {
        uint256 spot;
        (,rate, spot,, dust) = vat.ilks(ilk);
        require(spot > 0 && mul(ink, spot) < mul(art, rate), "Dog/not-unsafe");

        require(Hole > Dirt && milk.hole > milk.dirt, "Dog/liquidation-limit-hit");
        uint256 room = min(Hole - Dirt, milk.hole - milk.dirt);

        dart = min(art, mul(room, WAD) / rate / milk.chop);

        if (art > dart) {
            if (mul(art - dart, rate) < dust) {
                dart = art;
            } else {
                require(mul(dart, rate) >= dust, "Dog/dusty-auction-from-partial-liquidation");
            }
        }
    }

    uint256 dink = mul(ink, dart) / art;

    require(dink > 0, "Dog/null-auction");
    require(dart <= 2**255 && dink <= 2**255, "Dog/overflow");

    vat.grab(
        ilk, urn, milk.clip, address(vow), -int256(dink), -int256(dart)
    );

    uint256 due = mul(dart, rate);
    vow.fess(due);

    uint256 tab = mul(due, milk.chop) / WAD;
    Dirt = add(Dirt, tab);
    ilks[ilk].dirt = add(milk.dirt, tab);

    id = ClipperLike(milk.clip).kick({
        tab: tab,
        lot: dink,
        usr: urn,
        kpr: kpr
    });
}
```

## What This Function Does

`Dog.bark(...)` starts liquidation for an unsafe vault.

It checks vault safety, calculates how much debt/collateral to liquidate, confiscates it with `vat.grab(...)`, records bad debt, and starts a `Clipper` auction.

## Important Logic Notes

### Unsafe Vault Check

```solidity
require(spot > 0 && mul(ink, spot) < mul(art, rate), "Dog/not-unsafe");
```

This checks whether the vault is unsafe and can be liquidated.

Simple meaning:

```text
collateral value < debt value
```

### Liquidation Room

```solidity
uint256 room = min(Hole - Dirt, milk.hole - milk.dirt);
```

This calculates how much liquidation capacity is still available.

### Confiscate Vault Position

```solidity
vat.grab(ilk, urn, milk.clip, address(vow), -int256(dink), -int256(dart));
```

This removes collateral and debt from the unsafe vault and moves them into liquidation accounting.

### Start Auction

```solidity
id = ClipperLike(milk.clip).kick({
    tab: tab,
    lot: dink,
    usr: urn,
    kpr: kpr
});
```

This starts the collateral auction.
